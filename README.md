# TryHackMe: Extending Your Network — 2025 Newcomer Cheat Sheet (EchoLynx)

A fast, practical companion for the **Extending Your Network** room (Pre‑Security → Network Fundamentals). Exact Q&A, step‑by‑step flow, and careful clarifications — updated for **2025**.

---

## TL;DR — What you’ll do
- **Task 1:** Port forwarding basics (what it is vs. what it isn’t).
- **Task 2:** Firewalls — stateful vs. stateless; where they sit in OSI.
- **Task 3:** Practical firewall lab — block HTTP to the target and grab the flag.
- **Task 4:** VPN basics — tunneling + PPP / PPTP / IPSec.
- **Task 5:** LAN devices — routers vs. switches (L2 vs. L3) + VLANs.
- **Task 6:** Network simulator — send a TCP packet, count handshakes, capture the flag.

---

## Room Q&A (exact answers)
### Task 1 — Port Forwarding
- **What is the name of the device that is used to configure port forwarding?** → **router**

### Task 2 — Firewalls 101
- **What layers of the OSI model do firewalls operate at?** → **3 & 4**
- **What category of firewall inspects the entire connection?** → **stateful**
- **What category of firewall inspects individual packets?** → **stateless**

### Task 3 — Practical (Firewall)
- **Flag:** → **THM{FIREWALLS_RULE}**

### Task 4 — VPN Basics
- **What VPN technology only encrypts & provides the authentication of data?** → **PPP**
- **What VPN technology uses the IP framework?** → **IPSec**

### Task 5 — LAN Networking Devices
- **What is the verb for the action that a router does?** → **routing**
- **What are the two different layers of switches? (comma‑separated)** → **Layer 2,Layer 3**

### Task 6 — Network Simulator
- **What is the flag from the network simulator?** → **THM{YOU'VE_GOT_DATA}**
- **How many HANDSHAKE entries are there in the Network Log?** → **5**

---

## Step‑by‑step walkthrough
### Task 1 — Port forwarding (clear & concise)
- **What it does:** Publishes an **internal** service to the **Internet** by mapping a port on your **public IP** to a port on a **private IP** (e.g., `82.62.51.70:80 → 192.168.1.10:80`).
- **Where it lives:** Configured on the **router** at the network edge.
- **Why it matters:** Without it, internal services are only reachable from the same LAN (**intranet**).
- **Not a firewall rule:** Port forwarding **creates a path**; a **firewall** separately **allows/denies** traffic over that path.

> **2025 note:** This mapping is a form of **NAT** (Network Address Translation). Some ISPs use **CGNAT**, which can prevent inbound port‑forwarding from the public Internet.

### Task 2 — Firewalls 101 (stateful vs. stateless)
- **Stateless:** Checks **each packet** against fixed rules (src/dst IP, port, protocol). Very fast; no connection memory; great for simple filters / high‑volume scrubbing.
- **Stateful:** Tracks **whole connections** (e.g., TCP state). Smarter decisions, can block a bad **host/session**, not just a single packet; uses more resources.
- **Where in OSI (this room):** **Layers 3 & 4** (IP + TCP/UDP). *App‑aware/L7 firewalls exist in the real world, but are out of scope here.*

### Task 3 — Practical (firewall)
- **Goal:** Stop **malicious HTTP** to the web server **203.0.110.1**.
- **Rule idea:** **Deny** traffic with **dst IP = 203.0.110.1** **AND** **dst port = 80** (protocol TCP or Any per UI). Red packets get dropped; green traffic passes.
- **Flag:** **THM{FIREWALLS_RULE}**.

### Task 4 — VPN basics (tunneling)
- **What a VPN does:** Builds an **encrypted tunnel** over the Internet so devices on separate networks can communicate as if on a private link.
- **Why use it:** Site‑to‑site connectivity, secure use over public Wi‑Fi, and safe access to lab machines (as with TryHackMe’s VPN).
- **Tech in this room:**
  - **PPP:** Used by PPTP; handles **authentication**/**encryption** in this model; **non‑routable by itself**.
  - **PPTP:** Carries PPP across networks; easy to set up; **weaker** encryption than modern options.
  - **IPSec:** Encrypts using the **IP** framework; stronger but more complex to configure.

### Task 5 — LAN devices (router vs. switches, VLANs)
- **Router (L3):** Connects **networks** and performs **routing** (selects paths based on distance, reliability, link speed, etc.). Common place to configure NAT/port‑forwarding/firewall rules.
- **Switch:**
  - **Layer 2 switch:** Forwards **frames** on a LAN via **MAC** tables.
  - **Layer 3 switch:** Adds basic **routing** between VLANs/subnets while still switching at L2 — common for **inter‑VLAN routing**.
- **VLANs:** Virtual segmentation on the same physical switch; improves **organization** and **security** (e.g., Sales vs. Accounting isolation).

### Task 6 — Practical (network simulator)
- Use **Chrome/Firefox**. Send a **TCP** packet from **computer1 → computer3** (message like `hello`).
- Watch ARP + the TCP handshake and data transfer; capture the flag **THM{YOU'VE_GOT_DATA}**.
- In the **Network Log**, the count of **HANDSHAKE** entries is **5**.

---

## Acronyms & terms — plain English (2025)
- **NAT:** Network Address Translation (public ↔ private mapping); **port forwarding** is a static inbound NAT.
- **CGNAT:** Carrier‑Grade NAT; the ISP shares public IPs, which can **block inbound** port‑forwards.
- **Stateful vs. Stateless:** Remembers connections vs. per‑packet only.
- **VPN:** Virtual Private Network (encrypted tunnel over public networks).
- **PPP/PPTP/IPSec:** VPN building blocks in this room’s model.
- **VLAN:** Virtual LAN; logical segmentation on one switch.

---

## 2025 notes & good practice
- **Port‑forward + firewall:** Always pair the NAT mapping with the **least‑privilege** firewall rule (IP/port/protocol, logging).
- **Service exposure:** Prefer **reverse proxies**/**access controls** over exposing raw services to the Internet.
- **Site‑to‑site VPNs:** Plan IP schemes to avoid overlapping subnets; document ACLs between VLANs.
