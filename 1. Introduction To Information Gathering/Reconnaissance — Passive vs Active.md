#  Reconnaissance — Passive vs Active

> [!abstract] Lesson Objective By the end of this note, you will understand:
> 
> - The **difference** between passive and active reconnaissance
> - A **repeatable 4-step recon strategy** used before any engagement
> - **What data to collect** and why it matters
> - **Common beginner mistakes** and how to avoid them

---

##  What is Reconnaissance?

Reconnaissance (recon) is the **first phase of any penetration test or ethical hacking engagement**. Before you ever run a single scan or exploit, you need to understand your target.

Think of it like a **detective investigating a crime scene** — you gather clues before jumping to conclusions. The more information you collect about a target, the better your attack surface map will be.

> [!tip] Simple Definition **Reconnaissance = Gathering information about a target before attacking it.**

---

##  The Two Types of Reconnaissance

```mermaid
graph TD
    A[" Reconnaissance"] --> B[" Passive Recon\n(No direct contact)"]
    A --> C[" Active Recon\n(Direct interaction)"]

    B --> B1["WHOIS Lookups"]
    B --> B2["DNS Record Queries"]
    B --> B3["Google Dorking"]
    B --> B4["Shodan / Censys"]
    B --> B5["Public Email Harvesting"]
    B --> B6["Social Media OSINT"]

    C --> C1["Ping Sweeps / Host Discovery"]
    C --> C2["Port Scanning (nmap)"]
    C --> C3["Service Version Detection"]
    C --> C4["DNS Zone Transfer Testing"]
    C --> C5["Web Directory Fuzzing"]
```

---

##  Passive Reconnaissance

### What is it?

Passive recon is gathering information **without ever directly touching the target system**. You're using third-party sources — public databases, search engines, archived data.

> [!info] Key Characteristics
> 
> -  **No direct connection** to the target's servers
> -  **Very low risk of detection** — the target never sees your traffic
> -  **Always done first** before active recon
> -  Completely legal when using public sources

---

###  What Data Can You Collect Passively?

|Category|What You Find|Example Tools|
|---|---|---|
|**Domain Info**|Who owns the domain, registration dates, registrar|WHOIS, ViewDNS|
|**DNS Records**|Subdomains, mail servers, IP addresses|DNSdumpster, dig|
|**Web Content**|Technologies used, pages, login portals|BuiltWith, Wappalyzer|
|**Search Engine**|Exposed files, login pages, sensitive info|Google Dorks|
|**Emails**|Staff email addresses for phishing|hunter.io, theHarvester|
|**Breaches**|Leaked passwords or credentials|HaveIBeenPwned|
|**Infrastructure**|Hosting provider, CDN, cloud services|Shodan, Censys|

---

> [!success] What You Learned Without Touching the Target
> 
> - Main IP address
> - Mail server location
> - A potentially vulnerable dev subdomain
> - Outdated Apache version
> - Admin email for social engineering

---

##  Active Reconnaissance

### What is it?

Active recon involves **directly sending traffic to the target**. This means you're probing, scanning, or querying the target's systems directly — and they might notice.

> [!warning] Key Characteristics
> 
> -  **Sends traffic directly** to target servers
> -  **Can be detected** by firewalls, IDS/IPS systems
> -  **Leaves logs** on the target system
> -  Done **after passive recon** to confirm and expand findings

---

###  What Data Can You Collect Actively?

|Category|What You Find|Example Tools|
|---|---|---|
|**Live Hosts**|Which IPs are actually online|nmap -sn, fping|
|**Open Ports**|Which doors are open on the server|nmap, masscan|
|**Services**|What is running on each port|nmap -sV|
|**OS Detection**|What operating system is running|nmap -O|
|**DNS Zone Transfer**|Full DNS zone if misconfigured|dig AXFR|
|**Web Directories**|Hidden pages and folders|gobuster, dirb|
|**Banners**|Service version info via banner grabbing|netcat, telnet|

---

> [!danger] What You Found by Actively Probing
> 
> - MySQL database is **publicly exposed on port 3306** — critical vulnerability
> - OpenSSH version has **known CVEs**
> - Dev subdomain has a **misconfigured DNS zone transfer**

---

##  Passive vs Active — Side-by-Side Comparison

```mermaid
graph LR
    subgraph PASSIVE[" PASSIVE RECON"]
        P1["No direct contact"]
        P2["Uses public sources"]
        P3["Hard to detect"]
        P4["Done FIRST"]
    end

    subgraph ACTIVE["🔌 ACTIVE RECON"]
        A1["Direct contact with target"]
        A2["Uses scanning tools"]
        A3["Leaves logs/traces"]
        A4["Done SECOND"]
    end

    PASSIVE --> |"After gathering intel"| ACTIVE
```

|Feature|Passive|Active|
|---|---|---|
|**Target Interaction**|None|Direct|
|**Detection Risk**|Very Low|Medium–High|
|**Speed**|Slower (research-based)|Faster (automated)|
|**Data Type**|Public info, metadata|Live system responses|
|**When to Use**|Always — start here|After passive recon|
|**Example Tool**|WHOIS, Google, Shodan|nmap, gobuster, dig|

---

##  Full Recon Mapping Flow

```mermaid
flowchart TD
    A["Define Target Scope"] --> B["Passive Reconnaissance"]

    B --> C["Active Reconnaissance"]
    B --> PR_BOX

    subgraph PR_BOX["Passive Reconnaissance"]
        direction TB
        P1["Domains & Subdomains"] --> P2["DNS Records"]
        P2 --> P3["Whois Data"]
        P3 --> P4["Website Footprinting"]
        P4 --> P5["OSINT: Search Engines,\nEmails, Breach Awareness"]
        P5 --> P6["Technology Fingerprinting"]
    end

    C --> AR_BOX
    C --> D["Organize Findings"]

    subgraph AR_BOX["Active Reconnaissance"]
        direction TB
        A1["Host Discovery"] --> A2["Port Scanning"]
        A2 --> A3["Basic Service Identification"]
        A3 --> A4["DNS Zone Transfer Testing"]
    end

    D --> E["Proceed to Enumeration & Exploitation"]

    style PR_BOX fill:#fffde7,stroke:#c8a800
    style AR_BOX fill:#fffde7,stroke:#c8a800
```

---

##  4-Step Recon Strategy

### Step 1 — Define Target Scope

Before doing anything, answer these questions:

> [!question] Scope Checklist
> 
> - What domain(s) am I allowed to test?
> - What IP ranges are in scope?
> - Are subdomains included?
> - Is cloud infrastructure (AWS, Azure) included?
> - Are there any off-limit systems?

---

### Step 2 — Passive Reconnaissance

Your goal: **build a picture of the target using only public data.**

```mermaid
mindmap
  root((Passive Recon Goals))
    Domain Info
      WHOIS records
      Registrar details
      Admin contacts
    DNS
      Subdomains
      MX records
      A/AAAA records
    Web Presence
      Technologies used
      CMS platform
      JavaScript frameworks
    People & Emails
      Staff emails
      LinkedIn profiles
      Social media
    Infrastructure
      Hosting provider
      CDN used
      Cloud services
    Breaches & Leaks
      HaveIBeenPwned
      Pastebin leaks
      GitHub dorking
```

---

### Step 3 — Active Reconnaissance

Your goal: **confirm what's actually live and what's exposed.**
- in here you  can find live hosts, do a full port scan ,Service and version detection, os Detection , DNS zone transfer attempt and many more

- 
---

### Step 4 — Document & Organize Findings

> [!important] Why Documentation Matters
>  Without proper documentation, you will:
> 
> - Repeat work you've already done
> - Miss findings when writing your report
> - Lose track of which hosts you've tested
> - Waste time during the exploitation phase


---

## Common Mistakes (and How to Avoid Them)

```mermaid
graph TD
    M1[" Starting scans without defining scope"]:::mistake --> F1[" Always confirm scope FIRST\nGet written permission"]:::fix
    M2[" Skipping passive recon"]:::mistake --> F2[" Passive before active — always\nYou might find everything you need"]:::fix
    M3[" Scanning everything blindly"]:::mistake --> F3[" Focus on relevant targets\nQuality over quantity"]:::fix
    M4[" Not documenting results"]:::mistake --> F4[" Take notes in real time\nUse a notes tool like Obsidian"]:::fix
    M5[" Trusting tool output blindly"]:::mistake --> F5[" Always verify manually\nTools produce false positives"]:::fix

    classDef mistake fill:#ffebee,stroke:#c62828,color:#b71c1c
    classDef fix fill:#e8f5e9,stroke:#2e7d32,color:#1b5e20
```

---

##  Key Takeaways

> [!summary] What You Should Remember
> 
> 1. **Scope first** — never start without knowing what you're allowed to touch
> 2. **Passive before active** — always gather public info before sending any traffic
> 3. **Recon is about mapping** — your goal is to understand the target, not attack it yet
> 4. **Document everything** — your notes are your map for the rest of the engagement
> 5. **Don't trust tools blindly** — verify interesting findings manually

---
