# 📊 Customer Lifetime Value (CLV) & Survival Analysis for NBFC Lending

---

## 🚀 Problem Statement

Traditional credit models answer:

> “Will a customer default?”

But in lending, a more important question is:

> **“When will a customer default, and how much value will they generate before that?”**

This project uses **survival analysis + cashflow modeling** to answer both.

---

## 🎯 Objectives

* Model **time-to-default (survival analysis)**
* Identify **how risk evolves over time**
* Estimate **Customer Lifetime Value (CLV)**
* Translate model outputs into **business strategy**

---

## 📊 Dataset Overview

Simulated NBFC-style dataset with realistic noise:

### Customer Features

* `income` → declared income (noisy / unreliable)
* `bureau_score` → creditworthiness proxy
* `loan_amount` → exposure size
* `utilization` → credit usage (behavioral stress)

### Behavioral Features

* `payment_ratio` → % of dues paid
* `dpd_flag` → delinquency indicator

### Time-to-Event Variables

* `tenure` → observed duration (months)
* `event` → 1 = default, 0 = censored (no default observed)

---

## 🧠 Key Concept: Censoring

Not all customers default within observation period.

* `event = 1` → default observed
* `event = 0` → still active (censored)

👉 This is what makes survival analysis necessary.

---

# 📉 Step 1: Kaplan-Meier Survival Analysis

### 📌 What it does

Estimates:

> **Probability that a customer survives (does not default) up to time t**

---

### 📊 Output

A step-wise curve:

* Y-axis → survival probability
* X-axis → time

---

### 🧠 Interpretation Rules

* Steep drop early → high early defaults
* Flat curve → stable customers
* Lower curve → riskier portfolio

---

### 💼 Business Meaning

* Early drop → underwriting issue
* Late drop → behavioral / collection issue

---

# ⚙️ Step 2: Cox Proportional Hazards Model

### 📌 What it does

Models:

> **How each feature affects default risk over time**

---

## 📉 Model Equation

[
h(t|X) = h_0(t) \cdot e^{\beta X}
]

---

## 🔥 Key Output: Hazard Ratio (`exp(coef)`)

| Value | Meaning        |
| ----- | -------------- |
| > 1   | increases risk |
| < 1   | reduces risk   |
| = 1   | no effect      |

---

### 💼 Example

* `utilization = 1.8`
  → increases risk by 80%

* `bureau_score = 0.7`
  → reduces risk by 30%

---

## ⚠️ Important Assumption

### 👉 Proportional Hazards (PH)

> Feature impact should remain constant over time

---

# 📊 Step 3: Schoenfeld Residual Analysis

### 📌 What it does

Checks:

> “Does a feature’s effect change over time?”

---

## 📈 How to Read the Plot

Each plot shows:

* X-axis → time
* Y-axis → residual (effect variation)
* Black line → trend

---

## 🔥 Interpretation Rules (VERY IMPORTANT)

### 🔻 Downward Trend

👉 Effect decreases over time

💼 Meaning:

> Feature is an **early-stage risk driver**

Example:

* `utilization`
* Strong impact early → weak later

---

### 🔺 Upward Trend

👉 Effect increases over time

💼 Meaning:

> Feature is a **long-term risk driver**

Example:

* `bureau_score`
* Weak early → strong later

---

### ➖ Flat Line

👉 Effect is constant

💼 Meaning:

> Feature is **stable across lifecycle**

Example:

* `dpd_flag`

---

## ⚠️ Important Clarification

This plot tells:

❌ NOT “who defaults early or late”
✅ BUT “when the feature matters most”

---

## 📊 Statistical Test (p-value)

### Hypothesis:

* H₀ → effect is constant (good)
* H₁ → effect changes (violation)

---

### Decision Rule:

| p-value | Meaning          |
| ------- | ---------------- |
| > 0.05  | assumption holds |
| < 0.05  | violation        |

---

### Example

* utilization → p < 0.05 → time-dependent
* dpd_flag → p > 0.05 → stable

---

## 📊 Test Statistic

* Higher value → stronger evidence of violation
* Lower value → weak/no violation

---

# 💡 Key Insights from Model

* Utilization → early default driver
* Bureau score → long-term survival driver
* DPD → consistent risk indicator

---

# 💰 Step 4: Customer Lifetime Value (CLV)

---

## 📌 What it does

Estimates:

> **Expected revenue generated before default**

---

## 📉 Formula

[
CLV = \sum_{t=1}^{T} S(t) \cdot CF_t
]

---

## Components

* ( S(t) ) → survival probability
* ( CF_t ) → monthly cashflow

---

## ⚙️ Implementation

### Cashflow

```python
monthly_cashflow = emi * payment_ratio
```

---

### Survival

* Kaplan-Meier → portfolio level
* Cox → individual level (preferred)

---

## 💼 Interpretation

| CLV  | Meaning            |
| ---- | ------------------ |
| High | profitable         |
| Low  | risky / low return |

---

# 📊 Business Strategy

---

## 🔴 Early Stage (0–3 months)

* Focus: utilization
* Action: early intervention

---

## 🟡 Mid Stage

* Mix: behavior + bureau

---

## 🟢 Late Stage

* Focus: bureau + retention

---

## 🎯 CLV-Based Segmentation

| Segment  | Action          |
| -------- | --------------- |
| High CLV | retain & upsell |
| Low CLV  | reduce effort   |

---

# 🛠️ Tech Stack

* Python (pandas, numpy)
* lifelines (survival analysis)
* matplotlib

---

# 🚀 Key Takeaways

* Risk is **time-dependent**, not static
* Different features matter at different lifecycle stages
* CLV provides **profit-focused decisioning**

---

# 🔥 Future Improvements

* Uplift modeling for collections strategy
* A/B testing for interventions
* Real-time scoring pipeline

---

# 📌 Final Insight

> “Instead of predicting who defaults, this project predicts when they default and how much value they generate — enabling better financial decision-making.”

---
