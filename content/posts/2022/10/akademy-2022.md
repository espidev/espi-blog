---
title: "Akademy 2022"
date: 2022-10-11T11:26:29+01:00
draft: false
tags: [kde]
---

This year, I had the amazing opportunity to attend Akademy in person (@ Barcelona) for the first time!

For context, I first started contributing to Plasma Mobile in 2020, right around when easily testable hardware (ex. PinePhone) was taking shape. I originally started with some contributions to some applications to learn Qt and C++, but have since then taken more responsibility with tasks from all around the software stack.

# Talks

We first had a welcome event on Friday where attendees got to know each other. It was quite cool to finally be able to match usernames to faces, and finally meet the people I had been working with in-person. 

I also met up with amazing people from the postmarketOS team! I had the chance to see a Fairphone 4 with the OS running Plasma Mobile smoothly, which was amazing.

## Day 1 & 2

I had a great start... I missed the first two hours of talks on the first day because I slept in.

Luckily, my talk, **Plasma Mobile in 2022** with Bhushan was in the afternoon. Unfortunately, Bhushan was unable to come in-person this time around. Hopefully I will be able to meet him next year!

You can see our talk here (at around 5:21:00):

{{< youtube wSlMtf-YGw4 >}}

I attended quite a few interesting talks:

* **Konquering the World: Are We There Yet (Nate Graham)** - The state of Plasma shipping with hardware
* **Full Steam Ahead! (David Edmundson)** - Steam Deck with Plasma
* **Stop Crashing Already! (Harald Sitter)** - Integrating Dr. Konqi with powerful tools for developers to find issues
* **Getting your application ready for KF6 (Nicolas Fella & Alexander Lohnau)**
* **Asahi Linux (Hector Martin)** - Information about the Asahi Linux project, how it came about
* **Push Notifications - Infrastructure (not just) for Plasma Mobile (Volker Krause)** - Status of building the framework to have push notifications on Plasma (Mobile!)
* **Fedora, KDE, Kinoite, and Mobile (Neal Gompa)** - Overview of Fedora shipping Plasma 

... and there were many other cool shorter talks as well!

There were also people from Pine64 and Manjaro there that I met. It was really cool hearing about Pine64's future plans for RISC V (coming soon!), as well as seeing some of the devices that Manjaro had running Plasma Mobile on (portable game console, mini tablet/netbook?).

# Meetings/BoFs

After the weekend of talks, we had several days where rooms at the university we were at were available for BoFs.

BoFs, or "Birds of Feather" meetings are ways to gather people with similar interests in the same room, and discuss specific topics and plans.

There were quite a few BoF sessions, of which I attended a few:

## Plasma Mobile BoF

This was my first time attending a BoF, as well as hosting one. We unfortunately had some trouble with audio, which made it hard to communicate with online attendees.

However, we did discuss the following topics:

* Helping Bhushan do Plasma Mobile Gear releases since he does them himself at the moment
* Moving some Plasma Mobile applications to KDE Gear due to limited changes
* How to proceed with QtFeedback (vibrations stack) for Qt 6 as it is unmaintained
* Possibility of switching to `feedbackd` (by Purism) from `hfd-service` instead
* Plan is one last release of Plasma Mobile with KF 5 (5.27), before branches are made for Plasma 6
* No large changes anticipated for Plasma Mobile 6
* SHIFTphones and Fairphone can run pmOS with Plasma Mobile, perhaps we can open communication channels with them?
* Manjaro was working with 2 tablet (?) vendors, they have positive reception and had a few suggestions

## KDE PIM BoF

KDE PIM (Personal Information Management) refers to the KDE stack for applications like KMail, KOrganizer, Kalendar, etc.

Improving Akonadi for mobile devices was discussed, including the possibility to try using SQLite by default, which may improve resource consumption.

I had started working on a mail application called Raven for Plasma Mobile recently, and so it was also discussed how to better share code between it and other applications that use the Akonadi stack.

## Plasma Ink BoF

I attended a BoF about an upcoming project, Plasma Ink, which brings Plasma to e-ink devices. They can be useful for reading, taking notes, as well as simply being an enjoyable interface to write content.

This platform however needs special consideration for the way content is rendered, since e-ink devices typically have slow-refresh screens, and so animations as well as colours need to be adjusted to be as light as possible.

Pine64 has been developing the PineNote which can run Plasma Ink theoretically, I had the chance to try one and was quite impressed at the responsiveness for pen notetaking.

While I likely will not be very involved with the project since I am preoccupied with Plasma Mobile, I hope it continues forward!

## Debugging session with Aleix

We have had a severe regression for several months now with the lockscreen, which crashes frequently when waking from suspend: https://invent.kde.org/teams/plasma-mobile/issues/-/issues/156

I sat down with Aleix to really focus on trying to fix this issue, since he knows about KWin for more than me, though his PinePhone had not been working and so he couldn't replicate it previously.

After hours of jumping around git commits of `kscreenlocker` (compiling it on device is painful), we realized that the issue was elsewhere in the stack. There is some sort of Wayland registry failure that occurs when `kscreenlocker` is started immediately after waking from suspend, which causes it to crash. We unfortunately did not have enough time to pinpoint the issue, but we do have some ways to move forward with investigating in the future. We suspect it may be related to when KWaylandServer was merged into KWin.

## Convergent Forms BoF

The last BoF I attended was about convergent forms, and how we can design forms that use a single codebase, but have designs that work for both mobile and desktop.

Currently, we have special components in `kirigami-addons` for mobile, but are not necessarily great on desktop.

What was decided was to create a new "FormLayout" component in Kirigami, which can take a set of instructions to build the form, and generates the necessary components to display properly on whichever platform it is running on.

# Closing

Overall, I had an amazing time at Akademy this year, and hope to attend more in the future!

<img src="/images/blog/2022/10/konqi-calling.png" width=300px/>
