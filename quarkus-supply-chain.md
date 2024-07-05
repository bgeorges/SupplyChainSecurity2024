---
marp: true
theme: 
paginate: true
---

# Software Supply Chain Security

## What, Why and how

JBug User Group Korea. Jul 5th, 2024

![bg right 33%](./assets/Logo-Red_Hat-A-Standard-RGB.svg)

---

## Introduction to Quarkus

- Supersonic Subatomic Java  :rocket:
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
