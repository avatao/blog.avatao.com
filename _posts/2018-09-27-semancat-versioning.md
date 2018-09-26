---
layout: post
title: Semancat versioning 
author: kristoftoth
author_name: "Kristóf Tóth"
author_web: ""
featured-img: semancat_versioning 
---

Tackling the versioning pains of a greenfield project with cats.

<!--excerpt-->

----

New projects can force us, developers to face certain challenges that we won't even have to think about when working on an already existing codebase.
These include stuff like "how are we going to ship our code to customers/clients?" or coming up with a way to distinguish between versions.

We started working on a thing called "Tutorial Framework" here at Avatao back in 2017.
This is the framework behind the tutorial challenges on our platform with the "Avatao Tutorials" header up top.

Esentially this is a framework developed *for* our challenge developers that provides a robust messaging architecture between frontend and backend, and a set of commonly used components with the APIs to use them.
These include things like keeping track of user progression, writing instructions to our chat-like messaging component, or executing commands in a terminal visible on the webpage.

TLDR; the targeted group of users are developers working on tutorials for Avatao.
Once we've decided how to ship our framework (via Docker) we got to the versioning bit.

## Main contenders

The most commonly used versioning systems are: 
1. Semver -- semantic versioning
2. Calver -- calendar versioning

You have probably seen both of them already, event if you haven't really gave thought to their advantages and disadvantages.

### Semver

Semantic versioning is the practice of distinguishing release tags with 3 main components: *major*, *minor* and *patch*.
An example would be `v3.1.7` -- `vmajor.minor.patch`.

Each part in the release tag is there for a different purpose, and incrementing them could mean different things:
1. `major` -- this release contains **breaking changes**, i.e. code using `v2.7.8` may break with `v3.0.0`
2. `minor` -- addition of new features in a **backwards compatible** manner
3. `patch` -- a release without new features, mostly bugfixes and such, all being **backwards compatible**

#### The good

The most appealing feature of semver is the promise of solving *dependency hell* -- the nature of releases is clearly communicated through their version numbers, making it easier to manage your packages.
It is a hotly debated topic among developers whether semver actually delivers on it's promise or not, but it is probably safe to assume that it does more in this department than other versioning schemes, i.e. it encourages developers *not to* break public APIs and tries to signal it loudly when they do.

#### The bad

It is not always clear what a *breaking change* exactly *is*.
Sometimes things are not so black or white, but all the shades in between, and this applies for software as well.

The line between a *feature* or a *bug* could get blurry sometimes and this means that the most trivial bugfix could break client code.
It is really hard to tell if your code depends on any *undocumented behavior* in a framework or library and if it **does**, then there is no telling whether a `patch` or `minor` will break your code or not.

In case you are not aware of this stuff, you might fall for semver's *false promise* of never ever having to fear breaking changes and the ability to update your packages blindly as long as there are no `major` bumps.
What I mean by all that is the fact that semver *can* encourage you to pretend like `minor`s and `patch`es could never break anything, while that is not always the case.

Semver is good, but in no way perfect, as it is not always possible to compress this amount of information into 3 numbers.

### Calver

Calendar versioning is not **a** versioning system per se, but rather a *family* of versioning schemes.
It has several flavours, which all share that the versions are based on the *release calendar* instead of arbitrary numbers.

Most of the time this means that a project has to commit **not to release breaking changes** for a certain time period, so it is clear how long certain APIs are supported for.

For example the GNU/Linux disribution Ubuntu uses a flavour of calver, and they've specified that regular releases are supported for 9 months and LTS (long term support) versions of Ubuntu are supported for 5 years.
This means that in that time period they won't break anything (on purpose at least).

Citing [their wiki](https://wiki.ubuntu.com/TimeBasedReleases), this means that they only update packages for specific types of changes:
- Fixes for security vulnerabilities
- Other high-impact bug fixes, for example those which cause data loss
- Very conservative, unintrusive bug fixes with substantial benefit and very low risk

#### The good

Calver is really well suited for projects with a stable release cycle, or for software products aimed at people - not necessarily technical - instead of code.

In the case of an operating system - or any large software system in general - package developers can have a high level of trust for the OS (API) that their code will keep working for a certain amount of time.

JetBrain's PyCharm IDE uses calver as well, but it won't really break it's API in a traditional sense: 
most of the time you only depend on your IDE in the sense you know how to use it, but if they change somthing your production cluster won't go up in flames.

It is also really easy to tell when a certain version was released, which is always nice (and useful in some cases).

#### The bad

It can be hard to tell if a release contains breaking changes, especially when there are lots of them.

This usually means that calver is less suited for projects whose release calendar is a bit more chaotic, with lots of releases - possibly several times a week/month - because it's going to get hard to tell which releases are safe to upgrade to.

In case it is clearly visible that this is going to be an issue, calver is often combined with other versioning schemes (and this is kind of what we did in the end, but more on that later..).

Dates are also flat: in semver you have a hierarchy encoded into release numbers, but with calver you have no such thing.

## Semancat

Our situation was pretty unique:
- we knew we are going to have lots of releases - especially early on (semver+)
- and we want to communicate breaking changes loudly (semver+)
- but new features are aimed at customers as well, not just challenge developers (calver+)
- it should be visible how much *time* passed between two releases, to give challenge developers a rough estimate on the amount of new stuff (older challenges are often updated with new features) (calver+)
- we cannot really afford committing to an API for too long, because our release cycle could often be overwritten by business requirements (semver+)

Our first impression was that these requirements seem impossible to fulfill with semver or calver alone.
We develop our framework so that code can painlessly depend on it, but we must keep in mind that it is humans who write that code and in the end they write it for our customers.

This is why we came up with a hybrid versioning system, a bit of both worlds.

The framework versions follow a `major-YYYYMMDD` pattern in their versionings, like `egyptianmau-20180509` or `mainecoon-20180720`.
Note that the `major` is always named after a breed of cat 🐈.

Every version contains a `YYYYMMDD` timestamp of the day it was released on, as in some flavours of calver. 
Similar to semver, a change in the `major` indicates breaking changes.

For example code depending on `bombay-20180618` shouldn't break with `bombay-20180703`, so backwards compatibility between `major`s is respected.

We have found that this system comes with the advantages we need, and disadvantages that we can deal with:
- we cannot release more versions on the same day
- versions do not distinguish feature additons and bugfixes

The first issue could easily be solved by postfixing the versions with hours (HH) or incrementing numbers, but we thought that this won't be that big of a deal and we should not complicate things further.

The second issue is mitigated by the fact that we have an internal release announcement mailing list explaining the contents and purpose of each release.

All in all we're pretty statisfied with this system.
It is quite fun while still being functional:

The best parts are the "ritual" of naming a new `major` release (I found a book at home describing like 50 breeds of cats, usually we choose a cat from there), or when someone says something like "I just started working on `ocicat`" in our office.

Cats are awesome for team morale 🐱.

## Final notes

I think that what you can take away from all this is that you should not be afraid of coming up with your own stuff.
It certainly is possible to do that without reinventing the wheel (i.e. combining wheels into new ones).

The stuff we did is not even that unique (well, maybe the part with the cats is), there are projects using similar schemes.

It is also important to note, that no versioning system is better than the developers using them.
They can help a lot, but they are no snake oil either.

There are tons of npm packages using semver, which does not seem keep them from breaking their APIs every other week.
There are 30 year old libraries written in C with no breaking changes whatsoever and geat APIs, all without using any kind of formal versioning system.

Your project, your choice.
