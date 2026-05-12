## Live Report

[View full ZAP scan report](https://Manojgolla0516.github.io/web-vuln-scanner/ZAP-Report.html)

# web-vulnerability-scanner

Hands-on web vulnerability assessment using OWASP ZAP against DVWA (Damn Vulnerable Web Application) running locally on Kali Linux.

> ⚠️ **Ethical use only.** All scans were performed against DVWA — a deliberately vulnerable web application running on my local machine. No real websites were scanned.

---

## Tools Used

| Tool | Purpose |
|------|---------|
| **OWASP ZAP 2.17.0** | Automated web vulnerability scanner |
| **DVWA** | Deliberately vulnerable target app |
| **Kali Linux** | Security testing environment |

---

## Target

**DVWA (Damn Vulnerable Web Application)**
Running locally at `http://localhost/dvwa`
Security level set to: **Low**
Scan date: **12 May 2026**

---

## Scan Results

**Total alerts: 14**

| Risk | # | Vulnerability |
|------|---|--------------|
| 🟡 Medium | 4 | See below |
| 🟢 Low | 6 | See below |
| ℹ️ Informational | 4 | See below |

### 🟡 Medium Risk

| Vulnerability | What it means |
|--------------|---------------|
| Content Security Policy (CSP) Header Not Set | No CSP header — browser has no instructions on what content to trust. Makes XSS attacks easier. |
| Directory Browsing | Server exposes folder contents — attacker can see all files in a directory. |
| Missing Anti-Clickjacking Header | No `X-Frame-Options` header — site can be embedded in an iframe for clickjacking attacks. |
| Cookie without SameSite Attribute | Session cookies sent with cross-site requests — enables CSRF attacks. |

### 🟢 Low Risk

| Vulnerability | What it means |
|--------------|---------------|
| Cookie without SameSite Attribute | Cookie missing SameSite flag |
| Server Leaks Version Information | `Server` HTTP header reveals Apache version — gives attackers info about known vulnerabilities |
| Timestamp Disclosure - Unix | Unix timestamps leaked in responses — reveals internal server timing |

### ℹ️ Informational

| Finding | What it means |
|---------|---------------|
| Modern Web Application | Site uses modern JS frameworks |
| User Agent Fuzzer | Responses differ by user agent |
| Session Management Response | Session tokens present in responses |

---

## Screenshots

![ZAP Alerts](zap_alerts.png)

> Full detailed report: see `zap_report.html`

---

## What I Did

1. Installed and started DVWA on Kali Linux
2. Set DVWA security level to Low
3. Opened OWASP ZAP 2.17.0 → Automated Scan
4. Entered target: `http://localhost/dvwa`
5. Clicked Attack — scan ran for ~5 minutes
6. Reviewed 14 alerts across Medium / Low / Informational categories
7. Exported full HTML report

---

## What I Learned

### CSP Header Not Set
Content Security Policy is a browser security feature that tells the browser
which sources of scripts, styles, and images are trusted. Without it, any
injected script can run freely — making XSS attacks much easier to exploit.

**Fix:** Add `Content-Security-Policy` header to all server responses.

### Directory Browsing
The web server is configured to show the contents of directories when
no index file is present. This lets attackers browse the file structure
and discover sensitive files.

**Fix:** Disable directory listing in Apache config — `Options -Indexes`

### Missing Anti-Clickjacking Header
Without `X-Frame-Options`, the site can be embedded inside an invisible
iframe on a malicious page. The attacker overlays it on a fake page and
tricks users into clicking hidden buttons.

**Fix:** Add `X-Frame-Options: DENY` or `SAMEORIGIN` header.

### Server Version Disclosure
The `Server` HTTP response header reveals `Apache/2.4.66 (Debian)`.
This tells attackers exactly which version is running and lets them
look up known CVEs for that version.

**Fix:** Remove or obscure the Server header in Apache config.

---

## Key Takeaways

- Even with 0 High severity alerts, Medium findings are serious and exploitable
- Missing security headers are the most common and easiest-to-fix issues
- Directory browsing is a misconfiguration that should always be disabled
- Server version disclosure gives attackers a free head start

---

## Environment

- OS: Kali Linux (VirtualBox)
- Tool: OWASP ZAP 2.17.0
- Target: DVWA (local)
- Date: May 2026

---

## Disclaimer

All testing was performed against a deliberately vulnerable application
running on my own local machine. No real websites were scanned.

---

## License

MIT
