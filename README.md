# SQA Internship – DevelopersHub Corporation
## Software Quality Assurance – Weeks 4 to 6 Project

### 👤 Intern Information
- **Name:** Kashan Karim Intern (DHC-2527)
- **Organization:** DevelopersHub Corporation
- **Application Tested:** [automationexercise.com](https://automationexercise.com)
- **Submission Date:** May 25, 2026

---

## 📋 Project Overview

This repository contains all deliverables for the SQA Internship project covering:
- **Week 4:** Performance & Load Testing using Apache JMeter
- **Week 5:** Security Testing & Vulnerability Analysis using OWASP ZAP
- **Week 6:** CI/CD Integration + Final QA Portfolio

---

## 📁 Repository Contents

| File | Description | Week |
|------|-------------|------|
| `Week4_AutomationExercise_LoadTest.jmx` | JMeter test plan file | Week 4 |
| `Week4_Performance_Test_Plan_FINAL.pdf` | Performance test plan document | Week 4 |
| `Week4_Performance_Testing_Report_FINAL.pdf` | Performance results with JMeter screenshots | Week 4 |
| `Week5_Vulnerability_Report.pdf` | 20 vulnerabilities with full details | Week 5 |
| `Week5_Security_Testing_Summary_FINAL.pdf` | Security summary with ZAP + manual test evidence | Week 5 |
| `Week6_CICD_Workflow.pdf` | CI/CD pipeline design document | Week 6 |
| `Week6_Final_QA_Portfolio.pdf` | Complete QA portfolio combining all weeks | Week 6 |

---

## 🔧 Tools Used

| Tool | Version | Purpose |
|------|---------|---------|
| Apache JMeter | 5.6.3 | Performance & Load Testing |
| OWASP ZAP | 2.17.0 | Security Testing |
| GitHub Actions | - | CI/CD Pipeline (Theory) |
| Google Chrome | Latest | Manual Testing Browser |

---

## 📊 Week 4 – Performance Testing Summary

**Test Scenarios:**

| Scenario | Users | Ramp-Up | Avg Response | Error % | Throughput |
|----------|-------|---------|-------------|---------|------------|
| Load Test | 100 | 60s | 735 ms | 36.60%* | 21.9/sec |
| Stress Test | 200 | 60s | 409 ms | 20.00%* | 31.1/sec |
| Spike Test | 150 | 5s | 1,506 ms | 38.93%* | 59.3/sec |

> *Error % driven by POST /login CSRF protection blocking automated requests

---

## 🔒 Week 5 – Security Testing Summary

**Vulnerabilities Found: 20 total**

| Severity | Count |
|----------|-------|
| 🔴 High | 1 |
| 🟠 Medium | 5 |
| 🟡 Low | 8 |
| 🔵 Informational | 6 |

**Top Findings:**
- Vulnerable JS Libraries (Bootstrap, jQuery) – **HIGH**
- No Brute Force Protection on Login – **MEDIUM**
- No Session Timeout – **MEDIUM**
- Missing CSP, HSTS, HttpOnly/Secure Cookie Flags

---

## 🚀 Week 6 – CI/CD Pipeline Design

The following GitHub Actions pipeline was designed to automate QA testing:

```yaml
name: QA Automation Pipeline
on:
  push:
    branches: [ main ]
jobs:
  functional-tests:   # Selenium tests
  performance-tests:  # JMeter load test
  security-scan:      # OWASP ZAP scan
```

**Pipeline Stages:**
1. Code Push triggers pipeline
2. Selenium functional tests run
3. JMeter performance test runs
4. OWASP ZAP security scan runs
5. All reports uploaded as artifacts
6. Team notified of results

---

## 📌 Key Recommendations

1. 🔴 **Upgrade Bootstrap and jQuery** to latest versions
2. 🔴 **Add CAPTCHA + rate limiting** on /login
3. 🟠 **Implement session timeout** (15-30 minutes)
4. 🟠 **Add Content-Security-Policy** header
5. 🟠 **Set HttpOnly + Secure** flags on all cookies

---

*This project was completed as part of the DevelopersHub Corporation SQA Internship Program.*
