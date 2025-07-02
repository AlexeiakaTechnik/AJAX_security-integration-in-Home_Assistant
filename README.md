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
Inspired by amazing ideas of [Carlos Cordero](https://carloscordero.com/), [Youtube](https://www.youtube.com/@unlocoysutecnologia), [Github](https://github.com/ccorderor)




















# 🛡️ AJAX Security System Integration in Home Assistant  
_Full DIY Control via Smart Relays, Alarmo, and UI Cards_

---

## 📚 Table of Contents

1. [🔍 Introduction](#-introduction)  
2. [🏗️ AJAX System Overview & Integration Limitations](#-ajax-system-overview--integration-limitations)  
3. [🎮 Controlling AJAX with a SpaceControl Remote + Wi-Fi Relay](#-controlling-ajax-with-a-spacecontrol-remote--wifi-relay)  
4. [🏠 Integrating with Home Assistant & Alarmo](#-integrating-with-home-assistant--alarmo)  
5. [🖼️ Creating UI Cards & Status Sync](#-creating-ui-cards--status-sync)  
6. [🎥 Live Demo – AJAX + Home Assistant in Action](#-live-demo--ajax--home-assistant-in-action)  
7. [🧠 Conclusions & Thoughts](#-conclusions--thoughts)  
8. [🪪 License](#-license)  
9. [👨‍💻 Author and Inspiration](#-author-and-inspiration)  
10. [🔗 Related Projects & Resources](#-related-projects--resources)

---

## 🔍 Introduction


**First I would like to mention and give my gratitude to an author, IoT & Smart Home professional, who gave us the idea for such a project — [Carlos Cordero](https://carloscordero.com/) — [YouTube blogger](https://www.youtube.com/watch?v=ot_ndKUsFXk), CTO of [BKOOL](https://www.linkedin.com/company/bkool/), and tech wizard from Spain.**
**His GitHub: [Link](https://github.com/ccorderor)**

The AJAX system is built as a certified Grade 2 alarm platform, designed to be tamper-resistant and installation-friendly for security professionals. While it’s not an open DIY platform, AJAX does support integration with:

- Monitoring centers via SIA/Contact ID  
- Hardware relays and smart switches via their own devices (e.g., WallSwitch, Relay)  
- Third-party systems through official integration tools like AJAX Translator and selected partner APIs

However, for platforms like Home Assistant, AJAX still has no native integration — and likely won’t — probably because such public APIs/token-based security is a big no-no for certain security certifications.  
That’s where this guide steps in — showing how to safely bridge the gap with a hardware relay and smart logic.

This guide shows how to bridge AJAX with Home Assistant, combining the power and reliability of a certified security system with the intelligence and flexibility of smart home automation. You’ll learn how to:

- Control AJAX modes (arm, disarm, night, panic) via a simple smart relay hack  
- Build a synced Home Assistant dashboard with real alarm zones, triggers, and alerts  
- **(Bonus)** Use AJAX sensor states (motion, panic, etc.) in automations — covered in a companion article

> 📌 This is a real deployment. Every screenshot, wiring diagram, and automation here was used in an actual smart home with AJAX as the primary security system.


---

## 🧰 Requirements


- ✅ Home Assistant running (local or remote)  
- ✅ Configured AJAX security system with active SpaceControl fob and configured SIA output to a monitoring station (your HA server in this instance)  
- ✅ Basic soldering, wiring, and understanding of relays  
- ✅ 4-channel Smart Relay with dry-contact capability (Wi-Fi, Zigbee — any brand supported in HA)


---

## 🏗️ AJAX System Overview & Integration Limitations

The AJAX system is built as a closed, certified Grade 2 security solution. It’s designed to be tamper-proof, interference-resistant, and independent from DIY environments — for good reason: it’s used in banks, businesses, and homes that need real protection.

Here’s what you need to understand before integrating:

### 🚫 Why AJAX Doesn’t “Just Integrate”

- **No API** — There’s no REST, MQTT, or WebSocket interface to connect to  
- **Certified Devices Only** — The system only communicates with registered AJAX devices (you can’t spoof or simulate one)  
- **Cloud Locked** — The official app and ecosystem are locked out of any cloud-based integrations and only provide remote control, one-way (monitoring-only) integration with security providers (via SIA), and other misc. tools for internal AJAX purposes  
- **Tamper-Safe by Design** — Even opening a sensor triggers tamper alarms — not to mention messing with signals, circuits, data chains, etc.

### 🧩 But There Are Workarounds

In particular, AJAX can talk to monitoring centers via the SIA protocol (for alarms and events — so that armed security can respond to incidents).

We use this to our advantage:

- In **this repo**, we simulate button presses using a relay directly wired to the AJAX SpaceControl key fob. This lets Home Assistant arm/disarm AJAX — safely and reliably.  
- In the **other article**, I explain how we listen to SIA events from the AJAX Hub to react to real events and “hijack” them for Home Assistant automations.  
  👉 Be sure to [check it out](https://github.com/AlexeiakaTechnik/Use-Ajax-Security-alarm-sensors-as-a-Automation-Triggers-in-Home-Assistant)!


---

## 🎮 Controlling AJAX with a SpaceControl Remote + Wi-Fi Relay

This chapter explains how to *simulate button presses on the AJAX SpaceControl key fob* using a generic Wi-Fi (or Zigbee) relay with dry-contact output. This method allows Home Assistant to control AJAX modes like:

- Arm (Full Security)
- Disarm
- Night Mode
- Panic Alarm

### 🧱 Hardware Setup Overview

- You will need a **4-channel smart relay** with dry contacts.
- Each relay output is wired to one of the four buttons on the AJAX SpaceControl key fob.
- When triggered via Home Assistant, the relay will short the contact, simulating a button press.

📸 *[Insert wiring diagram and photos]*  
📦 *[Link to recommended relay models and enclosure]*

---

## 🏠 Integrating with Home Assistant & Alarmo

In this section, we configure **Home Assistant** to:

- Trigger relays for each AJAX mode
- Use **Alarmo** to create virtual alarm zones
- Sync the state of Alarmo with AJAX to reflect true armed/disarmed status
- Send notifications and alerts

### 🔧 Alarmo Configuration

- Set up custom alarm modes (Arm, Disarm, Night, Panic)
- Use input_booleans or binary_sensors to mirror AJAX state
- Automate relay activation based on Alarmo state changes

📄 Example YAMLs for automation, triggers, and states will be included here.

---

## 🖼️ Creating UI Cards & Status Sync

You’ll learn how to create an intuitive control interface using:

- `custom:mushroom-alarm-control-panel-card`
- Visual state mirroring (based on Alarmo + AJAX feedback)
- Bonus: integrating **AJAX IR MotionProtect** devices as presence indicators

### 🔁 Sync Tips

- Use very short SIA event-to-state automations (covered in the [SIA integration repo](https://github.com/AlexeiakaTechnik/Use-Ajax-Security-alarm-sensors-as-a-Automation-Triggers-in-Home-Assistant))
- Show real-time triggers from **AJAX** and third-party sensors
- Optional: overlay **Dahua SmartMotion** and door/window sensors into the same dashboard  
  📎 [See my Dahua integration article](https://github.com/AlexeiakaTechnik/Integrating-Dahua-Cameras-CCTV-System-in-Home-Assistant)

---

## 🎥 Live Demo – AJAX + Home Assistant in Action

🎬 This section will include:

- A **side-by-side video** showing:
  - Home Assistant dashboard on a tablet
  - The AJAX mobile app
  - Physical AJAX key fob being activated by the relay
- Visual demonstration of arming/disarming in real-time
- Audio alerts or TTS notifications (if configured)

---

## 🧠 Conclusions & Thoughts

This integration brings AJAX — a closed, commercial security system — into the world of flexible, reactive automation without voiding its integrity.

### ✅ Pros

- Doesn’t interfere with AJAX’s core functionality or warranty
- Reliable physical signal = very low chance of failure
- Leverages Alarmo to build powerful custom logic

### ⚠️ Limitations

- No native state feedback without SIA setup  
- Physical wiring is required (minor soldering needed)  
- Limited to key fob functionality (no deep device-level control)

Still — for most smart home users — this method unlocks **90% of the value** you want from alarm control.

---

## 🪪 License

This project is licensed under the [MIT License](LICENSE).

---

## 👨‍💻 Author and Inspiration

Created by [Alexei](https://github.com/AlexeiakaTechnik) — field integrator, systems tinkerer, and smart home architect in the making.  
Inspired by the amazing work of [Carlos Cordero](https://carloscordero.com/) → [YouTube](https://www.youtube.com/@unlocoysutecnologia) · [GitHub](https://github.com/ccorderor)

---

## 🔗 Related Projects & Resources

- ✅ [Use AJAX Alarm Sensors as Automation Triggers](https://github.com/AlexeiakaTechnik/Use-Ajax-Security-alarm-sensors-as-a-Automation-Triggers-in-Home-Assistant)  
- ✅ [Integrating Dahua Cameras with Home Assistant](https://github.com/AlexeiakaTechnik/Integrating-Dahua-Cameras-CCTV-System-in-Home-Assistant)  
- ✅ [ESP32 DIY CO₂ & Presence Sensors for HA](https://github.com/AlexeiakaTechnik/Engineering-DIY-ESP32-Sensors-and-Cameras-for-Home-Assistant)  
- 📄 [AJAX SIA Event Code Reference (Google Sheet)](https://docs.google.com/spreadsheets/d/1-N-RZVS8IiwM5zuw2u4gt8Bx_5xo_JOwuagHJgSJxUw/edit#gid=920971512)
