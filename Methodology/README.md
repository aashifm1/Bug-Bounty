
## Bug Bounty Methodology

---

### 🔹 1. **Subdomain Enumeration**

**Goal**: Find all subdomains of the target.

#### Passive Subdomain Enumeration

```bash
# Amass
amass enum -passive -d target.com

# Assetfinder
assetfinder --subs-only target.com

# Subfinder
subfinder -d target.com

# Findomain
findomain -t target.com
```

#### Active Subdomain Enumeration

```bash
# Amass (active)
amass enum -active -d target.com

# DNSx (for bruteforce or probing)
dnsx -l subdomains.txt -silent
```

---

### 🔹 2. **Check Live Subdomains**

```bash
# HTTPX (multi-probe for alive domains)
cat subdomains.txt | httpx -silent -status-code -title -tech-detect > alive.txt
```

---

### 🔹 3. **Subdomain Takeover Check**

```bash
# Subjack
subjack -w alive.txt -t 100 -timeout 30 -ssl -c fingerprints.json -v -o takeover.txt

# Nuclei takeover templates
nuclei -l alive.txt -t nuclei-templates/takeovers/ -o takeovers.txt
```

---

### 🔹 4. **Port Scanning**

```bash
# Naabu
naabu -iL alive.txt -top-ports 100 -o ports.txt
```

---

### 🔹 5. **Directory and File Bruteforcing**

```bash
# Dirsearch
dirsearch -u https://example.com -e php,html,js,txt -w wordlist.txt

# FFUF
ffuf -u https://example.com/FUZZ -w wordlist.txt -e php,html,js,txt
```

---

### 🔹 6. **Parameter Discovery**

```bash
# ParamSpider
python3 paramspider.py --domain example.com

# Arjun (for hidden GET/POST params)
python3 arjun.py -u https://example.com
```

---

### 🔹 7. **JS File Analysis**

```bash
# Get JS files
katana -u https://example.com -jc | grep "\.js" > jsfiles.txt

# Analyze JS (manually or with LinkFinder)
python3 linkfinder.py -i jsfiles.txt -o cli
```

---

### 🔹 8. **Vulnerability Scanning**

```bash
# Nuclei (massive scanning engine)
nuclei -l alive.txt -t nuclei-templates/ -o nuclei-results.txt

# Nikto (for web vulnerabilities)
nikto -h https://example.com
```

---

### 🔹 9. **Wayback URLs / Archived Content**

```bash
# Waybackurls
waybackurls target.com > wayback.txt

# Gau (GetAllUrls)
gau target.com > gau.txt

# Combine and filter
cat wayback.txt gau.txt | sort -u > urls.txt
```

---

### 🔹 10. **XSS, LFI, SSRF, etc. Testing**

```bash
# XSS with Dalfox
dalfox file urls.txt -o xss.txt

# SSRF / LFI / RCE: Use Nuclei templates or ffuf with payloads
```

---

### 🔹 11. **GitHub Recon**

```bash
# Github-subdomains
github-subdomains -d target.com -t <GH_TOKEN>

# GitLeaks (for secrets)
gitleaks detect --source=.
```

---

### 🔹 12. **Technology Stack Fingerprinting**

```bash
# Wappalyzer CLI
wappalyzer https://example.com

# Webanalyze
webanalyze -host https://example.com
```

---

## ✅ Pro Tips

* **Use `tmux` or `screen`** to multitask recon tools.
* **Automation**: Script your recon using Bash or Python.
* **Data Management**: Organize by target: `~/recon/target.com/`
* **Use VPS** for long or noisy scans.

---

Let me know if you want an **automated recon script** or **private Bug Bounty recon tools**.

