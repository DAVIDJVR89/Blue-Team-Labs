# TryHackMe — Jr Security Analyst Intro

**Room:** [Jr Security Analyst Intro](https://tryhackme.com/room/jrsecanalystintrouxo)
**Category:** Blue Team / SOC Fundamentals
**Date:** July 2026

---

## 🎯 Objective

Introduction to the Tier 1 SOC Analyst role: using a simulated SIEM dashboard to identify malicious activity, investigate a suspicious IP, escalate the incident, and apply an immediate containment measure.

## 🖥️ Environment

Simulated SIEM dashboard with 4 modules: **Alerts**, **IP Hunter**, **Escalation**, and **Firewall** — replicating the typical workflow of a real SOC (monitoring → triage → investigation → escalation → containment).

---

## 🔍 Investigation Process

### 1. Identifying the critical alert

The Alerts dashboard already displayed alerts sorted by severity (Critical, Medium, Low). I reviewed the list and identified the two alerts marked as *Critical*, both related to the same IP: several unauthorized SSH login attempts, followed by a successful login.

This pattern (multiple failed attempts followed by a successful access within a short time window) is the classic signature of a **brute-force attack** against the SSH service. Recognizing this sequence came faster thanks to my offensive background (eJPTv2): having practiced this type of attack from the attacker's side (for example, using tools like Hydra against SSH services) allows me to identify its footprint in the logs with more context than if I only knew the defensive theory. Seeing the attack "from the other side" helps understand what it looks like in the SIEM: a burst of failed attempts + a final success from the same IP.

*(<img width="1206" height="442" alt="image" src="https://github.com/user-attachments/assets/27ac6f08-7a38-48f5-9958-e69eb39ea35c" />)*

### 2. Investigating the suspicious IP

I used the *IP Hunter* module of the SIEM to analyze the IP flagged in the critical alert, obtaining the ISP, Domain Name and Country and confirming that the IP has been involved in previous Cyber Attacks.

To reinforce the investigation, I also cross-checked the IP independently on [WHOIS Domain Tools](https://whois.domaintools.com/), reviewing its WHOIS record (owning organization, country of origin, associated network range). Although the SIEM already provided sufficient context, I consider it good practice to **verify findings with an external source** before confirming an IP as malicious — especially in a real environment, where an incorrect attribution could lead to blocking legitimate traffic or escalating a false positive.

Both sources agreed that the IP did not belong to an expected range for the organization, reinforcing the conclusion of unauthorized activity.


### 3. Determining the escalation point

With the IP confirmed as malicious through both sources, I used the *Escalation* module to determine the correct team/role to report the incident to, following the principle that a Tier 1 analyst does not resolve the incident themselves, but documents it with clear evidence and passes it on to the appropriate level based on the type of threat.


### 4. Containment: blocking the IP

As an immediate containment measure, I accessed the *Firewall* module and added the malicious IP to the **block list**, categorizing the reason as *Brute-Forced SSH*. Documenting the reason for the block (not just blocking without context) is an important practice in a real SOC: it leaves a clear record for the Tier 2 / Incident Response team explaining why that action was taken, avoiding ambiguity if someone reviews the firewall later.

This action closes the full incident cycle covered in this room: **detection → investigation → escalation → containment**, which is the basic flow followed by any SOC analyst when facing a confirmed threat.

*(Insert screenshot here — Firewall block list showing the entry)*

---

## 🧠 Key Concepts Learned

- Alert prioritization by severity (triage)
- Recognizing attack patterns (brute force) directly in SIEM logs/alerts
- Cross-verifying indicators with external OSINT sources (WHOIS) before confirming a threat
- The difference between detection (identifying) and response (escalating) — the Tier 1 role focuses on the former
- Applying immediate containment measures (firewall block) with clear documentation of the reason
- Structured escalation process as part of the SOC workflow

## 📌 Reflection

Although this room is introductory, it reinforces the full incident response cycle: identify, investigate, escalate, and contain. My previous offensive background (eJPTv2) proved practically useful here: understanding how a brute-force attack is executed helps recognize it faster in a SIEM, and complementing the investigation with external sources like WHOIS reinforces the habit of not relying on a single tool when confirming a threat. Seeing the process from start to finish — including the final containment step via a documented firewall block — helps clarify why each step matters: detecting is not enough, you also need to act and leave a clear record of that action.

---

*Part of my [Blue Team / SOC Practice](https://github.com/DAVIDJVR89) portfolio, documenting hands-on defensive security practice alongside my ongoing BTL1 (Blue Team Level 1) certification.*
