---
layout: post
title: Not so smart pointers
author: richard
author_name: "Richard Raciborski"
author_web: ""
featured-img: SmartPointers
seo_tags: "C++; smart pointers; modern C++; secure coding in C; C++ security"
---

Even though modern C++ *( the standard since C++11)* has made programming in this language much more secure, it also introduced new vulnerabilities hidden under its layers of abstractions. In C and older versions of C++, the concept of pointers wasn't easy to grasp for beginners. You had to worry about null dereference, dangling pointers, deallocation, etc. However, the Middle Ages are over, we have smart pointers now.

<!--excerpt-->

----

## What are they exactly?
We distinguish two types, *unique* and *shared pointers*. The former one is a simple wrapper around raw pointers that **owns the pointer exclusively**, and frees it when it goes out of scope. The latter is much more interesting... There are several cases when you need to refer to an object from multiple places. Without smart pointers, this problem would impose several limitations on the structure of your code, but `shared_ptr` is **capable of tracking the pointer's lifetime** since *RAII* only works for stack-allocated objects. But how does `shared_ptr` know, when to free a pointer exactly?

## Reference counting
Internally, they use a technique called reference counting, which is a type of *garbage collection*. It works as follows:
  * You initialize a `shared_ptr` from a raw pointer, which associates it with a counter that is set to `1`.
  * If you assign this object to another one, the counter is incremented by one.
  * If one of the objects goes out of scope, the counter is decremented by one.
  * When the counter reaches `0`, then the raw pointer is deallocated.

Unfortunately, this method only works perfectly until a *reference cycle* occurs. It can happen when you allocate such a class/struct that has a member of the same pointer type. Before describing this in detail, let's see a very simple example. The following is a doubly linked list with a limited feature set:

```cpp
template<typename T> struct node {
    T data;
    shared_ptr<node<T>> prev, next;
};

template<typename T> class list {
  private:
    shared_ptr<node<T>> head;
  public:
    void push(const T& elem) {
      shared_ptr<node<T>> new_node = make_shared<node<T>>();
      
      new_node->data = elem;
      new_node->next = head;
      if(head != nullptr) head->prev = new_node;
      head = new_node;
    }
    void print() const {
      shared_ptr<node<T>> ptr = head;
      
      while(ptr) {
        cout << ptr->data << endl;
        ptr = ptr->next;
      }
    }
};
```
If you created a list with at least 2 elements, it would leak memory because of the aforementioned problem. You can easily understand why this happens if you count the references manually. This table below demonstrates the process of pushing two integers *(0,1)* on the list, then destroying it:

| ptr |               expression | refc |                                                                                                 explanation |
| --- | ------------------------ | ---- | ----------------------------------------------------------------------------------------------------------- |
|   0 | `make_shared<node<T>>()` |    1 | Allocating new node for '0'. No further actions are required, because the list is empty.                    |
|   1 | `make_shared<node<T>>()` |    1 | Allocating new node for '1'.                                                                                |
|   0 |  `new_node->next = head` |    2 | Appending '0' to '1'. There is no deep copy, only the reference count of head is incremented.               |
|   1 |  `head->prev = new_node` |    2 | Prepending '1' to '0'. Same happens as above.                                                               |
|   1 |                `~node()` |    1 | Destroying `head`. The reference count is decremented, but it's still greater than `0`, so nothing happens. |
|   0 |                `~node()` |    1 | Destroying `head->next`. Same happens as above.                                                             |

As you can see, neither of them has been released, they will stay in the memory until the process halts and kernel takes care of it.

## Preventing these cycles

Whether it is possible or not depends on the data structure. The key factor is a well-defined ownership model. In C++, there is another type of smart pointer called `weak_ptr`, which holds a non-owning reference to an object, and can be constructed **only** from a `shared_ptr`. It's a perfectly suitable solution to resolve the cycle problem in our linked list and can be also applied anywhere else, where you can define a *master-slave* relationship between objects. However, things get more complicated if you have a *master-master* one. If the objects own each other, they can only release the other one at the same time, which is impossible, so you have to implement an additional cycle detection algorithm to delete them manually.

## Conclusion
Smart pointers form an essential addition to *STL*, but you have to be careful with them. If they are left unnoticed, it may be possible to launch a *DoS attack* where the environment runs out of memory. The linked list example is very trivial (not in *Rust*, if you want to eliminate unsafe blocks, you will have a hard time). Think of dynamic languages for instance, where you can assign any type of value to an element in associative arrays... You wouldn’t be able to find a straightforward solution in C++ to implement a dynamic data type without dealing with circular references. Moreover, there are numerous additional situations where a garbage collector is more adequate. Always choose the right tool for the job.


