<img width="438" height="84" alt="Jarvis - Burnt To A Crisp" src="https://github.com/user-attachments/assets/edf40e43-ad4c-4a50-baea-3ba00d062797" />

<img width="640" height="360" alt="Jarvis demo" src="https://github.com/user-attachments/assets/698d28e2-11c0-439c-8b25-8087b33e968d" />

# Jarvis - Burnt To A Crisp 🔥

An **under-monitor OLED desk display / productivity companion** built from scratch around an ESP32.

The idea started pretty simple: I wanted a tiny thing on my desk that could show useful information without needing a phone or a giant screen. Then, obviously, I kept adding stuff until it became this :yay

> **Time + animations + timers + reminders + alarms + a web app + a custom 3D printed enclosure + custom PCB**

This repository is meant to be the all-inclusive documentation for the project — from the original idea and electronics to the firmware, web interface, enclosure and the weird problems I had to solve while building it.

---

## What does it actually do?

Jarvis is basically a small always-visible desk companion.

### ⏰ Time display — without an RTC

One of the fun parts of this project is that there is **no dedicated RTC module**.

Instead, Jarvis connects to Wi-Fi and gets the current time using **NTP (Network Time Protocol)**. Once the time has been obtained, the ESP32 keeps counting the time internally, so the display can continue working even if the Wi-Fi connection disappears afterwards.

That means:

- No RTC module required
- Wi-Fi is used for initial time synchronisation
- The ESP32 keeps the clock running internally afterwards
- The OLED can keep showing time even after the network drops

---

## ✨ Features

### 🕐 Clock

- Digital time display
- NTP-based time synchronisation
- No RTC module
- Continues keeping time after Wi-Fi disconnects

### 🎞️ Animations

The OLED isn't just a boring status screen.

There are multiple small animations designed to make the device feel alive and fun to look at. The display can switch between different visual states and animations instead of permanently showing the same screen.

### ⏱️ Productivity timers

Jarvis has timers for different things I actually wanted to keep track of:

- General productivity / work sessions
- Macondo hours
- Cat cuddle hours 🐈
- IELTS task timing
- Custom alarms / reminders

So instead of opening another app just to start a timer, the thing sitting on my desk can do it.

### 🌐 Web app

This is probably the biggest part of the software side.

Jarvis has its own small web interface hosted directly by the ESP32.

The setup works roughly like this:

```text
             Home Wi-Fi
                  │
                  ▼
             ┌─────────┐
             │  ESP32  │
             └────┬────┘
                  │
                  │ hosts web interface
                  ▼
        ┌────────────────────┐
        │  THA PRODUCTIVE    │
        │       TIMER        │
        │    Web Interface   │
        └────────────────────┘
                  │
                  ▼
          Controls / Timers
                  │
                  ▼
               OLED
```

The ESP32 can create an access point named:

**`THA PRODUCTIVE TIMER`**

You connect to it and the web interface can be used to control the device.

The goal was to make the device usable without needing a dedicated phone application.

---

# 🧠 The Workings — Aka Software

The software is basically split into a few jobs that all run together on the ESP32.

```text
                     ┌───────────────────┐
                     │       ESP32       │
                     │                   │
                     │  Main Application │
                     └─────────┬─────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
   ┌─────────────┐      ┌─────────────┐      ┌─────────────┐
   │ Time / NTP  │      │   Timers    │      │  Web Server │
   └──────┬──────┘      └──────┬──────┘      └──────┬──────┘
          │                    │                    │
          └────────────────────┼────────────────────┘
                               │
                               ▼
                      ┌────────────────┐
                      │ OLED Renderer  │
                      │ + Animations   │
                      └────────────────┘
```

The ESP32 is responsible for the actual logic instead of relying on another computer to run the application.

The OLED is essentially the output layer, while the web interface gives another way to interact with the same timers and settings.

---

# 🖥️ Display

The project uses a small monochrome OLED as the main display.

The screen is intentionally small because the whole point of Jarvis is to have something that can sit underneath / beside a monitor without becoming another giant screen on the desk.

The display is used for:

- Time
- Timer information
- Reminders
- Status information
- Animations

The animations were also designed around the limitations of a tiny monochrome OLED rather than trying to make it behave like a normal graphical display.

---

# 🔌 Hardware

The main controller is an **ESP32**.

The hardware stack consists of:

| Part | Purpose |
| --- | --- |
| ESP32 | Main microcontroller, Wi-Fi and web server |
| OLED display | Main visual interface |
| Custom PCB | Holds the electronics together cleanly |
| 3D printed enclosure | Physical housing for the electronics |
| USB power | Powers the device |

The project deliberately avoids unnecessary hardware where software can do the job. The best example is the clock: instead of adding an RTC, the ESP32 gets time from NTP and keeps counting internally.

---

# 🧩 PCB + Schematics

I also made a PCB and schematic for the project.

The PCB isn't strictly necessary for making a working version — you could build the electronics on a perfboard / protoboard — but I wanted to make an actual PCB for the final build.

The design process was:

```text
Idea
  ↓
Choose components
  ↓
Make schematic
  ↓
Design PCB
  ↓
Solder electronics
  ↓
Test firmware
  ↓
Design enclosure around the electronics
```

The PCB was mainly about making the final build cleaner and more compact.

---

# 🧱 The enclosure

The enclosure went through multiple iterations because apparently making a box around electronics is not as easy as making a box 💀

I originally made a simple rectangular enclosure, but that was pretty boring, so I moved on to a more custom design.

I started with **SolveSpace** and later moved to **FreeCAD** for the more complicated enclosure work.

The final workflow involved:

- Designing the enclosure around the actual electronics
- Adding internal walls so the ESP32 couldn't just wander around inside the case lol
- Separating the lid from the main body
- Designing a sliding / removable mechanism
- Tweaking clearances
- Slicing the model
- 3D printing it
- Fixing print failures
- Re-printing the final version

There were also a couple of failed prints caused by things like power loss, incorrect filament settings and spacing issues.

Eventually the final casing worked and the electronics were moved into it.

---

# 🛠️ Build process

This project was built in stages rather than magically appearing as the final thing.

### 1. Electronics

I first selected the ESP32 and OLED and made the basic connections.

### 2. Schematic

Then I made the schematic and figured out how everything should connect.

### 3. PCB

After that I designed a custom PCB for the final build.

### 4. First firmware

The first firmware was intentionally basic. It was mainly there to prove that the hardware, display and ESP32 were working correctly.

### 5. Animations

Once the basic display worked, I started adding animations and making the device feel less like a development board and more like an actual product.

### 6. Timers + reminders

Then came the useful stuff: timers, alarms, Macondo tracking, cat cuddle tracking and IELTS timing.

### 7. Web interface

After that I built the ESP32-hosted web app so the device could be controlled without needing another dedicated application.

### 8. Enclosure

Finally, I designed and printed the enclosure around the electronics.

---

# 📡 How the networking works

The networking side has two important jobs:

### Time synchronisation

```text
ESP32
  │
  ├── Connect to Wi-Fi
  │
  ├── Contact NTP server
  │
  ├── Get current time
  │
  └── Keep counting internally
```

### Device control

```text
Phone / Laptop
       │
       │ Wi-Fi
       ▼
     ESP32
       │
       ├── Web server
       ├── Timer logic
       └── OLED output
```

The idea is that the ESP32 itself is the computer running the whole thing.

---

# 🌐 Web App

The web app is served by the ESP32 itself instead of requiring a cloud backend.

This keeps the project relatively self-contained:

- No external server required for the controls
- No separate mobile application
- No cloud dashboard
- The ESP32 handles the web interface
- The same device also handles the display and timers

The access point is named:

```text
THA PRODUCTIVE TIMER
```

From there the user can open the hosted interface and interact with the device.

---

# ⏲️ Timer system

The timer system is one of the main reasons I built Jarvis in the first place.

Instead of having a generic countdown timer, I wanted timers that actually represented things I was doing.

Current uses include:

| Timer | Why it exists |
| --- | --- |
| Macondo | Track project/build time |
| Cat cuddle | Track cuddle time because why not 🐈 |
| IELTS | Keep track of IELTS-related tasks |
| Productivity | General focused work |
| Alarms | Reminders for things I need to do |

This also makes the device useful as a little productivity companion rather than just an under-monitor clock.

---

# 📦 Repository structure

The project is intended to keep the different parts of the build together:

```text
Jarvis-got-burnt/
│
├── README.md              ← You are here
│
├── Firmware/              ← ESP32 firmware
├── WebApp/                ← ESP32-hosted web interface
├── PCB/                   ← PCB + schematic files
├── Enclosure/             ← 3D model / CAD files
├── Production/            ← Manufacturing / exported files
└── Assets/                ← Photos, videos and documentation
```

> The repository started as a very small documentation repo, so some of these sections represent the logical parts of the project rather than files that were present from the very first commit.

---

# 🧪 Things I had to figure out

This project was also basically a giant excuse to learn a bunch of stuff.

Some of the bigger learning points were:

- ESP32 firmware development
- OLED graphics
- Animation on a small monochrome display
- NTP time synchronisation
- Maintaining time without an RTC
- Embedded web servers
- Wi-Fi access points
- Web interfaces for microcontrollers
- Timer / alarm logic
- PCB design
- Schematic design
- Soldering
- FreeCAD
- 3D modelling for real hardware
- 3D printer settings
- Designing around real-world tolerances
- Debugging hardware + software together

The enclosure especially took a few attempts before I got something that actually worked.

---

# 🚧 Limitations / things to know

This isn't a commercial product and there are definitely compromises.

- The clock is not backed by a dedicated RTC.
- Accurate initial time requires network time synchronisation.
- The OLED is intentionally small.
- The device is designed around an ESP32 and its available resources.
- The enclosure went through multiple iterations and is very much a custom DIY design.
- A PCB is used for the final build, but a perfboard / protoboard build would also be possible.

Basically: it is a tiny homemade desk device, not a production smart display :lol

---

# 📸 Build / project media

### Final project

<img width="438" height="84" alt="Jarvis project" src="https://github.com/user-attachments/assets/edf40e43-ad4c-4a50-baea-3ba00d062797" />

### Demo

<img width="640" height="360" alt="Jarvis demo" src="https://github.com/user-attachments/assets/698d28e2-11c0-439c-8b25-8087b33e968d" />

More build photos, enclosure iterations and videos are documented in the Hack Club Macondo project journal.

**Macondo project:** https://macondo.hackclub.com/projects/9875

---

# 📖 Build journal

The actual build journal is probably the best way to see how this project evolved because the final version hides how many things went wrong before it worked.

I documented:

- The initial schematic
- PCB design
- Soldering and wiring
- Early firmware
- Enclosure experiments
- Failed 3D prints
- FreeCAD learning
- The final enclosure
- Firmware features
- Web app development
- Time synchronisation
- Timers and alarms

👉 **Full Macondo journal:** https://macondo.hackclub.com/projects/9875

---

# 🙌 Why I made this

I wanted a tiny thing for my desk that was actually useful.

Something that could sit there quietly, show the time, run a timer, remind me about something, show a stupid little animation every now and then and be controllable from a browser.

Instead of buying one, I decided to build it from scratch.

And then, as usual, the project got way bigger than the original idea :yay

---

# 🔥 Project status

**Status:** Built / working

**Platform:** ESP32

**Display:** Small monochrome OLED

**Connectivity:** Wi-Fi

**Interface:** OLED + ESP32-hosted web app

**Timekeeping:** NTP + internal timekeeping

**Enclosure:** Custom 3D printed

**PCB:** Custom designed

**Project level:** Hardware — Level 2 on Macondo

---

## Credits / links

- **Hack Club Macondo project:** https://macondo.hackclub.com/projects/9875
- **Repository:** https://github.com/shift-xd/Jarvis-got-burnt

Made from scratch by **_Jarvis_On_Fire_**.

:yay
