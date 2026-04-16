
#  Google Dorks

> [!info] What are Google Dorks? **Google Dorking** (aka Google Hacking) is the technique of using advanced Google search operators to find specific, often hard-to-find information indexed on the web. It's widely used in OSINT, penetration testing, and research.

---

##  Core Operators

|Operator|Description|Example|
|---|---|---|
|`site:`|Limit results to a domain|`site:example.com`|
|`intitle:`|Search in page title|`intitle:"login"`|
|`allintitle:`|All words must be in title|`allintitle:admin panel`|
|`inurl:`|Search in the URL|`inurl:admin`|
|`allinurl:`|All words must be in URL|`allinurl:wp-admin login`|
|`intext:`|Search in page body|`intext:"password"`|
|`allintext:`|All words in page body|`allintext:username password`|
|`filetype:`|Search by file type|`filetype:pdf`|
|`ext:`|Alias for filetype|`ext:sql`|
|`cache:`|View cached page|`cache:example.com`|
|`link:`|Pages linking to a URL|`link:example.com`|
|`related:`|Sites similar to a URL|`related:example.com`|
|`define:`|Get definition|`define:phishing`|
|`info:`|Info about a site|`info:example.com`|
|`before:`|Results before a date|`before:2023-01-01`|
|`after:`|Results after a date|`after:2022-06-01`|

---

##  Modifiers & Syntax

```
"exact phrase"       → exact match
-word                → exclude a word
*                    → wildcard
OR / |               → either term
()                   → group terms
..                   → number range (eg 2020..2024)
```

---

##  Common Dork Categories

###  Login Pages

```
inurl:login site:example.com
intitle:"Login" inurl:/admin
inurl:/wp-login.php
intitle:"Sign in" inurl:account
```

###  Exposed Files & Directories

```
intitle:"index of" "parent directory"
intitle:"index of" passwd
intitle:"index of" ".git"
inurl:"/phpmyadmin/" intitle:"phpMyAdmin"
filetype:log inurl:access.log
```

###  Database & Config Files

```
ext:sql intext:"INSERT INTO"
ext:env "DB_PASSWORD"
filetype:xml inurl:config
filetype:ini inurl:wp-config
ext:bak inurl:config
```

###  Spreadsheets & Documents

```
filetype:xls intext:"username" "password"
filetype:csv intext:"email" "phone"
filetype:pdf site:gov.lk
filetype:doc intitle:"confidential"
```

###  Subdomains & Infrastructure

```
site:*.example.com -www
inurl:dev.example.com
inurl:staging.example.com
intitle:"Apache2 Ubuntu Default Page" site:example.com
```

###  Exposed Cameras & Devices

```
inurl:"/view/index.shtml"
intitle:"Live View / - AXIS"
inurl:top.htm inurl:currenttime
inurl:/control/userimage.html
```

###  Admin & Control Panels

```
inurl:admin intitle:login
intitle:"admin panel" inurl:admin
inurl:/admin/login.php
intitle:"cPanel" inurl:2083
```

###  Email / Contact Harvesting

```
site:linkedin.com "@gmail.com" "company name"
filetype:xls "email address"
intext:"@example.com" filetype:csv
```

###  OSINT & People Search

```
"John Doe" site:linkedin.com
"John Doe" filetype:pdf resume
"@username" site:twitter.com
```

---

##  Combining Dorks

```
# Find exposed SQL files on a specific site
site:example.com ext:sql

# Exposed .env files on any site
inurl:".env" ext:env DB_PASSWORD

# Login pages with "admin" in URL, not the main domain
inurl:admin intitle:login -site:example.com

# PDF documents from government sites
filetype:pdf site:.gov "confidential"

# Directory listings with backup files
intitle:"index of" ext:bak OR ext:backup
```

---

##  Defensive Awareness

> [!warning] Protect Your Own Site
> 
> - Regularly [[Google Dork]] your own domain to find exposures
> - Use `robots.txt` to restrict sensitive paths
> - Implement proper authentication on admin panels
> - Avoid storing sensitive data in publicly accessible files
> - Use `.htaccess` or server rules to block directory listing
