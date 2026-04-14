#  Target Scoping in Penetration Testing


---

> [!abstract] Core Idea Target scoping is the process of **defining exactly what systems, networks, or applications you are authorized to test**. In penetration testing, you do not test everything you can find — you test only what has been explicitly permitted.

---

##  What is Target Scoping?

Target scoping answers one fundamental question during information gathering:

> **"What am I allowed to collect information about?"**

- You operate within a **Letter of Engagement (LOE)** — also called _Rules of Engagement (ROE)_ or _Terms of Engagement (TOE)_
- The LOE is a **legally binding document** that outlines the scope, methodology, and boundaries of the assessment
- Once signed, you **cannot test anything outside the defined scope** — no exceptions

> [!warning] Legal Reminder The scope defined in your LOE is **legally binding**. Testing out-of-scope assets — even accidentally — can result in serious legal consequences. You cannot back out once the engagement begins.

---

##  Common Target Types

During reconnaissance, targets are typically defined in one or more of the following categories:

### 1.  Domain-Based Targets

|Type|Example|
|---|---|
|Primary Domain|`example.com`|
|Subdomain|`mail.example.com`|
|Admin Panel|`example.com/admin`|

- A client may limit testing to a **specific subdomain only**, not the entire root domain
- Subdomains can explicitly be placed **out of scope**

---

### 2.  IP-Based Targets

|Type|Example|Common Use|
|---|---|---|
|Single IP|`192.168.1.1`|Specific host testing|
|Network Range|`192.168.2.0/24`|Lab or cloud environments|

- Network ranges with CIDR notation are common in **internal lab** or **cloud-based** environments

---

### 3.  Application-Based Targets

|Type|Example|
|---|---|
|Web Application|A specific site or portal|
|Mobile Application|Android / iOS app|
|Login Portal|An authentication endpoint|
|API Endpoint|`/api/v1/users`|

- Reconnaissance is **limited to that application only** — not the entire server or infrastructure

---

##  In Scope vs. Out of Scope

```
┌──────────────────────────────────────────────────────┐
│                    SCOPE BOUNDARY                    │
│                                                      │
│       IN SCOPE                   OUT OF SCOPE        │
│   ─────────────               ───────────────        │
│   • Listed domains            • Third-party services │
│   • Approved IP ranges        • Unlisted domains     │
│   • Defined applications      • Partner systems      │
│   • Authorized subdomains     • Out-of-scope IPs     │
│                                                      │
└──────────────────────────────────────────────────────┘
```

> [!tip] Rule of Thumb If it's not **explicitly listed as in-scope**, treat it as **out of scope**. When in doubt, ask the client before proceeding.

---

##  Why Scoping Matters

Without a well-defined scope, your reconnaissance will suffer from:

|Problem|Impact|
|---|---|
|Scanning irrelevant hosts|Wasted time and noisy results|
|Missing actual targets|Incomplete assessment|
|Touching out-of-scope assets|Legal liability|
|Unfocused data collection|Confusing reports|

**A well-defined scope keeps reconnaissance:**

-  **Focused** — you know exactly what to look at
-  **Efficient** — no wasted effort on irrelevant systems
-  **Relevant** — findings directly feed into later pentest phases

---

##  Scoping in the Recon Workflow

```
         [ Client Signs LOE / ROE ]
                    │
                    ▼
         [ Scope Document Defined ]
          ┌─────────────────────┐
          │  Domains / IPs /    │
          │  Applications       │
          └─────────────────────┘
                    │
         ┌──────────┴──────────┐
         ▼                     ▼
   [ In Scope ]          [ Out of Scope ]
   Proceed with          Do NOT interact
   Reconnaissance        with these assets
         │
         ▼
   [ Information Gathering Begins ]
   (Passive + Active Recon)
```


