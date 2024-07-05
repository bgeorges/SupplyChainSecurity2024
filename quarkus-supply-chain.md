---
marp: true
theme: default
header: |
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
  <link rel="stylesheet" href="custom-styles.css">
---

# Quarkus in Secure Supply Chain

## Red Hat

![bg right](https://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/RedHat.svg/1200px-RedHat.svg.png)

---

## Introduction to Quarkus

- Supersonic Subatomic Java
- Designed for Kubernetes and optimized for GraalVM and OpenJDK HotSpot

---

## Secure Supply Chain

- Importance of securing the supply chain
- Threats and vulnerabilities in software supply chains

---

## Building with External Dependencies

1. Managing dependencies
2. Ensuring the integrity of dependencies
3. Using trusted sources and repositories

---

## Generating SBOM Artifact

- What is an SBOM?
- Tools and practices for generating SBOMs
- Example: Using Syft to generate an SBOM

---

## Signing with Sigstore

- What is Sigstore?
- Benefits of signing artifacts
- Example: Signing Quarkus artifacts with Sigstore

---

## Key Points on Secure Supply Chain Attacks

| Key Points | ![Supply Chain Image](https://slsa.dev/spec/v1.0/threats-overview/supply-chain-threats.png) |
|------------|---------------------------------------------------------------------------------------------|
| - Tampering with source code | |
| - Compromised dependencies | |
| - Malicious build processes | |
| - Unauthorized package uploads | |

---

## Conclusion

- Summary of secure supply chain practices with Quarkus
- Importance of generating SBOMs and signing artifacts
- Red Hat's commitment to security
