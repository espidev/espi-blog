---
title: "An update on ProtectionStones"
date: 2024-06-17T12:19:09-05:00
draft: false
tags: [minecraft]
---

> **TLDR**: ProtectionStones is in **maintenance** mode, my interest in maintaining the project for free has waned as I have not hosted a Minecraft server for many years. I will still try my best to upgrade the plugin and fix critical issues when I have the time, but do not expect major updates or consistent support on the Discord channel. I am currently looking for a **new** maintainer, but it seems unlikely I will find one due to a lack of contributors. See the bottom for more details for developers.

In 2018, I found myself in need of a protection plugin for a Minecraft server I was working on. The most popular option I found then was GriefPrevention, however it felt a bit clunky for my needs, as I just wanted something really simple and easy to understand how to use. I had played on a server in the past that used ProtectionStones, which I found really intuitive! However, the plugin was not very maintained, and had not even been properly updated to support UUIDs until that point, causing several security issues.

I decided to try and pick up the project myself!

**GitHub**: [https://github.com/espidev/ProtectionStones](https://github.com/espidev/ProtectionStones)

**Spigot Page**: [https://www.spigotmc.org/resources/protectionstones-updated-for-1-17.61797/](https://www.spigotmc.org/resources/protectionstones-updated-for-1-17.61797/)

# Picking up a project

The first [iteration](https://dev.bukkit.org/projects/protectionstones) of ProtectionStones was released in 2011 by AxelDios. It was rewritten and [revived](https://www.spigotmc.org/resources/protectionstones.10096/) in 2015. It was then [revived]() again in 2017. I continued off of the codebase of the last iteration.

I initially began with trying to fix some of the issues in the plugin (mainly related to UUID support). I posted my fixed version on [spigotmc](https://www.spigotmc.org/resources/protectionstones-updated-for-1-17.61797/), which started gaining traction from others in the Minecraft community. I started getting feature requests on the forum page, which motivated me to start greatly expanding the scope of the plugin. Over time, the amount of features grew, and with it, a larger and larger number of server owners to support. The forum became very hectic and it was hard to manage multiple conversations at once.

In 2020, I decided to have the plugin join the M.O.S.S (Minecraft Open Source Software) Discord [community](https://discord.gg/cqM96tcJRx), opening up a much better support experience. The documentation was also improved on the [GitHub](https://github.com/espidev/ProtectionStones/wiki) page.

Since picking up the project, the feature set has really grown, having allowed countless servers greater flexibility in tweaking the plugin to their own desires! Here are some of the most notable features that I added over the past few years:

- Economy support, ways of getting protection blocks from commands
- Restricting protection blocks from being obtained naturally
- PlaceholderAPI support
- Text-based GUIs for many commands
- Renting, taxes
- Heads support
- Admin commands
- Region merging, chunk-snapping, region groups
- Per-block configs, and a whole host of configurable config options

It does make me very happy to see how much adoption of the plugin there is now, but also incredibly sad that I do not have the bandwidth or incentive to do this much longer...

<img src="https://bstats.org/signatures/bukkit/protectionstones.svg">

# Maintaining a thing for free, alone

ProtectionStones is open-source (GPLv3, as Spigot plugins should be licensed), and will always be free.

I have operated with this guarantee from the beginning. I believe that it is incredibly important to have the source code for this plugin freely accessible and for the community to have the opportunity to modify it for their own uses, being such an important plugin to many servers.

## Can't keep up, while losing interest

However, it is incredibly hard to maintain a project like this indefinitely. I do not get paid for this work, and over time I have had less and less free-time in order to keep up with both support and maintenance. I unfortunately also have not really had any other active contributors to the codebase, and so development work is completely up to me. I have full-time school work, and jobs on the side, as well as other projects that I work on, and so over the years, I have spent less and less time on ProtectionStones development.

For almost a year now, I have really only had the time to respond to inquiries on the M.O.S.S. support Discord server. I got really sick last month and was unable to even tend to that for weeks on end...

I also no longer have my main motivator, which was the Minecraft server I worked on (inactive since 2020). I have not really done server work since then, have just been doing ProtectionStones work independently. This poses several problems. Over time, I have become less and less aware of the newer features that are being added to Minecraft, as well as projects that have appeared in the community (such as Folia). As the userbase has grown, this makes me ever more concerned about the possibility of issuing a faulty update that could have wide ramifications. Providing support over time has also become a very big chore for not much personal satisfaction.

Overall, it has been a wild ride! ProtectionStones had been an important project for me for so long, it is crazy for me to think that almost 6 years have now gone by. When I first picked up the project, I was in still in high school. I am now about to graduate university...

## Why not make it paid?

I have received a few donations here and there, which I greatly appreciate! Some have asked whether I could be more active in maintaining the project if I made it a paid plugin.

Some open-source plugins have found it possible to have a paid model where they charge for the built version of the plugin and support, while having the source code be freely accessible.

For me however, money is not my main motivation to maintain the project. If I had limitless bandwidth and motivation, I would be happy to maintain this into far future. However, with so many commitments and waning interest in Minecraft as a whole, it really hurts to continue working on ProtectionStones, having to do extensive testing on each release knowing that there is still the potential of pushing breaking changes, while not getting much in return.

## What about passing the project on to someone else?

If there was someone I could entrust it to, I would be really happy to do so!

However, there have barely been any [contributions](https://github.com/espidev/ProtectionStones/pulls) to ProtectionStones since its inception. It appears that most developers are employed by servers and are more keen on forking the project for their own uses than contributing upstream.

Perhaps you are interested in taking over the project!

There are some requirements that I would have to do a full transfer of ownership for security purposes. With the amount of servers that use the plugin, I do not want to jeopardize their trust by giving it to a bad actor, who may release malware (see the [xz incident](https://securelist.com/xz-backdoor-story-part-2-social-engineering/112476/)). I would only give it to someone once they have contributed for a long period of time, and I have trust in their intentions and ability to maintain the plugin. Even then, I would still review public releases to ensure safety.

# What now?

Not much is really going to change, I will try to keep the plugin in a working state for as long as I can. However, I am soon graduating university, and with a full-time job and other interests, I probably will not be active at all. I am not sure if I will keep doing Discord support either consistently, however I have seen that some community members have been helping others, which I appreciate!

# To you, the community!

If you are interested in contributing to ProtectionStones, I would highly encourage it! I can still dedicate time to reviewing pull requests to the repository. If not, I also still encourage developers to fork the project and expanding the codebase in a way they see fit. I hope that it serves as a good base to any that have grand ideas for improving the plugin for their own uses, or others!

Thank you everyone for having supported ProtectionStones these past years, I mean it!
