# Wireless Security Labs

**Devante Metoyer · Late 2024**

Hands-on labs studying WiFi security with a Hak5 WiFi Pineapple Mark VII. The focus of these notes is on **what the exercises revealed about wireless weaknesses and how to defend against them**, not on reproducing attacks.

> **Scope and ethics:** reconnaissance was passive only; the active exercises were performed against my own network and devices. Screenshots are not published because the survey captured other people's network names and device identifiers.

---

## 1. Passive reconnaissance: what a wireless network leaks

A short passive survey of the surrounding airspace surfaces a surprising amount of information without ever connecting to anything: access point names (SSIDs), encryption types in use, channels, relative signal strength, and the fact that client devices continuously announce the names of networks they have joined before.

**Why it matters for defense:**
- The list of networks a phone or laptop remembers is broadcast in the clear. That history is an information leak about where a device (and its owner) has been.
- Encryption type and channel are visible to anyone, so a network's security posture is public by default.

**Defensive takeaways:**
- Turn off auto-join for open and public networks, and have devices forget networks that are no longer used, so they stop advertising that history.
- Prefer WPA3 (or WPA2 at minimum) and disable legacy open/WEP configurations.
- Treat the SSID list as public information when threat-modeling.

## 2. Deauthentication / denial-of-service: why availability is the weak link

Classic WiFi management frames (the messages that join and leave a network) were historically **unauthenticated**. The lab demonstrated the practical effect of that design gap: a target device on my own network was repeatedly knocked off and forced to reconnect, degrading its connectivity.

**Why it matters for defense:**
- The attack needs no password and no prior access. It targets availability, not confidentiality.
- It affects both the access point and its clients.

**Defensive takeaways:**
- Enable **802.11w (Protected Management Frames)**, which authenticates management frames and closes the original gap. It is mandatory under WPA3.
- Monitor for abnormal volumes of deauthentication/disassociation activity as an indicator of interference.
- For critical links, don't rely on WiFi alone where a wired or cellular fallback is feasible.

## 3. Hardening an access point: the limits of MAC filtering

I configured an access point with WPA2 and a MAC-address client filter, verified that a filtered device was blocked, then removed the filter and confirmed the device could rejoin.

**What the exercise showed:**
- MAC filtering can block a specific, known device, but it is a **deny-list control**: once the entry is removed (or the filter isn't active) there is no protection, and MAC addresses themselves are visible and can be changed.
- Even a well-configured AP still broadcasts its SSID, BSSID, channel and security type, and client presence patterns remain observable.

**Defensive takeaways:**
- Treat MAC filtering as a minor, defense-in-depth layer, never as a primary access control.
- Base access control on strong authentication (WPA2/WPA3 with a strong passphrase, or 802.1X/EAP for enterprise networks).
- Assume metadata (who is connected, when, how often) is observable, and segment sensitive systems off the general wireless network.

---

## Summary

| Lab | Weakness studied | Primary defense |
|---|---|---|
| Passive recon | Information leakage from SSIDs and probe behavior | WPA3, disable auto-join, forget stale networks |
| Deauth / DoS | Unauthenticated management frames | 802.11w Protected Management Frames |
| AP hardening | MAC filtering is a weak, removable control | Strong auth (WPA2/WPA3, 802.1X), network segmentation |

## Author

Devante Metoyer · [LinkedIn](https://linkedin.com/in/devantedmetoyer)

Text licensed under [CC BY 4.0](../LICENSE).
