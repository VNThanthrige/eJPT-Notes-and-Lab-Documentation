

##  What is theHarvester?

**theHarvester** is a powerful, open-source **OSINT (Open Source Intelligence)** tool built for the **reconnaissance phase** of penetration testing and red team engagements.

It gathers publicly available information about a target domain by querying multiple search engines and online services — without ever directly touching the target.

> [!info] Think of it like this theHarvester acts as an **automated Google dorker** — it queries public search engines on your behalf to find anything the target has unintentionally exposed online.

---

##  What Can It Find?

|Data Type|Description|
|---|---|
| **Email Addresses**|Leaked or indexed emails linked to the domain|
| **Subdomains**|Subdomains discovered via search engine indexing|
| **IP Addresses**|IPs associated with the target|
| **ASNs**|Autonomous System Numbers (network ownership info)|
| **URLs**|Publicly indexed URLs belonging to the domain|
| **Names**|Employee names sometimes found in indexed content|

---

##  Installation & Command

theHarvester comes **pre-installed on Kali Linux**.

> [!warning] Correct Command Name The command is **theHarvester** — NOT `theharvester` (case-sensitive on some systems).

---

##  Basic Usage

```
theHarvester -d NAME.com -b duckduckgo,brave,baidu,yahoo,urlscan
```

**Or without the `.com` extension:**

```
theHarvester -d NAME -b duckduckgo,brave,baidu,yahoo,urlscan
```

###  Flag Breakdown

|Flag|Argument|Purpose|
|---|---|---|
|`-d`|`target.com`|**Domain** to search for|
|`-b`|`source1,source2`|**Data sources** (search engines/services) to use|
|`-l`|`number`|Limit number of results per source|
|`-f`|`filename`|Save results to HTML/XML file|
|`-s`|—|Start from a specific result number|

---

##  Supported Data Sources (for `-b` flag)

> [!tip] Use multiple sources together for broader coverage

### Search Engines

- `google` — Google Search
- `bing` — Microsoft Bing
- `yahoo` — Yahoo Search
- `duckduckgo` — DuckDuckGo
- `brave` — Brave Search
- `baidu` — Chinese search engine (often reveals different results)

### Specialized OSINT Services

- `urlscan` — Scans and indexes URLs
- `shodan` — IoT/device search engine _(requires API key)_
- `hunter` — Email finder service _(requires API key)_
- `securityTrails` — DNS/subdomain database _(requires API key)_
- `crtsh` — Certificate transparency logs (great for subdomains)
- `virustotal` — Threat intelligence platform _(requires API key)_
- `anubis` — Subdomain enumeration service
- `rapiddns` — Rapid DNS lookup

### Use All Available Sources

```
theHarvester -d target.com -b all
```

---

##  Example Commands

### Basic Email & Subdomain Hunt

```
theHarvester -d example.com -b google,bing,yahoo,duckduckgo
```

### Broad Multi-Source Recon

```
theHarvester -d example.com -b duckduckgo,brave,baidu,yahoo,urlscan
```

### Save Output to File

```
theHarvester -d example.com -b all -f results_example
```

> This saves `results_example.html` and `results_example.xml`

### Limit Results (faster for testing)

```
theHarvester -d example.com -b google -l 100
```

### Certificate Transparency (for subdomain discovery)

```
theHarvester -d example.com -b crtsh
```

---


##  theHarvester vs Sublist3r

|Feature|theHarvester|Sublist3r|
|---|---|---|
|**Primary Goal**|Emails + broader OSINT|Subdomains only|
|**Email Enumeration**| Yes| No|
|**Subdomain Discovery**| Yes| Yes (focused)|
|**IP / ASN Discovery**| Yes| No|
|**Sources Used**|Search engines + OSINT APIs|Search engines + DNS|
|**Best For**|Full passive recon|Deep subdomain mapping|

> [!tip] It is better to Use **both tools together** — Sublist3r for deep subdomain enumeration and theHarvester for email + broader intelligence gathering.

---
