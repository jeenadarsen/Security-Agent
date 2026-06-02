# 🛡️ Autonomous "Human-in-the-Loop" Security Scanner & Remediation Agent - Security Triage Agent

An intelligent, autonomous first-responder agent built using **Microsoft Copilot Studio** and **Power Automate**. This solution intercepts messy, chaotic vulnerability alerts from third-party scanners, analyzes them against internal corporate policies via a generative AI reasoning engine, and delivers a single-click remediation path directly into your engineering workflows via Microsoft Teams and **GitHub Issues**.

---

## 💡 The Problem & Business Value

* **The Avalanche:** Engineering teams face massive alert fatigue, receiving hundreds of unorganized, raw security logs weekly.
* **The Friction:** Manual triage requires developers to hunt down corporate compliance documents, calculate resolution deadlines (SLAs), and research stable package updates.
* **The Solution:** This agent condenses hours of manual compliance tracking and log analysis into a **5-second, single-click process**, ensuring critical vulnerabilities are never missed while keeping a human firmly in control.

---

## 🏗️ Solution Architecture & Workflow

The solution relies on three core operational phases:

1. **Intake & Parse:** The agent accepts a raw scanner log, automatically filtering out noise to isolate key variables (CVE ID, package name, current version, repository).
2. **Policy Evaluation:** Copilot Studio utilizes its Generative AI Chaining Engine to read an internal SharePoint knowledge base, extract the official patching SLA, and locate a target fix version.
3. **Human-in-the-Loop Remediate:** The agent pushes an interactive Adaptive Card to Microsoft Teams. Upon clicking "Approve & Log Ticket," a Power Automate plugin dynamically provisions a fully documented issue directly in the target **GitHub Repository**.

---

## 🛠️ Quick-Start Deployment Guide (5 Minutes)

Because this solution is pre-packaged inside a single Dataverse Solution, you only need to configure your local environmental connections and knowledge source to begin testing.

### Step 1: Update the Knowledge Source (Copilot Studio)

1. Open **Copilot Studio** and navigate to your imported agent.
2. Go to the **Knowledge** tab on the left menu.
3. Click on the existing SharePoint data source to edit it, or click **Add Knowledge** and select **SharePoint**.
4. Paste the URL of your local SharePoint library containing the `Sec-Policy-SLA-v1.docx` compliance document and click **Save**.
   * *Alternative:* If you do not wish to link a live SharePoint site, click **Add Knowledge**, select **File Upload**, and upload the `Sec-Policy-SLA-v1.docx` file directly into the agent.

### Step 2: Fix the Cloud Flow Connections (Power Automate)

1. Open [make.powerautomate.com](https://make.powerautomate.com) and go to your solution folder.
2. Locate and open the cloud flow: `SecurityAgent-RemediationAction`. Click **Edit**.
3. **Fix Microsoft Teams Connection:** Locate the action card labeled *“Post adaptive card and wait for a response”*. If there is a connection error icon, click it and select **+ Add new connection** to authenticate it with your current tenant account.
4. **Fix GitHub Connection:** Scroll down to the *“Create an issue (GitHub)”* action block inside the "True" branch. Click the block, authenticate using your personal or test GitHub account.
5. Click **Save** at the top right of the designer screen.

---

## 🚀 Step-by-Step Demo Guide (How to Run and Test)

Because this solution is configured inside a development workspace, the optimal way to test the agent's autonomous dynamic chaining is through the built-in test canvas without needing a full publication step.

1. Open the agent inside the **Copilot Studio Authoring Portal**.
2. Locate the **Test your copilot** chat panel on the left side of the screen.
3. Ensure that **Track between topics / Dynamic chaining** is toggled **ON**.
4. Copy and paste the following unformatted, chaotic mock scanner payload directly into the chat pane: **change the payment-gateway-api in the payload to a repository name you have access to**

```text
   [SCANNER_LOG] SEC_ALERT: critical dependency issue discovered in package registry. Path: root/src/components/payment-gateway-api/node_modules/lodash-es. Deployed Version: 4.17.21. Security identifier: CVE-2026-12345 (Remote Code Execution exploit risk). Recommended patch mitigation path: upgrade target package component to secure stable release lodash-es@4.17.22. Operational task tracking ID: 98721-delta.
