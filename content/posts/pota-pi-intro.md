---
title: "POTA-Pi Introduction"
date: 2022-07-02T22:13:21-05:00
draft: false
---

First off, many thanks to Jason (KM4ACK), Julian (OH8STN), Mike (K8MRD), Tracy (VE3TWM), and Chris (K2CJB)
for much of the inspiration and ideas for this project.

When the COVID-19 pandemic hit us in early 2020, I was looking for a way to safely get out of the house
from time to time.  I've been working for home for over 15 years, so getting out of the house for me is
a lot like coming home from a day of work for others!  In mid-2020, I discovered the Parks on the Air
program (aka POTA).

After the ARRL's wildly successful National Parks on the Air celebration in 2016, a number of ham radio
enthusiasts who really enjoyed the event and it's focus on portable operations wanted to continue it in
some manner after the ARRL celebration ended.  It turned out that World Wide Flora and Fauna (WWFF) had
a similar program that was popular in Europe, but had never really taken off here in the US.  So, the
first efforts to continue were under that program.  However, internal disagreements between the world
coordinators and the US-based coordinators led to a split of many operators from the WWFF program, and
the creation of a new program - POTA.

After learning about POTA, I started becoming active in it.  My first activation was a park right in my
hometown - Prairie Spirit Trail State Park (KS), K-2350 - in mid-September of 2020.  Since then, as I
write this, I have done 2 other activations - Cross Timbers State Park (KS), K-2334, as well as a
"two-fer" activation of Woodson State Fishing Lake/Woodson Wildlife Area, K-7411/K-7949.  During these
early operations, I found I needed a more "integrated" portable setup if I was going to do this regularly.
My existing portable setup (Elecraft KX3 and KXPA100) took way too long to hook up and get on the air with.

After watching a number of YouTube videos from active POTA operators, as well as other operators who
primarily operate portable, I decided to try to build a tightly-integrated portable setup based around
the Raspberry Pi single-board computer - hence the name "POTA-Pi".

First released in 2012, the Raspberry Pi is a small, credit card sized computer originally developed to be
an inexpensive (below $40 US) tool to teach computer science in schools and developing countries.  However,
the board went far beyond that original plan, and found large acceptance in the "maker" community as well.
The current (2022) version, the Raspberry Pi model 4B, provides connections via USB, Ethernet, WiFi, Mini-HDMI,
as well as a micro-SD card integrated for storage, and comes in models with 2GB, 4GB, and 8GB of memory.
I have chosen the 8GB model as the basis for this build.  Since MicroSD cards have a tendency to corrupt
themselves during power failures when used with a Pi, I've chosen to use the support built-in to the Pi to
boot from a USB hard drive.  So I have also acquired a Samsung 860 EVO M.2 SSD, and a specially designed case -
the [Argon ONE M.2 case](https://www.argon40.com/products/argon-one-m-2-case-for-raspberry-pi-4), which allows
the M.2 SSD to connect to the Pi via USB.
