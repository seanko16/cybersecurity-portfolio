# GCP Cybersecurity Module — Automation, IaC, and Security Best Practices

## Overview
In this module, I explored how **automation, Infrastructure as Code (IaC), and security best practices** interact in Google Cloud Platform (GCP). Key concepts include ephemeral resources, immutable infrastructure, DevSecOps practices, declarative vs imperative IaC, Terraform usage, and AI-assisted cloud security. The module emphasizes reducing human error, improving consistency, and embedding security from the start.

---

## Core Concepts

### TTL Policies
- **Definition:** Set an expiration time on a data asset in Firestore.  
- **Effect:** Asset becomes inaccessible after TTL expires, but can still be manually deleted.  
- **Security Benefit:** Limits data exposure over time.

---

### Ephemerality
- **Definition:** Resources are temporary and easily replaceable.  
- **Importance:**
  1. Reduces the risk of data leaks or persistent attacks.  
  2. Simplifies replacement and scaling of system resources.

### Immutability
- **Definition:** Once an object (container, VM image, etc.) is created, it cannot be modified. Updates require creating a new object.  
- **Importance:**
  1. Prevents configuration drift and ensures consistency across deployments.  
  2. Mitigates risk of attacker modifications (backdoors, misconfigurations).  
  3. Supports reliable CI/CD pipelines by ensuring all releases run in the same environment.

---

### Automation
- **Definition:** Technology-driven reduction of human involvement in repetitive tasks.  
- **GCP Perspective:**  
  > Automation helps reduce human error by completing repetitive tasks the same way every time, without altering processes.

- **Use Cases in Cloud Security:**
  - Vulnerability scanning  
  - CI/CD / DevSecOps pipelines  
  - Misconfiguration detection  
  - Logging and monitoring  
  - Threat detection and reporting

---

### Infrastructure as Code (IaC)
- **Definition:** Use of code/scripts to automate creation, configuration, and management of infrastructure.  
- **Benefits:**
  - Consistent and repeatable deployments  
  - Embedded security configurations (`secure by default`)  
  - Version-controlled infrastructure changes  

- **Tools:**  
  - Terraform: Builds, changes, versions cloud resources. Works across major cloud providers.  
  - Kubernetes: Automates container deployment, management, and scaling.

- **Security Applications:**  
  - Policy as code (e.g., Sentinel, OPA)  
  - Continuous audit and compliance  
  - Prevents manual drift and configuration errors

---

### Declarative vs Imperative IaC
- **Declarative:** Define the desired end state; the system determines how to reach it.  
- **Imperative:** Specify step-by-step commands to reach the desired state.  
- **Security Impact:** Declarative IaC reduces drift, enforces consistency, and simplifies secure deployments.

---

### DevSecOps
- **Definition:** Integrates security into DevOps practices.  
- **Benefits:**
  - Ensures security from the start of development  
  - Reduces future risk and costs  
  - Automates security tasks within CI/CD pipelines

---

### AI in Cloud Security
- **Capabilities:**  
  - Log monitoring and anomaly detection  
  - Malware detection  
  - Automated vulnerability scanning  
  - Alert prioritization
- **Caution:** AI can produce false positives, inaccuracies, or bias; human analysts make the final decisions.

---

## Terraform & Security in GCP

### Core Concepts
- **Resources Managed:** Compute, networking, storage, SaaS, DNS, etc.  
- **Advantage:** Prevents drift (manual changes that diverge from IaC code).  
- **Pre-deployment Questions:**
  - What infrastructure is required?  
  - Which security settings are necessary?  
  - How will monitoring/auditing ensure consistency?

### Continuous Audit
- **Items to Monitor:**  
  - RBAC (role-based access control)  
  - Root or elevated permissions  
  - Secrets (passwords, API keys)  
  - Service configurations  
  - Network connections (ingress/egress)  
  - Misconfigurations enabling attacks (e.g., MITM)

### Best Practices
- Document all processes and security decisions  
- Test scripts before deployment  
- Use version control (e.g., Git)  
- Apply all changes through Terraform to prevent drift  
- Prefer official provider resources (pre-vetted and secure)

### Terraform Concepts
- **Workspaces:** Security boundaries; separate variables, state, logs, SSH keys  
- **Projects:** Group related Workspaces  
- **Permissions:** Fine-grained access (read, write, admin)  
- **Custom Permissions:** Tailored for organizational requirements

---

## Key Takeaways
- **Ephemeral and immutable resources** reduce risk and simplify cloud operations.  
- **Automation** prevents human error and accelerates cloud security processes.  
- **IaC and Terraform** enforce consistent, secure deployments while enabling continuous audit.  
- **Declarative IaC** minimizes drift and eases secure provisioning.  
- **DevSecOps** integrates security early in the development lifecycle, improving efficiency and safety.  
- **AI-assisted security** aids monitoring and detection but requires analyst oversight.  

---

## Workflow Diagram (Terraform + Security)

```mermaid
flowchart TD
    A[IaC - Terraform] --> B[Policy as Code - Sentinel / OPA]
    B --> C[Secure Deploy - יצירת משאבים מאובטחים]
    C --> D[Continuous Audit - ניטור הרשאות, רשתות, סודות ושינויים ידניים]

    style A fill:#D6EAF8,stroke:#1B4F72,stroke-width:2px
    style B fill:#D5F5E3,stroke:#145A32,stroke-width:2px
    style C fill:#FCF3CF,stroke:#7D6608,stroke-width:2px
    style D fill:#FADBD8,stroke:#922B21,stroke-width:2px
