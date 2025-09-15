---
title: "Akademy 2025"
date: 2025-09-14T19:00:00Z
draft: false
tags: [kde]
---

I was able to attend the talks at Akademy this year in Berlin! The last time I attended Akademy in person was in 2022, so it was really nice being able to come back and meet everyone again. 

I was unfortunately not able to attend BoFs (development meetings) due to having to leave early. I did attend some meetings a few months earlier however, you can read more in my [Plasma sprint](/posts/2025/05/plasma-sprint/) recap post.

<img src="/images/blog/2025/09/IMG20250907203710.jpg" width="300px" />

# Talks

Akademy runs with two concurrent tracks of talks, and so sometimes there were two talks at the same that I both wanted to attend, I had a hard time deciding! Here are some of the ones I attended:

### KDE Linux: Banana Growth Cycle

Harald released KDE Linux Alpha was to the public during the talk! I hadn't followed the project super closely, but it was awesome getting up to speed learning about the state of the project and the inner workings of how the distribution works.

### The Role of New Languages in the Future of the Qt Ecosystem

I was introduced to [Qt Bridges](https://www.qt.io/qt-bridges), which is an effort to go beyond Qt bindings for other languages and tightly integrate with them (ex. Rust, Python). Once this is more mature, it will likely be an easy recommendation for others to start learning Qt with, who don't want to use C++!

### KDE Goals - One Year Recap

It was interesting to see all the work that had been done on the KDE Goals so far!

I am actually involved with one of them this time around ("We care about your input") through my work on [plasma-keyboard](https://invent.kde.org/plasma/plasma-keyboard). Blog post likely coming in a few months, once that work is further along!

### Next-Gen Documentation Infrastructure for KDE

KDE's reference API documentation has been a bit of sore spot for me, since it didn't support QML very well. As a result, I usually go manually go through header files instead in the source code to figure out how to use libraries.

The talk went over Nicolas's work on doing the mammoth task of porting all of KDE's API documentation to [QDoc](https://doc.qt.io/qt-6/qdoc-index.html) from Doxygen, which properly supports QML. The new [api.kde.org](https://api.kde.org/) went live, and boy is it such an improvement! It's much easier for me to point new developers to the Kirigami documentation now.

### Fedora KDE Plasma Desktop Edition is Real, Now What?

I personally use Fedora on my workstation and laptops, and so it was cool to get some history about how Plasma on Fedora was revived in the past, and plans for the future. Neal also expressed some interest in a [Plasma Bigscreen](https://plasma-bigscreen.org) spin (similar to the one for [Plasma Mobile](https://fedoraproject.org/spins/kde-mobile)), which could be pretty interesting once it becomes more mature!

### Plasma Mobile Power Management: Reliable Sleep and Wake Ups

Bhushan gave an update on his work power management work across the Plasma stack! He obtained an NLNet grant recently for the project, [detailed on his blog](https://blog.bshah.in/2025/03/22/professional-update-plasma-mobile-ngi0-core-grant/).

# Discussions

I was really happy to meet and discuss with quite a few people during the event. 

I met Bart, Luca, Casey and Pablo from the [postmarketOS](https://postmarketos.org) project! As it is the main platform I test and develop Plasma Mobile with, it was really nice to finally meet some of their developers (I had met Bart and Luca at Akademy 2022)! I also was able to finally meet Florian, who has been collaborating with me in contributing to Plasma Mobile in the past few years!

I met [Dorota](https://dorotac.eu/), who has been working on Wayland input related things for the past few years, and is in the process of pushing through updates to [text-input-v3](https://gitlab.freedesktop.org/wayland/wayland-protocols/-/merge_requests/435), and [Jakob]() who has been working on the KDE side pushing through the input related KDE goals! We discussed some input related topics, which was insightful as I worked on the client side through plasma-keyboard (and my limited Wayland knowledge). 

<img src="/images/blog/2025/09/IMG20250907182233.jpg" width="300px" />
