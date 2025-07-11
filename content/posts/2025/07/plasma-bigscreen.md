---
title: "Diving into Plasma Bigscreen"
date: 2025-07-10T19:00:00Z
draft: false
tags: [kde]
---

I have been a long time [Plasma Mobile](https://plasma-mobile.org) contributor, but I have always had a keen interest in having Linux on my TV! I have noticed that in the past few months, the [Plasma Bigscreen](https://plasma-bigscreen.org/) project has had some interest from people wanting to contribute, but there have not been any active KDE developers working on the project. Since I have some time off school (having just graduated university), I decided to take a swing at improving the project for a week.

<img src="/images/blog/2025/07/plasma-bigscreen/real-homescreen.png" width="400px" />

<img src="/images/blog/2025/07/plasma-bigscreen/homescreen.png" width="400px" />

# Background

[Plasma Bigscreen](https://plasma-bigscreen.org/) is a Plasma-based shell (desktop environment) for TVs and other large displays. It is designed to be used with arrow navigation using remotes or controllers.

I have not been involved with the project in the past so its history is a bit murky to me. From what I know, it was originally developed with [Mycroft](https://en.wikipedia.org/wiki/Mycroft_(software)) in mind, which was a open source virtual assistant. They had even developed [hardware](https://www.kickstarter.com/projects/aiforeveryone/mycroft-mark-ii-the-open-voice-assistant) for it, but unfortunately, the company shut down in recent years. The work by the developers at that time appears to have been sponsored by [Blue Systems](https://blue-systems.com/).

Plasma Bigscreen itself emerged around 2020 and was designed as a "Plasma shell", in a similar way to Plasma Desktop and Plasma Mobile. Back when development was active, it provided a TV friendly launcher to launch Linux apps, and even had its own "mini-apps", known as Mycroft Skills. These could be downloaded from the [KDE Store](https://store.kde.org/browse?cat=608&ord=latest). A TV-friendly [web browser](https://invent.kde.org/plasma/aura-browser) and [media player](https://invent.kde.org/plasma/plank-player) were also developed for the project. The project itself was released in the Plasma 5 release cycle, but got dropped with Plasma 6 in 2024 because it was not ported in time for the megarelease.

About a year ago, the code was the project was [ported](https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/92) to Plasma 6 (and Qt 6), but has not yet recieved a release since being removed from the Plasma release schedule. 

# Stepping in

A few months ago, my friend [Seshan](https://seshan.xyz/) started doing some work to get some KDE experience, and opened a few merge requests against the Plasma Bigscreen [shell repository](https://invent.kde.org/plasma/plasma-bigscreen/). I noticed that there had basically been no activity on the repository since the initial Qt6/Plasma6 port, and the [matrix channel](https://matrix.to/#/%23plasma-bigscreen:kde.org) had no active developers. I sensed an opportunity...

### Housekeeping

I started with some housekeeping work with the repository. I added a README, and a REUSE license checker to the CI. I then ported the QML library to be a declarative plugin, and removed a bunch of abandonded code folders that were not used anywhere in the codebase.

Merge requests:
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/111
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/122

## UI

At this point before my work, the shell UI looked like this:

TODO pictures

I was digging around some old Breeze Ocean [mockups](https://www.figma.com/design/gjuIy1rxU9xHkaJXM0pasK/old-ocean?node-id=1336-7194&t=ZRqHhUEJFR269CHt-0) and stumbled across some Bigscreen mockups by [Manuel](https://invent.kde.org/mdelafuente). It seems the original Bigscreen UI did try to follow it, but did not quite get there. I felt inspired to fully complete implementing them.

### Homescreen

I first worked on the homescreen UI. I flattened the layout to reduce visual complexity on the page, removing panel backgrounds and shadows where possible, while adding tooltips for the indicators. I then added an "expanded clock" view for when the user is at the top of application categories (based on the mockups), which shrinks when the user goes down the view. I ported the application lists to use ListView and delegate caching rather than having all elements having their coordinates positioned manually to improve performance. The background now also blurs when it is not the main focus of the UI.

<video width="400" controls>
    <source src="/images/blog/2025/07/plasma-bigscreen/homescreen.webm" type="video/webm">
</video>


I also added a search view based on KRunner. This allows users to search for the applications they need without needing to manually scroll through the entire application list.

<img src="/images/blog/2025/07/plasma-bigscreen/search.webp" width="300px" />

<video width="400" controls>
    <source src="/images/blog/2025/07/plasma-bigscreen/search.webm" type="video/webm">
</video>

Merge requests:
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/114
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/116
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/118
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/119
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/124
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/126

### Settings

I redesigned the system settings view to have a sidebar with categories, with a simple two-pane look.

The settings modules (KCMs) had a lot of hardcoded UI elements and layouts. I decided to make a small component library to build TV focused UIs (that still look Breeze like), and ported all of the settings modules to it. I moved away from horizontal layouts to vertical layouts for content, and put a heavier emphasis on sidebars for interacting with individual delegates. I think it looks pretty nice:

<video width="400" controls>
    <source src="/images/blog/2025/07/plasma-bigscreen/settings.webm" type="video/webm">
</video>

I ported settings modules to my controls library, and also fixed some issues:
- Display KCM (rewritten with libkscreen backend, as it was otherwise completely broken)
- Sound KCM (ported to new UI)
- KDE Connect KCM (ported to new UI, fixed some state issues)
- Bigscreen KCM (ported to new UI, fix shortcuts, fixed timezone selection)
- Wi-Fi KCM (ported to new UI)

<img src="/images/blog/2025/07/plasma-bigscreen/settings-bigscreen.png" width="300px" />
<img src="/images/blog/2025/07/plasma-bigscreen/settings-bigscreen-sidebar.png" width="300px" />
<img src="/images/blog/2025/07/plasma-bigscreen/settings-audio.png" width="300px" />

Merge requests:
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/115 (rework settings app)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/129 (controls library)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/130 (kdeconnect)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/131 (bigscreen settings)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/132 (audio)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/139 (don't sort settings modules with bogosort)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/145 (displays)

### Startup feedback

The UI feedback for starting an application was broken, so I decided to overhaul it to be something similar to what we have on mobile:

<img src="/images/blog/2025/07/plasma-bigscreen/startup-feedback.webp" width="300px" />

Merge request:
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/125

## Envmanager

I wrote [envmanager](https://invent.kde.org/plasma/plasma-mobile/-/tree/master/envmanager) as a program in Plasma Mobile that manages shell specific configuration we need in services such as KWin. This avoids the need for distros to ship custom configs to set certain settings that the shell needs. I recently changed Plasma Mobile to use config overlays in order to achieve this, with more details can be found in my [other blog post](/posts/2025/05/plasma-sprint/#envmanager).

Merge requests:
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/109 (import from Plasma Mobile, by [Seshan](https://seshan.xyz))
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/113 (port to overlays)

# Trying it out

In order to try it out (on a TV for realsies, not just on my workstation), I used a Raspberry Pi 5. I flashed [postmarketOS](https://postmarketos.org/) onto it, and then manually compiled and installed the [Plasma Bigscreen](https://invent.kde.org/plasma/plasma-bigscreen/) shell. 

**Note:** If you are trying to build and install on postmarketOS yourself, be sure install the dependencies and pass the build flags in this manifest as they are a necessity: https://gitlab.postmarketos.org/postmarketOS/pmaports/-/blob/ee4de8c6702a113914d6ed899c88cb9411e75427/packages/plasma-bigscreen/APKBUILD#L57

As a user you can otherwise install `plasma-bigscreen` from the [nightly repo](), or use the [AUR package](https://aur.archlinux.org/packages/plasma-bigscreen-git) on Arch (though I haven't tried it).

<img src="/images/blog/2025/07/plasma-bigscreen/real-homescreen2.png" width="400px" />

<img src="/images/blog/2025/07/plasma-bigscreen/rpi.png" width="300px" />

*It's dangling off the HDMI cable, I know*

## Applications

In its heyday, Plasma Bigscreen relied on "Mycroft Skills" to provide some media applications such as YouTube and SoundCloud. We do not have that anymore, so I tried out some other Linux applications.

These some of the ones I tried from [Flathub](https://flathub.org/):
- [Kodi](https://flathub.org/apps/tv.kodi.Kodi) - Works well with arrow navigation, allows you to manage a local digital library of content
- [VacuumTube](https://flathub.org/apps/rocks.shy.VacuumTube) - Provides the YouTube TV web UI wrapped in an application, works well with arrow navigation
- [Jellyfin](https://flathub.org/apps/com.github.iwalton3.jellyfin-media-player) - I don't have a Jellyfin server; it launches fine but I don't think it supports arrow navigation
- [SuperTux](https://flathub.org/apps/org.supertuxproject.SuperTux) - Fun game!
- [SuperTuxKart](https://flathub.org/apps/net.supertuxkart.SuperTuxKart) - It ran somewhat poorly on the Pi, but playing it with a controller was quite nice!

Of course, we also have KDE applications designed for TV:
- [Aura browser](https://invent.kde.org/plasma/aura-browser)
- [Plank media player](https://invent.kde.org/plasma/plank-player)

## Controller support

There is a repository called [plasma-remotecontrollers](https://invent.kde.org/plasma-bigscreen/plasma-remotecontrollers), which contains a daemon that is able to take both game controllers (ex. XBox) and TV remotes (over CEC on HDMI) and map them to system. It also has a settings module to configure the shortcuts.

I was able to successfully test having an XBox controller connected (with the daemon online), an having it map the arrow buttons to arrow keys on the system. I wasn't able to however test the [CEC](https://en.wikipedia.org/wiki/Consumer_Electronics_Control) support, which would allow buttons on TV remotes (over HDMI) map to arrow keys.

## Other contributors

[Seshan](https://invent.kde.org/seshpenguin/) and [User8395](https://invent.kde.org/usereightthreeninefive) have also been contributing to the project here and there in the past few weeks, here are some highlights:
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/127 - (Seshan - Add a UVC viewer app)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/117 - (Seshan - Add an "app" to swap session directly from Plasma Desktop)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/123 - (Seshan - Make Meta key go to home)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/106 - (Seshan - Improve the favorites selector)
- https://invent.kde.org/plasma/plasma-bigscreen/-/merge_requests/147 - (User8395 - Create a new bluetooth KCM)

# TODOs

There are still a lot of TODOs that need to be resolved in the project.

### Input

Input for sure is one thing. There isn't a virtual keyboard to input text with that supports arrow navigation. This is something planned for [Plasma Keyboard](https://invent.kde.org/plasma/plasma-keyboard/-/issues/10) however, keep tuned! [plasma-remotecontrollers](https://invent.kde.org/plasma-bigscreen/plasma-remotecontrollers)'s settings module is also not yet properly ported and tested on Bigscreen.

So... it's probably best to still use a bluetooth keyboard and mouse for now.

### Application design

We do not have any framework to design TV-based UIs in KDE. [Aura browser](https://invent.kde.org/plasma/aura-browser) and [Plank](https://invent.kde.org/plasma/plank-player) both use Qt Quick Controls and Kirigami, but have a lot of hardcoding and custom controls in order to be usable on a TV. I do have a few TV focused components for building settings modules, but that is a very narrow set of controls.

### Goals

What are the usecases we want to achieve with a TV focused desktop environment? Do we need to also pursue making frontends for various media services? There isn't a clear direction for the project, beyond making it a working desktop environment (though there aren't really many contributors to drive this forward).

# Overall

I am fairly happy with the work I was able to do on Bigscreen last month, I am also hoping that it can rejoin the Plasma 6.5 release! I have since returned to working on Plasma Mobile (due to having limited time as a volunteer contributor), but I can still step in and help review merge requests and guide new contributors to the project.

Come visit us in the Bigscreen matrix group [#plasma-bigscreen:kde.org](https://matrix.to/#/%23plasma-bigscreen:kde.org)!
