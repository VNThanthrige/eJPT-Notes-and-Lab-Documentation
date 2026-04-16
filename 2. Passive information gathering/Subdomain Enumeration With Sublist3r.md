
---
##  What is Sublist3r?

Sublist3r is a **Python-based OSINT tool** designed to enumerate subdomains of a target domain. It is widely used by penetration testers and bug bounty hunters during the **reconnaissance phase**.

>  **Note:** Results are **not 100% guaranteed** because passive enumeration depends on publicly indexed data.

---

##  How It Works

Sublist3r queries multiple **search engines and passive DNS sources** to collect subdomains without sending direct traffic to the target.

---

##  Command-Line Options

|Short Flag|Long Flag|Description|
|---|---|---|
|`-d`|`--domain`|Domain name to enumerate subdomains of|
|`-b`|`--bruteforce`|Enable the subbrute bruteforce module|
|`-p`|`--ports`|Scan found subdomains against specific TCP ports|
|`-v`|`--verbose`|Enable verbose mode and display results in real-time|
|`-t`|`--threads`|Number of threads to use for subbrute bruteforce|
|`-e`|`--engines`|Specify a comma-separated list of search engines|
|`-o`|`--output`|Save the results to a text file|
|`-h`|`--help`|Show the help message and exit|

---

##  Usage Examples

### Basic Subdomain Enumeration

```bash
sublist3r -d example.org
```

### Verbose Output (Real-Time Results)

```bash
sublist3r -d example.org -v
```

### Save Results to File

```bash
sublist3r -d example.org -o results.txt
```

### Use Specific Search Engines Only

```bash
sublist3r -d example.org -e google,bing,virustotal
```

### Enable Bruteforce Module with Threads

```bash
sublist3r -d example.org -b -t 50
```

---

##  Installation

```bash
# Clone the repository
git clone https://github.com/aboul3la/Sublist3r.git
cd Sublist3r

# Install dependencies
pip install -r requirements.txt

# Run directly
python sublist3r.py -d example.com
```

> On Kali Linux, Sublist3r may already be pre-installed and callable as `sublist3r`.


---
