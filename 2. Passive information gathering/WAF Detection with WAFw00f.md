

> [!info] What is a WAF? A **Web Application Firewall (WAF)** is a security layer that monitors, filters, and blocks HTTP/HTTPS traffic between a web application and the internet. It protects against common attacks like **SQL Injection**, **XSS**, **LFI/RFI**, and more.

---

##  What is WAFw00f?

**WAFw00f** is an open-source fingerprinting tool used to detect and identify which WAF solution is protecting a target web application. It is pre-packaged in **Kali Linux**, making it immediately available for penetration testers.

---

##  How Does It Work?

WAFw00f uses a **3 stage detection approach**:

> [!note] Detection Stages
> 
> 1. **Stage 1 — Passive Analysis** Sends a normal HTTP request and analyses the response headers, cookies, and body. Many WAFs inject identifiable signatures here.
>     
> 2. **Stage 2 — Active Probing** If passive fails, it sends a series of _(potentially malicious)_ HTTP requests — such as payloads with common attack strings — and uses logic to identify WAF behaviour.
>     
> 3. **Stage 3 — Heuristic Guessing** If stage 2 fails, it analyses all collected responses using a heuristic algorithm to determine if _any_ WAF or security layer is present, even if it can't identify it by name.
>     

---

##  Installation

WAFw00f comes **pre installed on Kali Linux**. For other systems:

```bash
# Via pip
pip install wafw00f

# Or from GitHub
git clone https://github.com/EnableSecurity/wafw00f
cd wafw00f
python setup.py install
```

---

##  Basic Usage

### List All Detectable WAFs

```bash
wafw00f -l
```

This shows all WAF signatures WAFw00f can fingerprint (supports **180+ WAFs**).

---

### Detect WAF on a Target

```bash
wafw00f https://example.org
```

### Detect WAF — Aggressive Mode (recommended)

```bash
wafw00f https://example.org -a
```

> [!tip] Use `-a` (all) flag The `-a` flag tells WAFw00f to test with **all detection techniques**, not just the first match. This gives more accurate and complete results.

**Example Output:**

```
                   ______
                  /      \
                 (  Woof! )
                  \  ____/                      )
                  ,,                           ) (_
             .-. -    _______                 ( |__|
            ()``; |==|_______)                .)|__|
            / ('        /|\                  (  |__|
        (  /  )        / | \                  . |__|
         \(_)_))      /  |  \                   |__|

                    ~ WAFW00F : v2.4.2 ~
    The Web Application Firewall Fingerprinting Toolkit

[*] Checking https://example.org
[+] The site https://example.org is behind Edgecast (Verizon Digital Media) WAF.
[~] Number of requests: 2
```

> [!note] Detection Stages
> you can find a cool introduction about WAFW00F in there offical github repo
>I personally really liked the offical github, above example output also from that git hub repo

---

##  Useful Flags

|Flag|Description|
|---|---|
|`-a`|Test with all WAF detection techniques|
|`-l`|List all WAFs WAFw00f can detect|
|`-v`|Verbose output|
|`-o <file>`|Save output to a file|
|`-f <format>`|Output format: `json`, `csv`, `xml`|
|`--proxy`|Route requests through a proxy (e.g., Burp Suite)|

**Example — Save output as JSON:**

```bash
wafw00f https://example.org -a -o output.json -f json
```

---

##  Integration with Recon Workflow

> [!important] Connecting WAFw00f to Your Recon Chain WAFw00f fits into the **passive → active recon pipeline**:
> 
> ```
> DNSdumpster / DNSrecon
>       ↓
>   Subdomains & IPs discovered
>       ↓
>   WAFw00f — Check if WAF is present
>       ↓
>   If NO WAF → Data from DNS recon is likely more reliable (real IPs, not CDN-masked)
>   If WAF found → Adjust attack strategy accordingly
> ```

> [!warning] No WAF ≠ No Protection If WAFw00f returns **no WAF detected**, it does NOT mean the target is completely unprotected. There may still be network-level firewalls, IDS/IPS systems, or rate limiting in place. However, it does suggest that IP data gathered from tools like **DNSdumpster** and **DNSrecon** is more likely to be the **real origin IP** rather than a CDN or proxy address.

---

##  Why Use WAFw00f? (Key Reasons)

> [!success] Why WAFw00f is Valuable
> 
> **1. Know Before You Attack** Attempting SQL injection or XSS on a target with an active WAF will get your requests blocked and your IP flagged. Knowing a WAF is present lets you adapt your approach first.
> 
> **2. Identify the WAF Vendor** Different WAFs (Cloudflare, AWS WAF, Imperva, Akamai, etc.) have different bypass techniques. Fingerprinting the exact vendor helps you choose the right bypass strategy.
> 
> **3. Validate Recon Data** If no WAF is found, IPs gathered from passive recon tools are more likely real origin IPs — not CDN proxy addresses. This is critical for accurate network mapping.
> 
> **4. Efficient & Non-Intrusive** WAFw00f starts with passive checks (just HTTP headers), only escalating to active probing if needed. This keeps detection **low-noise** and harder to flag early on.
> 
> **5. Supports 180+ WAF Signatures** Covers virtually every major commercial and open-source WAF in use today — from Cloudflare and Sucuri to ModSecurity and F5 BIG-IP.
> 
> **6. Integrates with Other Tools** Output can be piped into JSON/CSV for use in automated scripts or combined with tools like **Burp Suite**, **Nikto**, or **SQLMap**.

---


