---
layout: post
title: Autopsy Of A Car Accident With Comma.ai Part 1
---

### Background

#### The Car

I have been driving a Subaru XV 2013 model for about 4 years. After I get to know the comma.ai team and what they are doing, I purpased one Panda and of cource install the chffr app on my Pixel phone. It is really interesting to see on cabana.comma.ai of my uploaded driving data

#### The Accident

On one day Nov 2017 I was unfortunately caught in a car accident while driving. The car in front me hit the car before it because of its sudden stop, and with just a blink of lack of consentration I hit the car in front me as well. Everything happened too quickly and although I stomp the break pedal so hard, the crash could not be avoided. Luckily I was not harmed despite the airbags all flying out and smashing some of the gidgets that decorate the car.

### Autopsy Attempt, Round One

#### The DBC

Although it is pretty dramtic for me to review the incident footage chffr captured even now, curiosity preempts at the end. I want to know when an accident like this happens, what could chffr catch via Panda.

The first thing I need to figure out is the DBC file for Subaru. With a great open source community, comma.ai provides a [opendbc](https://github.com/commaai/opendbc) collaboration. Although we have one Subaru Outback [DBC](https://github.com/commaai/opendbc/blob/master/subaru_outback_2016_eyesight.dbc) available, it is designed to be consumed by the NEO or openpilot. What I need at the moment is to first figure out what do those CAN ID represents

#### CAN ID

Again with the support of an open source community, I find something would be helpful for me. Given that it is impossible for me to test and figure out the CAN ID meanings while driving, I go to the subaru channel on comma.ai slack and find out there is a [collaborative document](https://docs.google.com/spreadsheets/d/1w8ywaBBtRebH6e4ohcqpDv52gl3RadxKqjbVYO5kdfc/edit?usp=sharing) which Subaru drivers are working on. Exactly what I need to begin with.

##### CAN IDs needs attention

![CAN ID 0:d1](https://raw.githubusercontent.com/hannibalhuang/hannibalhuang.github.io/master/image/CAN%20PID%201.png)

![CAN ID 0:d3](https://raw.githubusercontent.com/hannibalhuang/hannibalhuang.github.io/master/image/CAN%20PID%202.png)

From the Subaru CAN ID document, I identify at least initially the above mentioned two things which relates to Break functionality is something I could confirm for XV, which should resemble the Outback design for most of the part.

##### Confirm Wtih Captured Data

![Captured Data Before Hit](https://raw.githubusercontent.com/hannibalhuang/hannibalhuang.github.io/master/image/Cabana%20Crash%200.png)

![Captured Data During Hit](https://raw.githubusercontent.com/hannibalhuang/hannibalhuang.github.io/master/image/Cabana%20Crash%201.png)

![Captured Data After Hit](https://raw.githubusercontent.com/hannibalhuang/hannibalhuang.github.io/master/image/Cabana%20Crash%202.png)

As could be seen above, for 0:d1, before hit there is no signal on the break (byte 3), and when the hit happened byte 3 suddenly goes crazy and after hit all the speed and break bytes turned to zero.

For 0:d3, Byte 5 always got value before and during the hit, but remains 98 after the hit. This I assume is because the Byte 5 is signalling the whether the break pedal is pressed. (I of course kept pressing the break pedal for quite a while after the hit)

### Next

This is a very coarse attempt on a preliminary examination of what happens in the CAN bus when an accident happens. I will explore more CAN ID meanings (there are quite a few undetermind) and see if i could paint a more and more holitic picture.
