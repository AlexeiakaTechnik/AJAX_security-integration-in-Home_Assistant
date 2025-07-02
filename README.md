# AJAX Security System integration in Home Assistant.
_Full DIY Control via Smart Relays, Alarmo, and UI Cards_

---

## 📚 Table of Contents

1. 🔍 Introduction
2. 🏗️ AJAX System Overview & Integration Limitations
3. 🎮 Controlling AJAX with a SpaceControl Remote + Wi-Fi Relay
4. 🏠 Integrating with Home Assistant & Alarmo
5. 🖼️ Creating UI Cards & Status Sync
6. 🎥 Live Demo – AJAX + Home Assistant in Action
7. 🧠 Conclusions & Thoughts
8. 🪪 License
9. 👨‍💻 Author and Insipiration
10. 🔗 Related Projects & Resources


##🔍 Introduction

First I would like to mention and give my gratitude to an author, IoT & Smart Home professional, who gave us an idea for such a project! [Carlos Cordero](https://carloscordero.com/) - [youtube blogger](https://www.youtube.com/watch?v=ot_ndKUsFXk), CTO of [BKOOL](https://www.linkedin.com/company/bkool/), and tech wiard from Spain. His Github - [Link](https://github.com/ccorderor)

The AJAX system is built as a certified Grade 2 alarm platform, designed to be tamper-resistant and installation-friendly for security professionals. While it’s not an open DIY platform, AJAX does support integration with:

Monitoring centers via SIA/Contact ID

Hardware relays and smart switches via their own devices (e.g., WallSwitch, Relay)

Third-party systems through official integration tools like AJAX Translator and selected partner APIs

However, for platforms like Home Assistant, AJAX still has no native integration, and likely will not have probably because such public API's/token-based security is a big No-No for certain security certifications. That’s where this guide steps in — showing how to safely bridge the gap with a hardware relay and smart logic.

This guide shows how to bridge AJAX with Home Assistant, combining the power and reliability of a certified security system with the intelligence and flexibility of smart home automation. You’ll learn how to:

Control AJAX modes (arm, disarm, night, panic) via a simple smart relay hack

Build a synced Home Assistant dashboard with real alarm zones, triggers, and alerts

(Bonus) Use AJAX sensor states (motion, panic, etc.) in automations — covered in a companion article

No reverse engineering. No unreliable cloud hacks. Just clever wiring, good automation logic, and a field-tested setup that works.

📌 This is a real deployment. Every screenshot, wiring diagram, and automation here was used in an actual smart home with AJAX as the primary security system.

---

## 🧰 Requirements

- ✅ Home Assistant running (local or remote)
- ✅ Configured AJAX security system with active SpaceControl fob and configured SIA output to Monitoring Station(Your HA Server in this instance)
- ✅ Basic soldering, wiring, and understanding of relays
- ✅ 4-channell Smart Relay with Dry-contact capability(Wifi, Zigbee, whatever brand you prefer as long as it's supported in HA)

---

##🏗️ AJAX System Overview & Integration Limitations
The AJAX system is built as a closed, certified Grade 2 security solution. It’s designed to be tamper-proof, interference-resistant, and independent from DIY environments — for good reason: it’s used in banks, businesses, and homes that need real protection.

Here’s what you need to understand before integrating:

🚫 Why AJAX Doesn’t “Just Integrate”
No API — There’s no REST, MQTT, or WebSocket interface to connect to

Certified Devices Only — The system only communicates with registered AJAX devices (you can’t spoof or simulate one)

Cloud Locked — The official app and ecosystem are locked out of any cloud based integrations and only provide remote control, one-way(monitoring only) integration with Security providers(SIA), and other misc. things for internal AJAX's purposes.

Tamper-Safe by Design — Even opening a sensor triggers tamper alarms, not to mention messing with signals, circuits, chains of data, etc.

🧩 But There Are Workarounds

Particulary, AJAX can talk to Monitoring centers via the SIA protocol (for alarms and events - so that armed security could arrive to your site and provide.. well, security)

We use this to our advantage:

In this repo, we simulate button presses using a relay directly wired to AJAX SpaceControl key fob! This lets Home Assistant arm/disarm AJAX — safely and reliably.

In the other article, I explain how we listen to SIA events from the AJAX Hub to react to real events and "hijack" them for Home Assistant Automations. Be sure to [check it out](https://github.com/AlexeiakaTechnik/Use-Ajax-Security-alarm-sensors-as-a-Automation-Triggers-in-Home-Assistant)!

🗺️ How It All Connects
Here’s the architecture we’re building:

```text
[Home Assistant]  <--->  [4ch Smart Relay]
     ^                         | - direct wiring
     |                         └──> [AJAX SpaceControl Key Fob]
     |                                    ↓ - ajax radio interface
     |                           [AJAX Hub ←→ AJAX App]
     |                                    ↓ -SIA
     --------------------    [Monitoring Center (HA )]
```

This preserves AJAX’s internal integrity — no warranty voiding, no breaking its security guarantees — while giving you just enough control to smarten it up and control from single(HA) interface.



---

## 🪪 License

This project is licensed under the [MIT License](LICENSE).

---

## 🧠 Author and Insipiration

Created by [Alexei](https://github.com/AlexeiakaTechnik) — field integrator, systems tinkerer, and smart home architect in the making.
Inspired by amazing ideas of [Carlos Cordero](https://carloscordero.com/)

