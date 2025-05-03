---
title: "A Plasma Sprint in Graz"
date: 2025-05-05T11:26:29+01:00
draft: true
tags: [kde]
---

I attended the Plasma sprint this year in Graz, Austria!

It has been a couple of years since I have last met KDE contributors in-person (Akademy 2022), so I really looked forward to finally being able to meet again. This sprint was the first time I met [Bhushan](https://bshah.io), who is a long time contributor to Plasma Mobile, and was the one that initially guided me through contributions! He recently got funding to overhaul and improve the power management stack we have in Plasma Mobile, which you can read about [here](TODO).

## Day 0

The day before, I had just finished my final university exams and moved back home! I then set out to downtown Toronto's Billy Bishop Airport, where I took a small propellor plane to Montreal, where I transferred onto a flight to Vienna.

pictures

After arriving in Vienna, I took the OBB RailJet to Graz arriving in the afternoon.

pictures

At night, I went to a rooftop bar and met a couple of contributors, while gazing over a beautiful view of the city.

- Arrived in Vienna
- Beautiful roof top dinner

## Day 1

The university was closed on this day for Easter, and so the sprint organizers were able to get us access to a hackerspace! 

I reviewed and tested some merge requests ([Micah](https://invent.kde.org/micahstanley) has been on fire lately!), and then started to do some preliminary digging into some issues with Bhushan and Luis.

- Met Bhushan and Luis
- Hacker space
- Triaging lockscreen issues

- Koko: the image viewer is broken for GNOME snapshot generated images
- https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/710 (lockscreen keypad sync)


## Day 2

- Triaging lockscreen issues
- Rebased on got https://invent.kde.org/plasma/kwin/-/merge_requests/6752 merged (lockscreen test)
- https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/713 (load speed test)
- https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/714 (plasma-nm singleton)
- Luis and Bhushan worked on https://invent.kde.org/plasma-mobile/plasma-dialer/-/merge_requests/182
- Rebased https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/270 (lockscreen overlay), discussed that this is the approach we are going to use

## Day 3

- quicksettings triaging
- https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/715 (port caffeine away from DataSource)
- Worked with Bhushan on https://invent.kde.org/teams/plasma-mobile/issues/-/issues/5#note_1203670 (long press power button)
- feedbackd (https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/719)
- Triaging issue tracker
- Worked with Marco on kirigami fixes: https://invent.kde.org/frameworks/kirigami/-/merge_requests/1785, https://invent.kde.org/frameworks/kirigami/-/merge_requests/1784

## Day 4

- feedbackd (https://invent.kde.org/jbbgameich/ktactilefeedback/-/merge_requests/2)
- Revert kwin rules (https://invent.kde.org/plasma/plasma-mobile/-/merge_requests/722), oops sorry
- Notch discussion
- envmanager

## Day 5

- powerdevil kcm: https://invent.kde.org/plasma/powerdevil/-/merge_requests/543
- Finally got logout overlay working (https://invent.kde.org/plasma/plasma-workspace/-/merge_requests/2406)
- Finally got lockscreen overlay working, some discussion and debugging with zamundaaa

Continued
