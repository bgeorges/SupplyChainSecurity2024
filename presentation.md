---
marp: true
theme: default
header: "Software Secure Supply Chain"
footer: "Developer Meetup"
paginate: true
backgroundColor: #fff
---

# Software Secure Supply Chain

## Introduction

- Importance of secure supply chains
- Key concepts and practices

---

## Early Software Supply Chains

### Example: Perl and C CGI Scripts

#### Perl CGI Example

```perl
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<html><body>Hello, World!</body></html>";

#### C CGI Example

```c
#include <stdio.h>

int main() {
    printf("Content-Type: text/html\n\n");
    printf("<html><body>Hello, World!</body></html>");
    return 0;
}

- No external dependencies
- Simple, isolated scripts


### Introduction of Dependencies
### Late 1990s - Early 2000s

   -  Use of libraries like cgi-bin, Mail::Form in Perl
   -  Increased functionality, but also new vulnerabilities


### Modern Frameworks: SSDF and SLSA
#### SSDF (Secure Software Development Framework)

 -   Framework for integrating security practices into software development

#### SLSA (Supply Chain Levels for Software Artifacts)

   -  Framework for ensuring the integrity of software artifacts

##### Components

   - SBOM (Software Bill of Materials)
   - Sigstore for signing and verifying software



#### Examples: Identifying and Remediating Vulnerabilities
### Java Example: Log4j CVE
### Identifying CVE in Log4j

```c

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class HelloWorld {
    private static final Logger logger = LogManager.getLogger(HelloWorld.class);

    public static void main(String[] args) {
        logger.info("Hello, World!");
    }
}



-    CVE-2021-44228: Remote code execution vulnerability in Log4j

##### Remediation

   -  Update Log4j to a version that patches the vulnerability
   -  Example: Upgrade to Log4j 2.16.0

```xml
<dependency>
  <groupId>org.apache.logging.log4j</groupId>
  <artifactId>log4j-core</artifactId>
  <version>2.16.0</version>
</dependency>


#### Conclusion

-    Importance of secure software supply chains
 -   Adopting frameworks like SSDF and SLSA
  -  Utilizing tools like SBOM and Sigstore for a secure foundation
