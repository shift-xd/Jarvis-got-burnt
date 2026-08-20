<div align="center">

<img width="438" height="84" alt="Jarvis - Burnt To A Crisp" src="https://github.com/user-attachments/assets/edf40e43-ad4c-4a50-baea-3ba00d062797" />

# **JARVIS — BURNT TO A CRISP 🔥**

</div>

Made a lil OLED desk robot thingy from scratch :yay

It sits on my desk, shows the time, does animations, runs timers, has alarms/reminders and has a whole web interface hosted by the ESP32 because apparently just making a clock wasn't enough lmao

<div align="center">

<img width="640" height="360" alt="Jarvis demo" src="https://github.com/user-attachments/assets/698d28e2-11c0-439c-8b25-8087b33e968d" />

</div>

## Table of Contents

- [Softwares](#softwares)
- [How To Make](#how-to-make)
- [The Workings Aka Software](#the-workings-aka-software)
- [Firmware](#firmware)
- [PCB && Schematics](#pcb--schematics)
- [Casing](#casing)
- [Features](#features)
- [Macondo](#macondo)

---

## Softwares

<div align="center">

<a href="https://www.arduino.cc/" target="_blank"><img src="https://img.shields.io/badge/Arduino-00878F?style=for-the-badge&logo=arduino&logoColor=white" alt="Arduino"></a>
<a href="https://www.freecad.org/" target="_blank"><img src="https://img.shields.io/badge/FreeCAD-333333?style=for-the-badge&logo=freecad&logoColor=white" alt="FreeCAD"></a>
<a href="https://solvespace.com/" target="_blank"><img src="https://img.shields.io/badge/SolveSpace-333333?style=for-the-badge" alt="SolveSpace"></a>
<a href="https://easyeda.com/" target="_blank"><img src="https://img.shields.io/badge/EasyEDA-1769FF?style=for-the-badge&logo=easyeda&logoColor=white" alt="EasyEDA"></a>

</div>

## How To Make

This is a PCB + firmware + casing project so basically you get all the fun parts :skull:

Get the PCB made, get the components, solder everything together, flash the firmware and shove it into the casing hehe boi.

The whole point was to make the final thing from scratch instead of buying some random smart display.

The general build is:

```text
ESP32 + OLED
     ↓
Solder / test
     ↓
Custom PCB
     ↓
Flash firmware
     ↓
Make the casing
     ↓
Put everything together
     ↓
Njoyy :yay
```

---

## The Workings Aka Software

The ESP32 is basically doing everything here.

It handles the OLED, Wi-Fi, time, timers, animations and the web interface.

```text
                    ESP32
                      │
        ┌─────────────┼─────────────┐
        │             │             │
        ▼             ▼             ▼
      NTP           Timers       Web Server
        │             │             │
        └─────────────┼─────────────┘
                      ▼
                 OLED Display
                      │
                      ▼
                  Animations
```

### The clock thingy

There isn't an RTC module in this :D

The ESP32 gets the time from **NTP over Wi-Fi** and then keeps track of the time itself afterwards.

So basically:

```text
Wi-Fi → NTP → Get time → ESP32 keeps counting → OLED
```

Which saved me from having to add another chip just to tell the time lol.

### The web thingy

The ESP32 also hosts the web interface itself.

The device can create a Wi-Fi network called:

```text
THA PRODUCTIVE TIMER
```

You connect your phone/laptop to it and use the web interface to interact with the timers and stuff.

No separate app. No cloud server. Just the lil ESP32 doing its thing.

---

## Firmware

The firmware is where most of the actual JARVIS brain lives.

It handles:

- OLED rendering
- Animations
- Clock
- NTP synchronisation
- Timers
- Alarms / reminders
- Wi-Fi
- Web server
- Timer controls

The idea was to keep the device mostly self contained so once it's powered up it can just sit there and do its thing.

> Firmware files / source are kept in the repo alongside the hardware files.

---

## Features

### 🕐 Clock

A tiny always-visible clock because opening my phone every time I wanted to know the time felt stupid.

Uses NTP instead of an RTC.

### 🎞️ Animations

The OLED has little animations and visual states so it doesn't just sit there showing boring text 24/7.

### ⏱️ Productivity timers

Some of the timers I made specifically for things I actually do:

- Productivity
- Macondo
- IELTS
- Cat cuddle time 🐈
- Alarms / reminders

Yes, the cat timer is real.

### 🌐 Web interface

A browser based interface served directly from the ESP32.

This lets the device be controlled without making a whole separate phone app.

---

## PCB && Schematics

The electronics are on a custom PCB because obviously I wasn't going to leave the final version as a pile of jumper wires :skull:

### PCB

<div align="center">

<!-- Put the final PCB screenshot here -->

</div>

### Schematic

<div align="center">

<!-- Put the final schematic screenshot here -->

</div>

The PCB basically brings the ESP32 + OLED + the rest of the electronics together into one nice lil board.

---

## Casing

The casing was one of the parts that took wayyy more time than expected.

I used **SolveSpace** and **FreeCAD** while designing it and had to iterate on the dimensions around the actual PCB.

There were some failed prints, spacing problems and other silly things along the way :skull:

The workflow was basically:

```text
Design
  ↓
Print
  ↓
Doesn't fit
  ↓
Cry
  ↓
Change dimensions
  ↓
Print again
  ↓
IT FITS :yay
```

The final enclosure was made to hold the electronics properly instead of just being a random box around it.

---

## Build

This project went through a bunch of little stages before becoming the final thing.

### 1. OLED + ESP32

First I got the basic display working.

### 2. Firmware

Then I started making the clock, animations and other software stuff.

### 3. Timers

Added the productivity / Macondo / IELTS / cat timers and alarms.

### 4. Web app

Made the ESP32 host its own web interface.

### 5. PCB

Designed the actual board for the final build.

### 6. Casing

Designed and printed the enclosure around the electronics.

### 7. Put everything together

And boom. Tiny desk thing.

---

## Hardware

| Thing | What it does |
| --- | --- |
| ESP32 | Brain + Wi-Fi + web server |
| OLED | Shows everything |
| Custom PCB | Holds the electronics |
| 3D printed case | Makes it look like an actual thing |
| USB | Power |

The hardware is intentionally pretty simple. Most of the complicated behaviour is done in software.

---

## Macondo

This project was made as part of **Hack Club Macondo**!

I documented the build process there, including the hardware, software, PCB, enclosure and all the stuff that inevitably went wrong while making it :skull:

**Project:** https://macondo.hackclub.com/projects/9875

---

## Random stuff I learnt making this

- ESP32 firmware
- OLED graphics
- NTP
- Embedded web servers
- Wi-Fi APs
- Timer logic
- PCB design
- Soldering
- FreeCAD
- SolveSpace
- 3D printing
- Designing around actual hardware tolerances
- Debugging hardware + software at the same time

And probably most importantly:

**Don't design the enclosure before you know the exact size of your PCB.**

I learnt that one the hard way :skull:

---

<div align="center">

### Made from scratch by **Jarvis_On_Fire** 🔥

**Njoyy :yay**

</div>
