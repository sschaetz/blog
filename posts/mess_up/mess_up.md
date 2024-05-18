---
title: Mess Up
description: Musings on messing up as much as possible
date: 2023-05-09
tags:
  - another tag
layout: layouts/post.njk
---

Recently I came across the concept of "messing up as much as possible" so one can learn as much as possible.
I first encountered it when following and reading about SpaceX Starship April 20 2023 orbital test flight.

--excerpt--

Quite a few things went wrong during that test flight:

- three engines did not work during liftoff
- more engines failed during flight
- the ability to steer the rocket was lost, with vehicle entering a spin
- autonomous flight termination system delayed self-destruction by 40 seconds

But I suspect the engineers were able to gather a lot of data to improve their designs, systems, and integrations and to have a more successful test flight next time.


In an episode of the "SmarterEveryDay" YouTube channel, glass sculptor and artist Cal Breed creates Prince Rupert's drops and explodes them in
molten glass in a vessel made from glass. In creating those fascinating objects, the team around the artist has to execute a well coordinated choreography
of heating, cooling, transporting, cutting, pouring, dropping and breaking of glass.

<figure>
    <img src="prince-rupert-drop-exploding.jpg" alt="Expressionist oil painting of a glass prince rupert's drop exploding by DALL·E 2 (top right and bottom).">
    <figcaption>A Prince Rupert's drop exploding by DALL·E 2.
    </figcaption>
</figure>

Cal the artists says in the video "Typically, if I screw up I want to screw up as many ways as I can in one go. Let's mess up as much as possible so we can figure out as much as possible.".

This concept can also be found in software development. One example that we take a bit for granted are compilers: modern compilers are often able to identify _all the things_ wrong with an
input program. They usually don't just pinpoint the first error they come across, but also highlight other existing problems.
To some degree, they can make assumptions about what the user intended to do and continue on. But this breaks down at some point, for example if curly braces are not matched in C/C++.

I am wondering how I can leverage the strategy of "messing up as much as possible" to my advantage. I often find myself bogged down in details, but perhaps sometimes it's better to sidestep them.
For example,
let's say I'm implementing a large feature and I realize at some point the way I provide the path to a file in the feature does not work. If I reach a point like this I should decide:
is it worth it to fix this now or should I put a short-cut in-place (like hard-coding a file or copying a file manually to a location that will not work in production) and proceed with the feature's development.
Alternatively, I could choose to invest the time now to avoid the shortcut.

My take on it is: it often makes sense to take shortcuts to make progress and identify all potential issues during feature development, instead of being hit by each
problem one at a time. This strategy can be especially helpful when defining or designing a feature: being aware of all potential issues before committing to a particular architecture can offer a significant advantage.
It's akin to having a comprehensive understanding of all requirements upfront.

Because after all:
> “Oops is the sound we make when we improve our beliefs and strategies.” <br/>
> _-- Eliezer Yudkowsky_
