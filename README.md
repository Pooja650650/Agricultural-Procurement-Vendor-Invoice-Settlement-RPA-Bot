# 🌾 Agricultural Procurement & Vendor Invoice Settlement RPA Bot

[![Automation Anywhere A360](https://img.shields.io/badge/Platform-Automation%20Anywhere%20A360-orange?style=flat-square&logo=automationanywhere)](https://www.automationanywhere.com/)
[![Process Status](https://img.shields.io/badge/Bot%20Status-Production%20Ready-brightgreen?style=flat-square)](https://github.com/)
[![RPA Architecture](https://img.shields.io/badge/Architecture-Modular%20Fault--Tolerant-purple?style=flat-square)](docs/architecture/)

An enterprise-grade Robotic Process Automation (RPA) solution developed in **Automation Anywhere (A360)** to automate agricultural vendor invoice reconciliation, cross-verify weighbridge logs and government Minimum Support Price (MSP) rate limits, execute web portal transactions, and maintain structured audit ledgers.

## 📂 Project Assets & Live Demo Video

Since Automation Anywhere A360 enterprise bots operate in secure Control Rooms, the complete project portfolio has been exported for evaluation. 

All execution assets—including the **Full Demonstration Video**, **Input/Output Excel Databases**, and **High-Resolution Screenshots**—are securely hosted in the project drive.

👉 **[Access the Complete Project Portfolio Here](https://drive.google.com/drive/folders/18pFCH7mSzdvegXIi8ZqypnklWDcn9KdZ?usp=sharing)**<br>

[![Watch Demo](https://img.shields.io/badge/Watch-Demo%20Video-red?style=flat-square&logo=youtube)](https://drive.google.com/file/d/19okYoEklWdSr0WyunRplxRR0X_9-oivu/view?usp=sharing)

## 📸 Execution Screenshots

| 1. Control Room Workflow | 2. Validation Engine Logic |
| :---: | :---: |
| ![Control Room](https://lh3.googleusercontent.com/d/1j0sdnsGCHwVYLlsvf2d7Q5LtrDh2T1Wh) | ![Validation Engine](https://lh3.googleusercontent.com/d/1TdQF56umTk8AfwUN2S5DvRxwqUshJfoH) |
| *A360 Task Bot orchestration flow* | *Nested conditional rules for weight & MSP check* |

| 3. Web Portal Automation | 4. Discrepancy Audit Log |
| :---: | :---: |
| ![Web Automation](https://lh3.googleusercontent.com/d/1BaG8YcFiIQTQcoTLKgwcoG2-fNG9S82t) | ![Discrepancy Report](https://lh3.googleusercontent.com/d/1CO3Wf3DHL3fFkR7I3JQjHertSvWs9thq) |
| *Recorder package dynamic form entry* | *Structured output for rejected invoices* |


## 🚀 Overview

Agricultural supply chains handle high volumes of daily procurement receipts across hundreds of vendors. Manual settlement verification requires cross-checking invoiced weights against physical weighbridge gross receipts and verifying claimed rates against government MSP ceilings. 

This bot automates the end-to-end reconciliation pipeline: ingesting queue files, executing business rules in memory, handling web settlement, logging exceptions, and updating master audit ledgers without human intervention.

---

## 🎯 Business Problem

Manual settlement processes suffer from significant operational bottlenecks:
* **High Error Rate:** Human oversights lead to accepting over-billed weights or non-compliant rates.
* **Processing Delays:** Manual cross-referencing between CSV queue files, Excel logs, and web portals takes 5–8 minutes per invoice.
* **Audit Risks:** Inconsistent discrepancy logging creates compliance vulnerabilities during financial audits.

---

## 💡 Solution

The **Agri-Procurement Settlement Bot** automates data matching, rule enforcement, web transaction processing, and exception handling:
* **Speed:** Processes each invoice record in under 5 seconds.
* **100% Accuracy:** Strict algorithmic enforcement of 1% weight tolerance and MSP limits.
* **Fault Tolerance:** Robust `Try-Catch` architecture prevents process termination on individual record failures.

---

## 🔄 Business Process Workflow

```text
[Input Queue: VendorInvoices.csv]
              │
              ▼
[Read Reference Data: WeighbridgeLogs.xlsx & RateMaster.xlsx]
              │
              ▼
[Loop: For Each Invoice Row] ───► Extract Variables
              │
              ▼
[Reconciliation Engine]
 ├── Match VendorID in Weighbridge Logs ──► Get Actual Weight
 └── Match CropType in Rate Master       ──► Get Max Allowed MSP Rate
              │
              ▼
[Validation Checks]
 ├── 1. Vendor ID Exists?
 ├── 2. |Invoiced Weight - Actual Weight| <= 1% Actual Weight?
 └── 3. Claimed Rate <= Max Allowed MSP Rate?
              │
      ┌───────┴───────┐
      │  Is Valid?    │
      └───────┬───────┘
   YES │       │ NO
       │       └─────────────────────────────────────────┐
       ▼                                                 ▼
[Open Web ERP Portal]                           [Open Discrepancy_Report.xlsx]
       │                                                 │
[Fill Form & Submit via Recorder]               [Write Vendor, Reason & Timestamp]
       │                                                 │
[Extract Generated Voucher ID]                  [Log Rejection to ExecutionLog.txt]
       │                                                 │
[Update Processing_Master.xlsx (SUCCESS)]                │
       │                                                 │
[Log Success to ExecutionLog.txt]                        │
       │                                                 │
       └───────────────────────┬─────────────────────────┘
                               │
                               ▼
                   [Increment Loop Counter]
                               │
                               ▼
               [All Rows Done? ──► Step 07 Cleanup & Notify]
