 <div align="center">

<img src="https://img.shields.io/badge/Cybersecurity-Internship-0D1B2A?style=for-the-badge&logo=shield&logoColor=white"/>

# 🔐 Cybersecurity Internship — Weeks 4–6

### Advanced Threat Detection · Ethical Hacking · Security Audits & Secure Deployment

[![Week 4](https://img.shields.io/badge/Week%204-Threat%20Detection%20%26%20API%20Security-2E86C1?style=flat-square)](##week-4)
[![Week 5](https://img.shields.io/badge/Week%205-Ethical%20Hacking%20%26%20SQLi%20%2F%20CSRF-7D3C98?style=flat-square)](##week-5)
[![Week 6](https://img.shields.io/badge/Week%206-Security%20Audits%20%26%20Deployment-117A65?style=flat-square)](##week-6)
[![Node.js](https://img.shields.io/badge/Node.js-20%2B-339933?style=flat-square&logo=nodedotjs)](https://nodejs.org)
[![Docker](https://img.shields.io/badge/Docker-Secured-2496ED?style=flat-square&logo=docker)](https://docker.com)
[![OWASP](https://img.shields.io/badge/OWASP-Top%2010%20Compliant-000000?style=flat-square)](https://owasp.org)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

</div>

---

## 📋 Table of Contents

- [📖 Project Overview](#-project-overview)
- [🛠️ Tech Stack & Tools](#%EF%B8%8F-tech-stack--tools)
- [📁 Repository Structure](#-repository-structure)
- [🔵 Week 4 — Advanced Threat Detection & Web Security](#-week-4--advanced-threat-detection--web-security)
- [🟣 Week 5 — Ethical Hacking & Exploiting Vulnerabilities](#-week-5--ethical-hacking--exploiting-vulnerabilities)
- [🟢 Week 6 — Advanced Security Audits & Final Deployment](#-week-6--advanced-security-audits--final-deployment)
- [🏆 OWASP Top 10 Compliance](#-owasp-top-10-compliance)
- [🎯 Bonus Challenges](#-bonus-challenges)
- [⚙️ Setup & Installation](#%EF%B8%8F-setup--installation)
- [🔑 Environment Variables](#-environment-variables)
- [🚀 Running the Application](#-running-the-application)
- [🐳 Docker Deployment](#-docker-deployment)
- [📊 Security Audit Reports](#-security-audit-reports)
- [📸 Screenshots](#-screenshots)
- [📝 License](#-license)

---

## 📖 Project Overview

This repository contains the complete implementation of a **3-week cybersecurity internship programme** covering real-world security engineering practices. The project progressively hardens a Node.js/Express REST API from an insecure baseline to a fully audited, production-ready secured application.

| Week | Focus | Key Deliverable |
|------|-------|----------------|
| **Week 4** | Threat Detection & API Security | Secured API with intrusion detection |
| **Week 5** | Ethical Hacking & Vulnerability Fixes | Exploitation + remediation report |
| **Week 6** | Security Audits & Secure Deployment | Fully audited, Dockerized application |

> ⚠️ **Disclaimer:** All penetration testing, SQL injection, and hacking tasks were performed **exclusively** on local, self-hosted test environments (DVWA on localhost). No real systems were targeted.

---

## 🛠️ Tech Stack & Tools

<div align="center">

| Category | Tools / Technologies |
|----------|---------------------|
| **Runtime** | Node.js 20+, Express.js |
| **Security Middleware** | Helmet.js, express-rate-limit, csurf, cors |
| **Intrusion Detection** | Fail2Ban, OSSEC |
| **Penetration Testing** | Kali Linux, Burp Suite, Metasploit, SQLMap |
| **Vulnerability Scanning** | OWASP ZAP, Nikto, Lynis |
| **Dependency Scanning** | npm audit, Snyk, Trivy |
| **Containerization** | Docker, Docker Compose |
| **Database** | MySQL (with mysql2/promise) |
| **Test Environment** | DVWA (Damn Vulnerable Web App) |

</div>

---

## 📁 Repository Structure

```
cybersecurity-internship/
│
├── 📂 src/
│   ├── index.js                  # Main Express application
│   ├── 📂 middleware/
│   │   ├── apiKeyAuth.js         # API key authentication
│   │   ├── rateLimiter.js        # Rate limiting config
│   │   └── csrfProtection.js     # CSRF middleware
│   ├── 📂 routes/
│   │   ├── auth.js               # Auth endpoints
│   │   └── api.js                # Protected API routes
│   └── 📂 db/
│       └── queries.js            # Prepared statement queries
│
├── 📂 config/
│   ├── fail2ban/
│   │   └── jail.local            # Fail2Ban configuration
│   ├── docker-compose.yml        # Secure Docker Compose
│   └── Dockerfile                # Hardened Dockerfile
│
├── 📂 reports/
│   ├── zap_report.html           # OWASP ZAP scan report
│   ├── nikto_report.html         # Nikto scan report
│   ├── trivy_report.json         # Trivy container scan
│   ├── week5_ethical_hacking.md  # Ethical hacking report
│   └── week6_pentest_report.md   # Final pen test report
│
├── 📂 docs/
│   ├── week4_report.docx
│   ├── week5_report.docx
│   └── week6_report.docx
│
├── .env.example                  # Environment variables template
├── .gitignore
└── README.md
```

---

## 🔵 Week 4 — Advanced Threat Detection & Web Security

**Goal:** Implement advanced security measures, detect threats in real-time, and secure API endpoints.

### ✅ Task 1 — Intrusion Detection & Monitoring (Fail2Ban)

Set up real-time intrusion detection to automatically ban IPs after repeated failed login attempts.

```bash
# Install Fail2Ban
sudo apt install fail2ban -y

# Configure jails
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

```ini
# jail.local configuration
[sshd]
enabled  = true
maxretry = 5
bantime  = 3600
findtime = 600

[http-auth]
enabled  = true
maxretry = 5
bantime  = 3600
```

```bash
# Start service and verify
sudo systemctl enable --now fail2ban
sudo fail2ban-client status sshd
```

---

### ✅ Task 2 — API Security Hardening

**Rate Limiting** — Prevents brute-force and DoS attacks:

```js
const rateLimit = require('express-rate-limit');

const limiter = rateLimit({
  windowMs : 15 * 60 * 1000,   // 15 minutes
  max      : 100,               // 100 requests per window
  message  : 'Too many requests, please try again later.'
});

app.use('/api/', limiter);
```

**CORS Restriction** — Whitelist trusted origins only:

```js
const corsOptions = {
  origin         : ['https://yourtrustedsite.com'],
  methods        : ['GET', 'POST'],
  allowedHeaders : ['Content-Type', 'Authorization']
};

app.use(cors(corsOptions));
```

**API Key Authentication** — Every `/api/` request requires a valid key:

```js
function apiKeyAuth(req, res, next) {
  const key = req.headers['x-api-key'];
  if (!key || key !== process.env.API_KEY) {
    return res.status(401).json({ error: 'Unauthorized' });
  }
  next();
}
```

---

### ✅ Task 3 — Security Headers & CSP

```js
const helmet = require('helmet');

// All default security headers
app.use(helmet());

// Content Security Policy
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc     : ["'self'"],
    scriptSrc      : ["'self'"],
    objectSrc      : ["'none'"],
    frameAncestors : ["'none'"],
  }
}));

// HTTP Strict Transport Security
app.use(helmet.hsts({
  maxAge            : 31536000,
  includeSubDomains : true,
  preload           : true,
}));
```

> 🔗 Verify headers at [securityheaders.com](https://securityheaders.com) — aim for **A+**

---

## 🟣 Week 5 — Ethical Hacking & Exploiting Vulnerabilities

**Goal:** Learn ethical hacking techniques, exploit vulnerabilities in a test environment, and enhance application security.

### ✅ Task 1 — Ethical Hacking Basics (Reconnaissance)

```bash
# Set up DVWA test target
docker run -d -p 80:80 vulnerables/web-dvwa

# Nmap port & service scan
nmap -sV -A -T4 localhost

# Directory enumeration
gobuster dir -u http://localhost -w /usr/share/wordlists/dirb/common.txt

# HTTP header inspection
curl -I http://localhost
```

---

### ✅ Task 2 — SQL Injection & Exploitation

**Exploitation with SQLMap:**

```bash
sqlmap -u "http://localhost/dvwa/vulnerabilities/sqli/?id=1&Submit=Submit" \
  --cookie="PHPSESSID=YOUR_ID; security=low" \
  --dbs

# Dump users table
sqlmap -u "..." --cookie="..." -D dvwa -T users --dump
```

**Fix — Prepared Statements (mysql2):**

```js
// ❌ VULNERABLE
const query = `SELECT * FROM users WHERE username = '${username}'`;

// ✅ SECURE
const [rows] = await connection.execute(
  'SELECT * FROM users WHERE username = ? AND password = ?',
  [username, password]
);
```

---

### ✅ Task 3 — CSRF Protection

```js
const csrf         = require('csurf');
const cookieParser = require('cookie-parser');

app.use(cookieParser());
const csrfProtection = csrf({ cookie: true });

// Token generation
app.get('/form', csrfProtection, (req, res) => {
  res.json({ csrfToken: req.csrfToken() });
});

// Token validation (automatic)
app.post('/submit', csrfProtection, (req, res) => {
  res.json({ message: 'Success' });
});

// Error handler
app.use((err, req, res, next) => {
  if (err.code === 'EBADCSRFTOKEN') {
    return res.status(403).json({ error: 'Invalid CSRF token' });
  }
  next(err);
});
```

> **Tested with Burp Suite Repeater** — confirmed `403 Forbidden` when token is missing or tampered.

---

## 🟢 Week 6 — Advanced Security Audits & Final Deployment

**Goal:** Conduct advanced security audits, ensure OWASP compliance, and prepare for secure deployment.

### ✅ Task 1 — Security Audits

```bash
# OWASP ZAP — Web vulnerability scan
zap-baseline.py -t http://localhost:3000 -r reports/zap_report.html

# Nikto — Server misconfiguration scan
nikto -h http://localhost:3000 -o reports/nikto_report.html -Format html

# Lynis — Linux server hardening audit
sudo lynis audit system
sudo grep 'hardening_index' /var/log/lynis-report.dat
```

---

### ✅ Task 2 — Secure Deployment

**Hardened Dockerfile:**

```dockerfile
FROM node:20-alpine

# Non-root user
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .

USER appuser
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -qO- http://localhost:3000/health || exit 1

CMD ["node", "index.js"]
```

**Container Image Scanning (Trivy):**

```bash
trivy image --severity HIGH,CRITICAL your-app-image:latest
```

**Dependency Scanning:**

```bash
npm audit
npm audit fix
snyk test
```

---

### ✅ Task 3 — Final Penetration Testing

```bash
# Metasploit port scan
msf6 > use auxiliary/scanner/portscan/tcp
msf6 > set RHOSTS 127.0.0.1
msf6 > run

# HTTP version detection
msf6 > use auxiliary/scanner/http/http_version
msf6 > set RHOSTS 127.0.0.1 ; set RPORT 3000 ; run

# Save report
msf6 > spool /home/user/msf_pentest_report.txt
```

---

## 🏆 OWASP Top 10 Compliance

| # | Category | Control Applied | Status |
|---|----------|----------------|--------|
| A01 | Broken Access Control | Auth middleware on all routes | ✅ Fixed |
| A02 | Cryptographic Failures | HTTPS + HSTS enforced | ✅ Fixed |
| A03 | Injection | Prepared statements (mysql2) | ✅ Fixed |
| A04 | Insecure Design | Threat modelling applied | ✅ Fixed |
| A05 | Security Misconfiguration | Helmet headers + ZAP audit | ✅ Fixed |
| A06 | Vulnerable & Outdated Components | npm audit + Snyk + Trivy | ✅ Fixed |
| A07 | Auth & Session Failures | API key auth + CSRF tokens | ✅ Fixed |
| A08 | Software Integrity Failures | Dependency scanning enabled | ✅ Fixed |
| A09 | Logging & Monitoring Failures | Fail2Ban + server logs active | ✅ Fixed |
| A10 | SSRF | Input validation + URL whitelisting | ✅ Fixed |

---

## 🎯 Bonus Challenges

<details>
<summary><b>🔒 Zero Trust Security</b></summary>

Implemented JWT-based Zero Trust authentication — every request is verified independently with no implicit session trust.

```js
const jwt = require('jsonwebtoken');

function verifyToken(req, res, next) {
  const token = req.headers['authorization']?.split(' ')[1];
  if (!token) return res.status(401).json({ error: 'No token provided' });
  try {
    req.user = jwt.verify(token, process.env.JWT_SECRET);
    next();
  } catch {
    res.status(403).json({ error: 'Invalid token' });
  }
}
```
</details>

<details>
<summary><b>🛡️ Web Application Firewall (WAF)</b></summary>

Deployed ModSecurity with NGINX using the OWASP Core Rule Set (CRS) to filter malicious traffic before it reaches the application.

```nginx
# nginx.conf snippet
modsecurity on;
modsecurity_rules_file /etc/modsecurity/crs/crs-setup.conf;
```
</details>

<details>
<summary><b>🎣 Social Engineering Simulation</b></summary>

Simulated a phishing awareness campaign using GoPhish targeting internal test users. Results documented in `reports/social_engineering_report.md`.
</details>

---

## ⚙️ Setup & Installation

```bash
# 1. Clone the repository
git clone https://github.com/yourusername/cybersecurity-internship.git
cd cybersecurity-internship

# 2. Install dependencies
npm install

# 3. Copy environment variables
cp .env.example .env
# Edit .env with your values

# 4. Install security tools (Kali Linux / Ubuntu)
sudo apt update
sudo apt install fail2ban nikto lynis zaproxy sqlmap nmap gobuster trivy -y
```

---

## 🔑 Environment Variables

Create a `.env` file based on `.env.example`:

```env
# Server
PORT=3000
NODE_ENV=production

# Authentication
API_KEY=your_super_secret_api_key_here
JWT_SECRET=your_jwt_secret_here

# Database
DB_HOST=localhost
DB_USER=dbuser
DB_PASSWORD=dbpassword
DB_NAME=appdb
```

> ⚠️ **Never commit your `.env` file.** It is already listed in `.gitignore`.

---

## 🚀 Running the Application

```bash
# Development mode
npm run dev

# Production mode
npm start

# Run security tests
npm run test:security

# Run npm audit
npm audit
```

---

## 🐳 Docker Deployment

```bash
# Build the secured image
docker build -t secure-app:latest .

# Scan image before running
trivy image secure-app:latest

# Run with Docker Compose (security flags enabled)
docker compose up -d

# Check running container
docker ps
docker logs secure-app
```

**docker-compose.yml highlights:**
```yaml
services:
  app:
    build: .
    read_only: true
    security_opt:
      - no-new-privileges:true
    cap_drop:
      - ALL
    cap_add:
      - NET_BIND_SERVICE
```

---

## 📊 Security Audit Reports

All generated reports are available in the `/reports` directory:

| Report | Tool | Description |
|--------|------|-------------|
| `zap_report.html` | OWASP ZAP | Web application vulnerability scan |
| `nikto_report.html` | Nikto | Web server misconfiguration scan |
| `lynis-report.dat` | Lynis | Linux server hardening audit |
| `trivy_report.json` | Trivy | Docker image CVE scan |
| `week5_ethical_hacking.md` | SQLMap + Burp Suite | Ethical hacking findings |
| `week6_pentest_report.md` | Burp Suite + Metasploit | Final penetration test results |

---

## 📸 Screenshots

| Task | Screenshot |
|------|-----------|
| Fail2Ban banning IP | `docs/screenshots/fail2ban_ban.png` |
| Rate limiter 429 response | `docs/screenshots/rate_limit_429.png` |
| OWASP ZAP scan results | `docs/screenshots/zap_report.png` |
| SQLMap dumping DB | `docs/screenshots/sqlmap_dump.png` |
| CSRF 403 blocked | `docs/screenshots/csrf_403.png` |
| Trivy image scan | `docs/screenshots/trivy_scan.png` |
| Burp Suite active scan | `docs/screenshots/burp_scan.png` |

---

## 🗓️ Commit History Summary

```
feat: add Fail2Ban intrusion detection config
feat: add rate limiting with express-rate-limit
feat: configure CORS whitelist for trusted origins
feat: add API key authentication middleware
feat: implement Helmet security headers, CSP and HSTS
feat: add SQL injection demo against DVWA
fix:  prevent SQL injection using prepared statements
feat: implement CSRF protection with csurf middleware
feat: add OWASP ZAP and Nikto security audit reports
feat: enable automatic updates and dependency scanning
feat: secure Dockerfile with non-root user and healthcheck
feat: add Trivy container image scanning
feat: complete final penetration test with Burp Suite and Metasploit
docs: add weekly reports and updated README
```

---

## 📝 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Made with 🔐 during Cybersecurity Internship — Weeks 4–6**

![Visits](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

</div>

---

<div align="center">

## 👨‍💻 About the Author

<img src="https://readme-typing-svg.herokuapp.com?font=Fira+Code&size=22&pause=1000&color=2E86C1&center=true&vCenter=true&width=600&lines=Suny+Kumar+Sochi;Cybersecurity+Expert;Ethical+Hacker;IT+Engineer;Web+Developer;SEO+Expert" alt="Typing SVG" />

### **Suny Kumar Sochi**

![Cybersecurity Expert](https://img.shields.io/badge/🔐-Cybersecurity%20Expert-0D1B2A?style=for-the-badge)
![Ethical Hacker](https://img.shields.io/badge/💀-Ethical%20Hacker-7D3C98?style=for-the-badge)
![IT Engineer](https://img.shields.io/badge/⚙️-IT%20Engineer-2E86C1?style=for-the-badge)
![Web Developer](https://img.shields.io/badge/🌐-Web%20Developer-117A65?style=for-the-badge)
![SEO Expert](https://img.shields.io/badge/📈-SEO%20Expert-E67E22?style=for-the-badge)

---

> *"Security is not a product, but a process — and I engineer that process."*
> — **Suny Kumar Sochi**

---

### 🧠 Skills & Expertise

| Domain | Skills |
|--------|--------|
| 🔐 **Cybersecurity** | Penetration Testing, Threat Detection, Vulnerability Assessment, Incident Response |
| 💀 **Ethical Hacking** | Kali Linux, Burp Suite, Metasploit, SQLMap, Nmap, OSINT |
| ⚙️ **IT Engineering** | Linux Administration, Docker, Networking, Server Hardening, Cloud Security |
| 🌐 **Web Development** | Node.js, Express.js, REST APIs, JavaScript, HTML/CSS |
| 📈 **SEO** | Technical SEO, On-Page Optimization, Site Audits, Performance Tuning |

---

### 📬 Connect with Me

[![GitHub](https://img.shields.io/badge/GitHub-Suny%20Kumar%20Sochi-181717?style=for-the-badge&logo=github)](https://github.com/yourusername)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin)](https://linkedin.com/in/yourusername)
[![Email](https://img.shields.io/badge/Email-Contact%20Me-EA4335?style=for-the-badge&logo=gmail)](mailto:youremail@gmail.com)

---

<sub>🔐 Built with dedication by <b>Suny Kumar Sochi</b> — Cybersecurity Internship 2026</sub>

</div>
