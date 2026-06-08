Docker Image Vulnerability Scanning and Remediation using Trivy

Objective

The objective of this lab is to scan a Docker image using Trivy, identify vulnerabilities, understand the vulnerability report, fix the vulnerable package using a Dockerfile, rebuild the image, and verify the fix by rescanning the image.

This lab demonstrates a basic DevSecOps workflow:
Scan → Identify Vulnerability → Fix → Rebuild Image → Rescan

What is Trivy?

Trivy is an open-source vulnerability scanner used to scan Docker images, containers, filesystems, repositories, and cloud configurations.

It helps identify:

* Known vulnerabilities
* CVEs
* Misconfigurations
* Secrets
* Vulnerable packages

In this lab, Trivy was used to scan a Docker image for OS-level vulnerabilities

Image Scanned

The docker image scanned was nginx:alpine
This image contained the Nginx Web Server built on Alpine Linux which is a small Linux OS.

Steps

1. Pull the Docker Image: docker pull nginx:alpine
2. Scan the image using Trivy: trivy image --pkg-types os nginx:alpine

3. Initial Scan Report

Target: nginx:alpine
Base OS: Alpine 3.23.4

Total Vulnerabilities: 1
HIGH: 1
CRITICAL: 0

Library: libxml2
Severity: HIGH
Installed Version: 2.13.9-r0
Fixed Version: 2.13.9-r1
Status: fixed

Understanding the Report

Trivy reported that the libxml2 package inside the Docker image had a known vulnerability.
In this case, the installed version of libxml2 was vulnerable, and Trivy showed that a fixed version was available.

4. A Dockerfile was created to rebuild the image and update the vulnerable package.
5. Build the Fixed Docker Image
6. Rescan the Fixed Image: trivy image --pkg-types os nginx-alpine-fixed
7. Result: After rebuilding the image and rescanning it with Trivy, the result showed Total Vulnerabilities:0

This confirms that the vulnerable package was updated successfully

Conclusion

This lab demonstrated how Trivy can be used to scan Docker images, identify vulnerabilities, fix them by updating packages through a Dockerfile, rebuild the image, and verify that the vulnerabilities were resolved.

The final scan showed 0 vulnerabilities, confirming that the remediation step was successful.

