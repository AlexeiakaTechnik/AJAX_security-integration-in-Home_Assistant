![image](https://github.com/user-attachments/assets/80a4172b-df12-4e60-a5d7-28ed11b5f64c)



# 🛡️ AJAX Security System Integration in Home Assistant  
_Full DIY Control via Smart Relays, Alarmo, and UI Cards_

---

## 📚 Table of Contents

1. [🔍 Introduction](#-introduction)  
2. [🧰 Requirements](#-requirements)
3. [🏗️ AJAX System Overview & Integration Limitations](https://github.com/AlexeiakaTechnik/AJAX_security-integration-in-Home_Assistant/blob/main/README.md#%EF%B8%8F-ajax-system-overview--integration-limitations)
4. [🎮 Controlling AJAX with a SpaceControl Remote + Wi-Fi Relay](https://github.com/AlexeiakaTechnik/AJAX_security-integration-in-Home_Assistant/blob/main/README.md#-controlling-ajax-with-a-spacecontrol-remote--wi-fi-relay)
5. [🏠 Integrating with Home Assistant & Alarmo](https://github.com/AlexeiakaTechnik/AJAX_security-integration-in-Home_Assistant/blob/main/README.md#-integrating-with-home-assistant--alarmo)
6. [🖼️ Creating UI Cards & Status Sync](https://github.com/AlexeiakaTechnik/AJAX_security-integration-in-Home_Assistant/blob/main/README.md#%EF%B8%8F-creating-ui-cards--status-sync) 
7. [🎥 Live Demo – AJAX + Home Assistant in Action](#-live-demo--ajax--home-assistant-in-action)  
8. [🧠 Conclusions & Thoughts](#-conclusions--thoughts)  
9. [🪪 License](#-license)  
10. [👨‍💻 Author and Inspiration](#-author-and-inspiration)  
11. [🔗 Related Projects & Resources](#-related-projects--resources)

---

## 🔍 Introduction


**First I would like to mention and give my gratitude to an author, IoT & Smart Home professional, who gave us the idea for such a project — [Carlos Cordero](https://carloscordero.com/) — [YouTube blogger](https://www.youtube.com/watch?v=ot_ndKUsFXk), CTO of [BKOOL](https://www.linkedin.com/company/bkool/), and tech wizard from Spain.**
**His GitHub: [Link](https://github.com/ccorderor)**

The AJAX system is built as a certified Grade 2 alarm platform, designed to be tamper-resistant and installation-friendly for security professionals. While it’s not an open DIY platform, AJAX does support integration with:

- Monitoring centers via SIA/Contact ID  
- Hardware relays and smart switches via their own devices (e.g., WallSwitch, Relay)  
- Third-party systems through official integration tools like AJAX Translator and selected partner APIs

However, for platforms like Home Assistant, AJAX still has no native integration — and likely won’t — probably because such public APIs/token-based security is a big no-no for certain security certifications and industry requirements. That’s where this guide steps in — showing how to safely bridge the gap with a hardware relay and AJAX's Alarm logic.

This guide shows how to bridge AJAX with Home Assistant, combining the power and reliability of a certified security system with the intelligence and flexibility of smart home automation. You’ll learn how to:

- Control AJAX modes (arm, disarm, night, panic) via a simple smart relay hack  
- Build a synced Home Assistant dashboard with real alarm zones, triggers, and alerts  
- **(Bonus)** Use AJAX sensor states (motion, panic, etc.) in automations — covered in a companion article

No reverse engineering. No unreliable cloud hacks. Just clever wiring, good automation logic, and a field-tested setup that works.

> 📌 This is a real deployment. Every screenshot, wiring diagram, and automation here was used in an actual smart home with AJAX as the primary security system.


---

## 🧰 Requirements


- ✅ Home Assistant running (local or remote)  
- ✅ Configured AJAX security system with active SpaceControl fob and configured SIA output to a monitoring station (your HA server in this instance)  
- ✅ Basic soldering, wiring, and understanding of relays  
- ✅ 4-channel Smart Relay with dry-contact capability (Wi-Fi, Zigbee — any brand supported in HA)


---

## 🏗️ AJAX System Overview & Integration Limitations

📸 *[Insert picture of AJAX Ecosystem protection]* 

The AJAX system is built as a closed, certified Grade 2 security solution. It’s designed to be tamper-proof, interference-resistant, and independent from DIY environments — for good reason: it’s used in banks, businesses, and homes/locations that need _secured_ protection.

![image](https://github.com/user-attachments/assets/b9580717-a260-4d85-85b2-c5b069a0ae34)


Here’s what you need to understand before integrating:

### 🚫 Why AJAX Doesn’t “Just Integrate”

- **No API** — There’s no REST, MQTT, or WebSocket interface to connect to  
- **Certified Devices Only** — The system only communicates with registered AJAX devices (you can’t spoof or simulate one)  
- **Cloud Locked** — The official app and ecosystem are locked out of any cloud-based integrations and only provide remote control, one-way (monitoring-only) integration with security providers (via SIA), and other misc. tools for internal AJAX purposes  
- **Tamper-Safe by Design** — Even opening a sensor triggers tamper alarms — not to mention messing with signals, circuits, data chains, etc.

### 🧩 But There Are Workarounds

In particular, AJAX can talk to monitoring centers via the SIA protocol (for alarms and events — so that armed security can respond to incidents).

We use this to our advantage:

- In this guide, we simulate button presses using a relay directly wired to the [AJAX SpaceControl key fob](https://ajax.systems/products/ajaxspacecontrol/). This lets Home Assistant arm/disarm AJAX — safely and reliably.  
- In the **other article**, I explain how we listen to SIA events from the AJAX Hub to react to real events and “hijack” them for Home Assistant automations. Be sure to [check it out](https://github.com/AlexeiakaTechnik/Use-Ajax-Security-alarm-sensors-as-a-Automation-Triggers-in-Home-Assistant)!


---

## 🎮 Controlling AJAX with a SpaceControl Remote + Wi-Fi Relay

This chapter explains how to *simulate button presses on the AJAX SpaceControl key fob* using a generic Wi-Fi (or Zigbee) relay with dry-contact output. This method allows Home Assistant to control AJAX modes like:

- Arm (Full Security)
- Disarm
- Night Mode
- Panic Alarm

![image](https://github.com/user-attachments/assets/55497e3d-6a38-4d6a-a206-655ed49c5041)


### 🗺️ How It All Connects

Here’s the architecture we’re building:

![AJAX Schema crop](https://github.com/user-attachments/assets/b6b711b8-adb4-4136-bf49-5b7193217e6c)


This preserves AJAX’s internal integrity — no warranty voiding, no breaking its security guarantees — while giving you just enough control to smarten it up and control from single(HA) interface.

### 🧱 Hardware Setup Overview

Basically:
- You will need a **4-channel smart relay** with dry contacts.
- Each relay output is wired to one of the four buttons on the AJAX SpaceControl key fob.
- When triggered via Home Assistant, the relay will short the contact, simulating a button press.

## **The AJAX Space Control fob** plastic enclosure is easily opened by inserting something like plastic knife/tool in beween front and back.


![image](https://github.com/user-attachments/assets/287b096c-dbb2-4660-bda8-3c1dd8f90af3) ![image](https://github.com/user-attachments/assets/4d77be34-d1fd-41cd-8915-b87e4d94dc9e)


## There are 4 buttons, 4 contact pairs, by shorting button and GRND we press the button. You can wire each button signal and any ground contact direcltly to Smart Relay contacts.


![image](https://github.com/user-attachments/assets/ab0abd59-02a3-49da-aa4b-2c54ba74595a)

## **The Smart Relay with dry contacts** should have contacts designated for "Ithing mode" or "Momentary mode", "Dry Contact mode"


![image](https://github.com/user-attachments/assets/e02cc0bc-340d-4153-a34d-17d6ecdea1a3)


## Here is the clear wiring diagram:


![image](https://github.com/user-attachments/assets/fdd75ece-c6ee-4499-aa08-73fab17b553c)
  



- Regarding the device itself - you can use whatever relay you like, as long as it supports dry contact connection and Home Assistant integration of any kind.

I have used this one from AliExpress:

📦 *[[Link]](https://sl.aliexpress.ru/p?key=QJ9S3Vp)*

- But you can use others. Examples:

  - [SONOFF](https://sl.aliexpress.ru/p?key=Wc9S3gV) - ok brand, good value
  - [eWelink](https://sl.aliexpress.ru/p?key=jb9S3nc) - same as Sonoff, Actually eWelink is official Sonoff APP, don't know why they are presented as separate brands, but its very usual for Chinese market
  - Combine 2 [Aqara](https://www.aqara.com/eu/product/dual-relay-module-t2) relays - if you have their hub, but HA interation may be not as staightforward(may require integration through Apple homekit)
  - Or something like [Shelly](https://sl.aliexpress.ru/p?key=DX9S3Tf) - it's an overkill IMO but if you can spend a lot - options like this are extra reliable professional equipment with great integration capabilities


## 💡 Bonus - ESP32 DIY Solution

It's actually fairly easy to buy very cheap esp32 development board and configure/flash it with [ESPHome](https://esphome.io/index.html) or completely yourself from the ground up. Configure PINs to close the circuit momentarily and use it as your relay substitute with any additional functions you want, executed on the esp32 Chip itself if you want. Plus it's expandable! Be sure to check out my big article - [ESP32 DIY CO₂ Temperature & Humidity Sensors / Cameras for HA](https://github.com/AlexeiakaTechnik/Engineering-DIY-ESP32-Sensors-and-Cameras-for-Home-Assistant).


---

## ⚙️ AJAX System - Devices, Groups and SIA Monitoring Station Setup

Whether you have installed AJAX system yourself or used security company services - there are important moments in configuring a few things in the **AJAX APP** ([Android](https://play.google.com/store/apps/details?id=com.ajaxsystems&pcampaignid=web_share), [Apple](https://apps.apple.com/us/app/ajax-security-system/id1063166247)). Better yet - use AJAX PRO([Android](https://play.google.com/store/apps/details?id=com.ajaxsystems.pro&pcampaignid=web_share), [Apple](https://apps.apple.com/us/app/ajax-pro-tool-for-engineers/id1249276895)) and make yourself a Private Installer in your native User AJAX APP(Hub settings -> Installers/Companies -> Invite -> introduce email of account you have registered within AJAX PRO APP). Be sure to mention it to your installer or whoever provides warranty/support for AJAX system to avoid any issues.

Important things to consider in the AJAX APP are:

- If you want to get readings of motion detection(or door/window triggers) in your configured areas _without_ setting alarm to Armed/Night mode, for example for quick dashboard glance in HA, as I have in my setup, you want to configure all your [MotionProtect](https://ajax.systems/products/motionprotect/) and [DoorProtect](https://ajax.systems/products/doorprotect/) devices to be **Always Active (24hrs)**. Do not worry - this will not drain your batterry considerably faster. I have used this setup for two years now, after several years in default mode and have not noticed any additional battery drain. To do this - go to device list, click on your devices one by one, enter device setting(cog) and set Always active slider to enabled.

- [SIA Integration in Home Assistant](https://www.home-assistant.io/integrations/sia/) will recieve events from AJAX's configured **Groups of devices**, not from _each and every device separately_. Important note here - different generations of AJAX Hubs have different group number limitations(sales booster of course). My Hub - [Hub (2G) Jeweller](https://ajax.systems/products/specs/hub/) - supports up to 9 groups as all Non-Plus models. If you want 25 Groups you have to buy Hub Plus/Hub 2 Plus. To set up Security Groups got to Control tab in APP -> Settings(cog) -> in Space settings select Groups -> + Add Group -> introduce Name and add devices in the Group. As I only have 9 group limit and 12+ devices I had configured one group to include MotionProtect and DoorProtect device from the same room/area.
![image](https://github.com/user-attachments/assets/a4ef4f86-ce33-4ff9-9e1e-41a08409e35b)

- And finally - you should configure your AJAX Hub to send SIA Events to your HA instance. To do this go to Device tab in APP -> your Hub -> Settings(cog) -> Security companies -> CMS CONNECTION Monitoring station: 
  * Protocol - SIA DC-09 (SIA-DCS), 
  * ALARM CHANNEL - Ethernet(unless your server is remote and you have configured network to get data from your Hub's location via Cellular), 
  * PRIMARY IP ADDRESS - *Your HA IP* and *Use some free Port*(you also will use this port in HA SIA Integration when adding entry), 
  * SECONDARY IP ADDRESS - I have used my HA servers Wi-Fi adapters IP for redundancy, 
  * Monitoring station ping interval - 1 min works for me, 
  * Connect on demand - Disabled works for me, 
  * Object number - pick any name, but be sure to write it down, it will be useful while tracing SIA Events in HA Event bus, 
  * Encryption - I did not configure it as my network is secured, 
  * Send coordinates - Disable, 
  * Alarm restoration on ARC - Send restoration code > When disarmed / Send panic (hold-up) device restoration event > Enabled

This should be enough to get AJAX System ready for uasge with HA.


---

## 🏠 Integrating with Home Assistant & Alarmo

In this section, we configure **Home Assistant** to:

- Use **SIA Alarm Systems** integration to add and configure AJAX Zones to Home Assistant
- Use **Alarmo** to create virtual alarm zones, send notifications and alerts
- Sync the state of HA's Alarmo with AJAX and vice versa to reflect true Armed/Disarmed/other statuses

### 🔧 SIA Alarm Systems Configuration

Install SIA Alarm Systems Official Inegration.

[![Open this in Home Assistant](https://my.home-assistant.io/badges/integration.svg)](https://my.home-assistant.io/redirect/config_flow_start/?domain=sia)

![image](https://github.com/user-attachments/assets/5393f27d-6a29-4d84-93d8-42ba2bb70ebc)

When adding config - set **Port** as you have set port in AJAX APP, **Protocol** to TCP(unless you set it otherwise), **Account ID** to **Object number** as set in AJAX APP Monitoring station, **Encryption key** if you have set it uo in AJAX APP, **Ping Interval** - same as in AJAX APP, **Number of zones** is supposed to be equal to **Group number** set up in AJAX APP or Group number + 1(for adding HUB as a zone itself) - try both or add ones if you do not see all zones set up.

These are screenshots from my SIA Device List after I reamed them and added lables, etc.

![image](https://github.com/user-attachments/assets/be81d636-2204-491b-be78-e3d7ecb0c249)

FYI, there will be Smoke / Moisture devices & entities added. This is how integration gets info from AJAX Hub, but they are useless as far as I know.

![image](https://github.com/user-attachments/assets/cd66d368-9632-4534-8e3c-d69244532428)



### 🔧 Alarmo Configuration

First, lets install ALARMO Custom Inegration. For this we will need HACS Community Store. Here is the link in case you don't have it installed:

[![HACS](https://img.shields.io/badge/HACS-Install%20Guide-blue?logo=home-assistant&style=flat-square)](https://hacs.xyz)

And ALARMO Integration:

[![Open this in Home Assistant](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?repository_url=https%3A%2F%2Fgithub.com%2Fnielsfaber%2Falarmo)

ALARMO Integration is unfortunatly only configured via UI, so the process is a bit tedious. Alarmo is intentionally designed to be UI-configured for most settings.
There is a possible way to carefully edit .storage/alarmo JSON files manually, but I would not reccomend it as mistakes can lead to breaking your HA config. I have had a situation once when dirty edits in HA files led to me restoring HA Backup through it's console CLI.

Basically in ALARMO you should configure Home 

📄 Example YAMLs for automation, triggers, and states will be included here.

### 🔧 Synchronization of Alarm states

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
- ✅ [ESP32 DIY CO₂ Temperature & Humidity Sensors / Cameras for HA](https://github.com/AlexeiakaTechnik/Engineering-DIY-ESP32-Sensors-and-Cameras-for-Home-Assistant)  
- 📄 [AJAX SIA Event Code Reference (Google Sheet)](https://docs.google.com/spreadsheets/d/1-N-RZVS8IiwM5zuw2u4gt8Bx_5xo_JOwuagHJgSJxUw/edit#gid=920971512)
