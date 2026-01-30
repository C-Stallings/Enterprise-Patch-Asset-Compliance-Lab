# Enterprise Patch & Asset Compliance Lab (AWS)

## Overview

This lab demonstrates enterprise patch management, asset inventory tracking, and compliance validation using AWS Systems Manager. Systems are centrally managed using agent-based secure access without SSH or RDP dependency.

The project produces audit-ready compliance artifacts and RMF-style documentation.

---

## Architecture

- EC2 Windows Server 2019 — t3.micro
- EC2 Amazon Linux 2 — t3.micro
- AWS Systems Manager (SSM)
- Patch Manager
- Fleet Manager
- SSM Inventory
- IAM Role: **AmazonSSMManagedInstanceCore**

---

## Objectives

- Centrally manage Windows and Linux systems
- Validate SSM secure access
- Perform patch compliance scans
- Remediate one system
- Leave one system non-compliant for validation
- Capture asset inventory
- Simulate configuration drift
- Perform remediation

---

## Deployment

Two EC2 instances launched with SSM IAM role attached.

### 📸 EC2 Instances Running

<p align="center">
  <img 
    src="https://github.com/user-attachments/assets/63018795-8c68-45d1-a55a-2b8edca37e24" 
    height="367" 
    width="643" 
    alt="SS-EC2-Instances-Launched"
  />
</p>

---

## Systems Manager Validation

Instances confirmed as Managed + Online in Fleet Manager.

### 📸 SSM Managed Nodes

<p align="center">
  <img 
    src="https://github.com/user-attachments/assets/bb5acd3f-b575-4eda-b0cc-f99c57a61671" 
    height="367" 
    width="643" 
    alt=" SS_SSM-FleetManagerOverview_1"
  />
</p>

---

## Secure Session Manager Access

Connected using Session Manager instead of SSH/RDP.

## Commands Executed

```powershell
systeminfo
Get-Process amazon-ssm-agent
```
- Expected output:
  - **amazon-ssm-agent running**

 <p align="center">
  <img 
    src="https://github.com/user-attachments/assets/ed8ba732-4f3f-4dcc-a987-7d2030321264" 
    height="367" 
    width="643" 
    alt="SS_SSM_Check_Command_1"
  />
</p>

---

```powershell
systeminfo | Select-String "System Boot Time"
```
- Expected output:
  - **Shows system uptime which is useful ops data.**
 
 <p align="center">
  <img 
    src="https://github.com/user-attachments/assets/c0adaad8-ab4f-4207-b032-0b3d6d4bb481" 
    height="367" 
    width="643" 
    alt="SS_SSM_Check_Command_3"
  />
</p>

---

## Configuration Drift Test

SSM agent disruption simulated drift.

- Windows patched
- Linux has a Non-compliant status of: **Connection Lost**.

### Drift Detected

 <p align="center">
  <img 
    src="https://github.com/user-attachments/assets/7fe5bd07-c0b8-4d3b-bd21-00a1f7169677" 
    height="367" 
    width="643" 
    alt="SS_Non-compliant-managed-node-state_1"
  />
</p>

### Remediation Restored

 <p align="center">
  <img 
    src="https://github.com/user-attachments/assets/ca31ce88-42b4-48b4-9481-f2d35c3c5c6c" 
    height="367" 
    width="643" 
    alt="SS_Manageed-note-restored-after-remediation_1"
  />
</p>

---

## RMF Documentation
**System Description**

- The system consists of AWS EC2 Windows and Linux instances managed through AWS Systems Manager. Centralized agent-based management supports secure remote access, patch compliance monitoring, and automated asset inventory collection.

---
**Patch Management SOP (SI-2)**

**Purpose:** Timely remediation of vulnerabilities.

**Tools:** AWS Systems Manager Patch Manager

**Process:**

- Monthly patch scan
- Baseline comparison
- Maintenance window patching
- Health validation
- Compliance report capture

**Rollback:** Restore from EC2 snapshot.
- Controls: SI-2, CM-3

---
## Configuration Baseline (CM-2)

- Approved AWS patch baseline
- IAM-managed access
- SSM agent required
- No direct admin ports required

---

**Drift Validation Record**

- Configuration drift introduced by disabling management agent. Systems Manager detected loss of managed status. Remediation performed via controlled reboot restoring compliant state.

**Compliance Narrative**

- Initial scans showed full compliance under the default patch baseline. A stricter baseline was temporarily applied to simulate non-compliance. Findings were detected and remediated. Environment returned to approved baseline after validation.

**Asset Inventory Statement**

- Asset data including OS version, patch state, and instance identifiers is automatically collected through AWS Systems Manager Inventory.
