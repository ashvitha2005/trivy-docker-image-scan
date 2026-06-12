# Docker Image Vulnerability Scanning and Remediation using Trivy

## Objective

The objective of this lab is to scan a Docker image using Trivy, identify vulnerabilities, understand the vulnerability report, fix the vulnerable package using a Dockerfile, rebuild the image, and verify the fix by rescanning the image.

This lab demonstrates a simple DevSecOps workflow:

```text
Scan → Identify Vulnerability → Fix → Rebuild Image → Rescan
```

---

## What is Trivy?

Trivy is an open-source vulnerability scanner used to scan Docker images, containers, filesystems, repositories, and cloud configurations.

It helps identify:

- Known vulnerabilities
- CVEs (Common Vulnerabilities and Exposures)
- Misconfigurations
- Secrets
- Vulnerable packages

In this lab, Trivy was used to scan a Docker image for operating system package vulnerabilities.

---

## Image Scanned

The Docker image scanned was:

```text
nginx:alpine
```

This image contains the Nginx web server running on Alpine Linux, a lightweight Linux distribution commonly used in containers.

---

## Pull the Docker Image

```bash
docker pull nginx:alpine
```

---

## Scan the Image Using Trivy

```bash
trivy image --pkg-types os nginx:alpine
```

---

## Initial Scan Report

### Target

```text
nginx:alpine
```

### Base OS

```text
Alpine 3.23.4
```

### Vulnerability Summary

| Severity | Count |
|------------|-------|
| CRITICAL | 0 |
| HIGH | 1 |
| MEDIUM | 0 |
| LOW | 0 |

---

## Vulnerable Package Identified

| Package | Severity | Installed Version | Fixed Version |
|-----------|----------|------------------|---------------|
| libxml2 | HIGH | 2.13.9-r0 | 2.13.9-r1 |

---

## Understanding the Report

Trivy reported that the `libxml2` package inside the Docker image contained a known vulnerability.

The installed version:

```text
2.13.9-r0
```

was vulnerable.

Trivy also showed that a fixed version was available:

```text
2.13.9-r1
```

This indicates that updating the package would remediate the vulnerability.

---

## Remediation

A custom Dockerfile was created to update the vulnerable package and rebuild the image.

### Dockerfile

```dockerfile
FROM nginx:alpine

RUN apk update && apk upgrade libxml2
```

---

## Build the Fixed Image

```bash
docker build -t nginx-alpine-fixed .
```

---

## Rescan the Fixed Image

```bash
trivy image --pkg-types os nginx-alpine-fixed
```

---

## Final Scan Result

### Target

```text
nginx-alpine-fixed
```

### Vulnerability Summary

| Severity | Count |
|------------|-------|
| CRITICAL | 0 |
| HIGH | 0 |
| MEDIUM | 0 |
| LOW | 0 |

```text
Total Vulnerabilities: 0
```

This confirms that the vulnerable package was successfully updated.

---

## DevSecOps Workflow Demonstrated

```text
Docker Image
      ↓
Trivy Scan
      ↓
Identify Vulnerability
      ↓
Update Vulnerable Package
      ↓
Build New Image
      ↓
Rescan with Trivy
      ↓
Verify Remediation
```

---

## Concepts Learned

- Docker images
- Alpine Linux packages
- Trivy vulnerability scanning
- CVEs and vulnerability reports
- Operating system package vulnerabilities
- Package remediation
- Dockerfile updates
- Image rebuilding
- Verification through rescanning
- Basic DevSecOps workflow

---

## Conclusion

This lab demonstrated how Trivy can be used to scan Docker images, identify vulnerabilities, and verify remediation.

The initial scan identified a HIGH severity vulnerability in the `libxml2` package. A custom Docker image was built after updating the package, and a subsequent scan reported zero vulnerabilities.

This lab illustrates a fundamental DevSecOps practice:

> Scan → Identify → Fix → Rebuild → Verify

and highlights the importance of continuously scanning container images to reduce security risks before deployment.
