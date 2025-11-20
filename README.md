# aws-inspector-automation
🚨 AWS Inspector Automation — Continuous Vulnerability Reporting Pipeline
👤 Author: aav05


📌 Overview

This project automates the collection, processing, and reporting of Amazon Inspector findings across an AWS account. The solution removes the need for manual console-based review by integrating Inspector → Lambda → S3 → SES → Email, sending stakeholders a scheduled vulnerability report containing prioritized security risks.

The automation supports continuous discovery, near real-time alerting, and auditable reporting, which are critical for SOC 2, PCI-DSS, ISO 27001, and CIS framework requirements.

🎯 Goal of This Project

✔ Replace manual vulnerability assessments with automated, repeatable reporting
✔ Provide security, developers, and compliance teams with scheduled actionable findings
✔ Enforce least-privilege IAM and centralize reporting through S3 + email
✔ Demonstrate end-to-end security automation in AWS using serverless architecture

🧰 Tech Stack

| Component           | Purpose                           |
| ------------------- | --------------------------------- |
| Amazon Inspector    | Vulnerability discovery           |
| AWS Lambda (Python) | Fetch & format Inspector findings |
| Amazon S3           | Storage of Inspector reports      |
| Amazon / SES        | Distribution via email            |
| Amazon EventBridge  | Scheduling automation             |
| AWS IAM             | Role-based least-privilege access |
| AWS CloudTrail      | Auditing function activity        |


🏗 Architecture

EventBridge (trigger on schedule: daily/weekly)
        ↓
AWS Lambda (Python)
        ↓
Amazon Inspector API → Get Findings
        ↓
Generate CSV Report
        ↓
Upload report → Amazon S3 (versioning enabled)
        ↓
Send notification & link → SES (email)


🧠 Key Features

🔹 Automated export of Inspector findings (CVSS, affected resources, remediation)
🔹 Supports both EC2 scanning and ECR image scanning
🔹 Creates timestamped reports (CSV and/or JSON) for auditing
🔹 Stores all reports in S3 with encryption and versioning enabled
🔹 Sends email alerts to designated team with link to the report
🔹 Designed with least-privilege IAM (no wildcard permissions)
🔹 Fully serverless — no EC2 cost, free-tier friendly

🔒 Security Controls Applied

| Control Area        | Implementation                                                           |
| ------------------- | ------------------------------------------------------------------------ |
| IAM Least Privilege | Lambda role limited to `InspectorReadOnly` + PutObject to S3 bucket only |
| Encryption          | S3 SSE-S3 (or SSE-KMS optionally)                                        |
| Auditing            | CloudTrail logs Lambda calls and S3 access                               |
| Logging             | Lambda writes execution logs to CloudWatch                               |
| Network Exposure    | No public endpoints; internal AWS calls only                             |
| Resiliency          | Automatic retries and failure logging                                    |


🧾 Example Use Cases
| Use Case                            | Benefit                                     |
| ----------------------------------- | ------------------------------------------- |
| Continuous vulnerability management | Improves remediation SLAs                   |
| Audit evidence for ISO27001/ PCI    | Eliminates screenshots/manual export        |
| Cloud security dashboard input      | Provides structured data for PowerBI/Splunk |
| Engineering leadership summary      | Weekly email summaries of open risks        |


▶️ How It Works (Execution Flow)

EventBridge triggers the automation on schedule
Lambda calls the Inspector API with pagination handling
Findings are normalized — critical fixes prioritized first
CSV report is generated and uploaded to S3
SES sends an email to stakeholders with summary of vulnerabilities and a link to the stored report

📂 Repository Contents

| File                         | Description                                    |
| ---------------------------- | ---------------------------------------------- |
| `lambda_function.py`         | Main automation logic                          |
| `iam_policy.json`            | IAM permissions to deploy least-privilege role |
| `sample_report.csv`          | Example output                                 |
| `deployment_instructions.md` | Install & deploy guide                         |
| `architecture.png`           | Optional visual diagram                        |

🔜 Future Enhancements (Roadmap)

 Multi-account support via AWS Organizations

 Slack & Microsoft Teams alert integration

 Optional export to Security Hub / Jira tickets

 Dashboard using Athena + QuickSight

 Auto-remediation workflows (optional & controlled)

📈 Results & Impact

After deployment, the automation:
| Metric                            | Before           | After                          |
| --------------------------------- | ---------------- | ------------------------------ |
| Time to generate Inspector report | 2–3 hrs manually | < 1 min automated              |
| Frequency of reporting            | Monthly          | Daily/Weekly                   |
| Audit readiness                   | Low              | Reports timestamped & archived |
| Visibility for leadership         | Limited          | Automated summaries            |

💡 What I Learned

This project strengthened my knowledge in:

Building secure serverless automations in AWS

Vulnerability management in cloud environments

IAM least-privilege design and permissions scoping

Security architecture for reporting pipelines

Monitoring & auditing for compliance frameworks
