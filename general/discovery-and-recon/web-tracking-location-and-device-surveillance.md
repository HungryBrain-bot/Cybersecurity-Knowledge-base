---
title: "Web Tracking, Location and Device Surveillance"
domain: [General]
category: [Discovery & Recon]
difficulty: Beginner
verification: Verified
attack_ids: []
checked_against:
  - https://www.w3.org/privacy/
  - https://www.cisa.gov/secure-our-world
open_issues: []
---

# Web Tracking, Location and Device Surveillance

## Summary

Modern devices and the web **track users pervasively**, and understanding the mechanisms matters for privacy, for recognizing how the same techniques enable targeted attacks, and for investigations. **Web tracking** identifies and follows users across sites via: **cookies** (especially third-party cookies), **browser/device fingerprinting** (combining browser, OS, fonts, screen, and hardware attributes into a near-unique identifier that works even without cookies), **tracking pixels and scripts** (embedded by advertisers/analytics), and **cross-site identifiers**. **Location tracking** comes from **GPS**, **Wi-Fi and Bluetooth** proximity, **cell-tower triangulation**, and **IP geolocation**, plus the location metadata embedded in photos (EXIF) and app data. **Device surveillance** spans legitimate telemetry, app permissions (microphone, camera, contacts, location), and — at the malicious end — **stalkerware/spyware** that covertly monitors a device. The privacy implications are significant: this data is aggregated into detailed profiles, sold or breached, and can reveal identity, movements, habits, and associations. The security implications are twofold: the same tracking/surveillance capabilities are used **offensively** (reconnaissance, de-anonymization, targeting for phishing, and stalkerware against individuals), and awareness supports **defense** (minimizing exposure through privacy settings, tracker blocking, permission hygiene, and anti-surveillance practices). For investigators, tracking artifacts and location data are also **evidence sources**. The core insight is that users leave extensive, often invisible trails through everyday device and web use; these trails are valuable to advertisers, attackers, and investigators alike, so managing one's digital exposure — and understanding how tracking works — is both a privacy and a security necessity.

## Tracking and surveillance mechanisms

| Mechanism | How it tracks |
|---|---|
| **Cookies (3rd-party)** | Follow users across sites |
| **Fingerprinting** | Near-unique profile from device/browser attributes |
| **Pixels/scripts** | Advertiser/analytics embeds |
| **GPS/Wi-Fi/cell/IP** | Location from multiple signals |
| **App permissions / stalkerware** | Device-level monitoring |

Everyday use leaves invisible trails valuable to advertisers, attackers, and investigators alike — managing exposure is both a privacy and a security need.

## Security relevance

- **De-anonymization**: fingerprinting identifies users even without cookies or logins.
- **Targeting**: tracking and location data support reconnaissance and tailored phishing.
- **Stalkerware**: covert monitoring is a real threat to individuals (e.g. domestic abuse).
- **Evidence**: tracking and location artifacts can place a device/user in time and space.

## Defense / practices

- **Reduce tracking**: block third-party cookies and trackers; use privacy-respecting browsers/settings.
- **Manage app permissions**: grant location/mic/camera only when needed; review regularly.
- **Strip location metadata** from shared photos; limit location sharing.
- **Watch for stalkerware**: unexpected battery/data use, unknown apps; use reputable security tools.
- **Understand fingerprinting**: recognize that cookie-blocking alone doesn't stop all tracking.

## Explanation

Every time you browse the web or carry your phone, you're quietly broadcasting a trail — and a whole industry, plus the occasional attacker, is built on collecting it. On the web, the oldest tracker is the cookie, especially the third-party kind that lets advertisers follow you from site to site. But even if you block cookies, you're not invisible: "fingerprinting" quietly notes dozens of little details about your browser and device — your screen size, fonts, operating system, and so on — and combines them into a signature that's often unique to you. Invisible tracking pixels and scripts baked into web pages do the watching. Your location leaks from several directions at once: GPS, the Wi-Fi and Bluetooth signals around you, which cell towers your phone talks to, your IP address, and even the hidden GPS tags inside the photos you post. And your devices themselves can be surveilled — sometimes legitimately through app permissions you granted, and sometimes maliciously through "stalkerware" secretly installed to spy on someone. All of this gets stitched into detailed profiles that reveal who you are, where you go, and who you associate with — profiles that get sold, leaked, and, in the wrong hands, used to find targets for scams or to stalk individuals. The same trails are also evidence that can place a device somewhere at a certain time. The practical upshot is that privacy isn't automatic; it takes deliberate choices — blocking trackers, tightening app permissions, stripping location tags from photos, and staying alert for spyware — because the default state of modern devices is to leave a remarkably detailed, mostly invisible record of your life.
