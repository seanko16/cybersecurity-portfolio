## Cloud Security Lab — XSS Vulnerability Exercise

### Overview

This lab documents the deployment, scanning, and remediation of a **vulnerable Python Flask application** on Google Cloud Platform (GCP). The exercise covers reserving a static IP, launching a VM, deploying the app, scanning with Google Web Security Scanner, confirming an XSS finding, remediating the vulnerability, and re-scanning to verify remediation. A known lab verification issue is noted in the support section.

-----

### Steps & Commands

#### 1\. Reserve a static IP

Create a static IP to assign to the VM:

```bash
gcloud compute addresses create xss-test-ip-address --region=us-central1
```

#### 2\. Create a VM and assign the static IP

Launch an instance and install Python/Flask via a startup script:

```bash
gcloud compute instances create xss-test-vm-instance \
  --address=xss-test-ip-address \
  --no-service-account --no-scopes \
  --machine-type=e2-micro \
  --zone=us-central1-f \
  --metadata=startup-script='apt-get update; apt-get install -y python3-flask'
```

#### 3\. Create a temporary firewall rule for scanning

This rule is intentionally permissive for the scan and should be tightened or removed after scanning completes.

```bash
gcloud compute firewall-rules create enable-wss-scan \
  --direction=INGRESS --priority=1000 \
  --network=default --action=ALLOW \
  --rules=tcp:8080 --source-ranges=0.0.0.0/0
```

#### 4\. Deploy and start the application

Deploy the sample Flask app files provided by the lab. Start the app and verify it is reachable via the reserved static IP and port (for example: `http://<STATIC_IP>:8080`).

#### 5\. Proof-of-concept XSS payload (non-malicious demonstration)

Used to confirm the vulnerability in a controlled lab environment:

```html
<script>alert('This is an XSS Injection to demonstrate one of OWASP vulnerabilities')</script>
```

**Note:** This payload was used only for validation within the lab exercise.

#### 6\. Scan with Google Web Security Scanner

Enable the Web Security Scanner API in the GCP Console. Create and run a scan (e.g., title: “Cross-Site Scripting scan”) against the application URL. Monitor VM logs (SSH) and review results in the Web Security Scanner UI. The scanner should report the XSS finding.

-----

### Remediation

To prevent rendering user-supplied input as HTML, **escape special characters** before returning them to the client.

#### Vulnerable code

```python
output_string = input_string
```

#### Remediated code (example)

```python
output_string = "".join([html_escape_table.get(c, c) for c in input_string])
```

Ensure `html_escape_table` properly maps characters such as `<, >, &, " and '`. Prefer using a standard escaping function or the templating engine's auto-escaping when available.

-----

### Re-scan & Verification

1.  Restart the application after applying the fix.
2.  Re-run the Web Security Scanner against the same target.
3.  Confirm that the XSS finding is no longer present in the scan results.
4.  Correlate application and scanner logs to verify the exploit no longer executes.

-----

### Lab Issue / Support Note

The lab contains a verification bug: Section 5 does not correctly validate remediation progress, which may prevent the lab from marking as completed despite successful remediation and re-scanning. Report this behavior to TryHackMe support if encountered.

-----

### Lessons Learned & Best Practices

  - Prefer domain/path-level controls instead of broad IP-range blocks for cloud/CDN-hosted services to avoid collateral impact.
  - Use temporary and restrictive scan-time firewall rules; remove or tighten them after scans finish.
  - Prefer standardized escaping libraries or templating engines with auto-escaping over custom implementations.
  - Always verify remediation by rescanning and correlating logs.
  - Document actions and decisions to support auditability and knowledge transfer.

-----

### Next Steps (optional)

  - Integrate automated scanning into CI/CD pipelines for continuous verification.
  - Replace manual escaping with a vetted library or templating engine with built-in escaping.
  - Harden the VM: enforce least privilege, restrict service accounts, remove unnecessary packages, and apply OS hardening benchmarks.

-----

### Commit-ready checklist

  - [ ] Replace placeholder `<STATIC_IP>` with the actual reserved IP before sharing publicly.
  - [ ] Remove or secure the permissive firewall rule after verification.
  - [ ] Store remediation code in the repository with a short test demonstrating the fix.
  - [ ] Add references to the lab ticket or TryHackMe room ID if needed for traceability.
