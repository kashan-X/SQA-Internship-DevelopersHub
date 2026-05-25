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
# ============================================================
# Selenium Test Suite - automationexercise.com
# DevelopersHub Corporation - SQA Internship Week 6
# ============================================================

import pytest
import time
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.chrome.options import Options
from webdriver_manager.chrome import ChromeDriverManager

BASE_URL = "https://automationexercise.com"

# ── Setup: runs before every test ───────────────────────────
@pytest.fixture
def driver():
    options = Options()
    options.add_argument("--headless")           # Run without opening browser window
    options.add_argument("--no-sandbox")
    options.add_argument("--disable-dev-shm-usage")
    options.add_argument("--disable-gpu")
    options.add_argument("--window-size=1920,1080")

    driver = webdriver.Chrome(
        service=Service(ChromeDriverManager().install()),
        options=options
    )
    driver.implicitly_wait(10)
    yield driver
    driver.quit()  # Close browser after each test


# ============================================================
# TEST 1: Home Page Loads Successfully
# ============================================================
def test_home_page_loads(driver):
    """TC-001: Verify home page loads and title is correct"""
    driver.get(BASE_URL)
    time.sleep(2)

    assert "Automation Exercise" in driver.title, \
        f"Expected 'Automation Exercise' in title but got: {driver.title}"

    print(f"✅ TC-001 PASSED: Home page loaded | Title: {driver.title}")


# ============================================================
# TEST 2: Products Page is Accessible
# ============================================================
def test_products_page(driver):
    """TC-002: Verify products page loads and shows products"""
    driver.get(f"{BASE_URL}/products")
    time.sleep(2)

    assert "Products" in driver.title or driver.find_elements(
        By.CLASS_NAME, "product-image-wrapper"), \
        "Products page did not load correctly"

    products = driver.find_elements(By.CLASS_NAME, "product-image-wrapper")
    assert len(products) > 0, "No products found on page"

    print(f"✅ TC-002 PASSED: Products page loaded | Products found: {len(products)}")


# ============================================================
# TEST 3: Login Page Elements Exist
# ============================================================
def test_login_page_elements(driver):
    """TC-003: Verify login page has email, password fields and login button"""
    driver.get(f"{BASE_URL}/login")
    time.sleep(2)

    email_field    = driver.find_element(By.CSS_SELECTOR, "input[data-qa='login-email']")
    password_field = driver.find_element(By.CSS_SELECTOR, "input[data-qa='login-password']")
    login_button   = driver.find_element(By.CSS_SELECTOR, "button[data-qa='login-button']")

    assert email_field.is_displayed(),    "Email field not visible"
    assert password_field.is_displayed(), "Password field not visible"
    assert login_button.is_displayed(),   "Login button not visible"

    print("✅ TC-003 PASSED: Login page has all required elements")


# ============================================================
# TEST 4: Login with Invalid Credentials Shows Error
# ============================================================
def test_login_invalid_credentials(driver):
    """TC-004: Verify error message shown on wrong credentials"""
    driver.get(f"{BASE_URL}/login")
    time.sleep(2)

    driver.find_element(By.CSS_SELECTOR, "input[data-qa='login-email']")\
          .send_keys("wrongemail@test.com")
    driver.find_element(By.CSS_SELECTOR, "input[data-qa='login-password']")\
          .send_keys("wrongpassword123")
    driver.find_element(By.CSS_SELECTOR, "button[data-qa='login-button']").click()
    time.sleep(2)

    error = driver.find_element(By.XPATH, "//*[contains(text(),'incorrect')]")
    assert error.is_displayed(), "Error message not shown for invalid credentials"

    print(f"✅ TC-004 PASSED: Error shown for invalid login | Message: {error.text}")


# ============================================================
# TEST 5: Product Search Works
# ============================================================
def test_product_search(driver):
    """TC-005: Verify product search returns results"""
    driver.get(f"{BASE_URL}/products")
    time.sleep(2)

    search_box = driver.find_element(By.ID, "search_product")
    search_box.send_keys("top")
    driver.find_element(By.ID, "submit_search").click()
    time.sleep(2)

    results = driver.find_elements(By.CLASS_NAME, "product-image-wrapper")
    assert len(results) > 0, "Search returned no results"

    print(f"✅ TC-005 PASSED: Product search works | Results: {len(results)}")


# ============================================================
# TEST 6: Signup Page Loads and Has Required Fields
# ============================================================
def test_signup_page_elements(driver):
    """TC-006: Verify signup page has name and email fields"""
    driver.get(f"{BASE_URL}/login")
    time.sleep(2)

    name_field  = driver.find_element(By.CSS_SELECTOR, "input[data-qa='signup-name']")
    email_field = driver.find_element(By.CSS_SELECTOR, "input[data-qa='signup-email']")
    signup_btn  = driver.find_element(By.CSS_SELECTOR, "button[data-qa='signup-button']")

    assert name_field.is_displayed(),  "Name field not visible"
    assert email_field.is_displayed(), "Email field not visible"
    assert signup_btn.is_displayed(),  "Signup button not visible"

    print("✅ TC-006 PASSED: Signup form has all required fields")


# ============================================================
# TEST 7: Contact Us Page Form Exists
# ============================================================
def test_contact_us_page(driver):
    """TC-007: Verify Contact Us page loads with a form"""
    driver.get(f"{BASE_URL}/contact_us")
    time.sleep(2)

    assert "Contact Us" in driver.page_source, \
        "Contact Us heading not found"

    name_field = driver.find_element(By.CSS_SELECTOR, "input[data-qa='name']")
    assert name_field.is_displayed(), "Name field not visible on Contact Us page"

    print("✅ TC-007 PASSED: Contact Us page loaded with form")


# ============================================================
# TEST 8: Navigation Menu Links Work
# ============================================================
def test_navigation_links(driver):
    """TC-008: Verify all main navigation links are present"""
    driver.get(BASE_URL)
    time.sleep(2)

    nav_links = driver.find_elements(By.CSS_SELECTOR, ".navbar-nav li a")
    link_texts = [link.text.strip() for link in nav_links if link.text.strip()]

    expected = ["Home", "Products", "Cart"]
    for item in expected:
        assert any(item in text for text in link_texts), \
            f"Navigation link '{item}' not found. Found: {link_texts}"

    print(f"✅ TC-008 PASSED: Navigation links present | Links: {link_texts}")


# ============================================================
# TEST 9: Cart Page is Accessible
# ============================================================
def test_cart_page(driver):
    """TC-009: Verify Cart page loads"""
    driver.get(f"{BASE_URL}/view_cart")
    time.sleep(2)

    assert "Shopping Cart" in driver.page_source or \
           "Cart" in driver.title or \
           driver.find_elements(By.ID, "cart_info"), \
           "Cart page did not load"

    print("✅ TC-009 PASSED: Cart page loaded successfully")


# ============================================================
# TEST 10: Footer is Present on Home Page
# ============================================================
def test_footer_present(driver):
    """TC-010: Verify footer exists on home page"""
    driver.get(BASE_URL)
    time.sleep(2)

    footer = driver.find_elements(By.ID, "footer")
    assert len(footer) > 0, "Footer not found on home page"
    assert footer[0].is_displayed(), "Footer not visible"

    print("✅ TC-010 PASSED: Footer is present and visible")
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
