# Enterprise Cybersecurity Assessment & Penetration Testing Case Study

A controlled vulnerability assessment and penetration testing engagement evaluating a simulated two-site enterprise Active Directory environment. The assessment followed the **NIST SP 800-115** technical framework and mapped security findings and recommendations to **NIST Cybersecurity Framework (CSF) 2.0** and **CVSS v3** concepts.

---

## 📌 Project Overview

This repository documents an end-to-end cybersecurity assessment conducted against a simulated enterprise network representing the **SOAC Inc.** environment and the `SWANSEA.ACCA` Active Directory domain.

The environment consisted of two remote site subnets connected through a **site-to-site IPsec VPN**. The primary objective was to identify exposed attack surfaces, assess vulnerable services and cryptographic configurations, and evaluate whether network pivoting or cross-site lateral movement could occur across the environment.

### Key Technical Areas

* 🔎 Vulnerability Assessment & Network Reconnaissance
* 🛡️ Network Security & Infrastructure Hardening
* 🏢 Active Directory & Windows Security
* 🐧 Linux Server Security
* 🔀 Network Routing, Segmentation & Pivoting
* 🔐 Cryptographic Protocol & TLS Analysis
* 🧪 Controlled Penetration Testing
* 📊 Risk Assessment & CVSS
* 📋 NIST Cybersecurity Framework Mapping

---

## 📐 Network Architecture

The assessment environment simulated a two-site enterprise network connected through a site-to-site VPN.

```text
                         SITE-TO-SITE VPN
                 ┌──────────────────────────┐
                 │                          │
                 ▼                          ▼

      SWANSEA SUBNET                       LONDON SUBNET
      192.168.1.0/24                       192.168.2.0/24

 ┌───────────────────────┐            ┌───────────────────────┐
 │ Swansea Domain        │            │ London Server /       │
 │ Controller (S-DC)    │            │ Gateway (L-CD)        │
 │ Windows Server 2022  │            │ Debian Linux          │
 │ 192.168.1.1          │            │ 192.168.2.1           │
 └───────────┬───────────┘            └───────────┬───────────┘
             │                                    │
 ┌───────────▼───────────┐            ┌───────────▼───────────┐
 │ Swansea Client        │            │ London Client          │
 │ Windows 10            │            │ Debian Linux           │
 │ 192.168.1.10          │            │ 192.168.2.10           │
 └───────────────────────┘            └─────────────────────────┘
             ▲
             │
      ┌──────┴──────┐
      │ Kali Linux  │
      │ Assessment  │
      │ Host        │
      └─────────────┘
```

The assessment host was used to perform reconnaissance, vulnerability assessment and controlled security validation across the simulated environment.

> **Security Note:** All testing was performed within an isolated and authorised laboratory environment created for educational purposes.

---

## 🛠️ Tools & Technologies

| Category                    | Tools / Technologies                                                          |
| --------------------------- | ----------------------------------------------------------------------------- |
| Assessment & Reconnaissance | Kali Linux, Nmap, RPCclient, Netcat                                           |
| Vulnerability Assessment    | Nessus Essentials                                                             |
| Security Validation         | Metasploit Framework, Impacket, SMBClient                                     |
| Operating Systems           | Windows Server 2022, Windows 10, Debian Linux                                 |
| Infrastructure & Protocols  | Active Directory, Kerberos, SMB, SSH, TLS/SSL, IPv4 Routing, Site-to-Site VPN |
| Security Frameworks         | NIST SP 800-115, NIST CSF 2.0, CVSS v3                                        |

---

## 🔄 Assessment Methodology

The assessment followed a structured security testing methodology based on **NIST SP 800-115**.

```text
┌─────────────────┐
│ 1. Planning &   │
│    Engagement   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 2. Discovery &  │
│    Enumeration  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 3. Vulnerability│
│    Analysis     │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 4. Exploitation │
│    Validation   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ 5. Reporting &  │
│    Mitigation   │
└─────────────────┘
```

### Assessment Phases

**1. Planning & Engagement**

Defined the assessment scope, testing boundaries and non-destructive testing requirements.

**2. Discovery & Information Gathering**

Performed host discovery, network enumeration, service identification and topology verification.

**3. Vulnerability Analysis**

Used Nessus Essentials for uncredentialed vulnerability scanning and analysed findings using severity and CVSS concepts.

**4. Controlled Exploitation & Validation**

Performed controlled security validation using Metasploit and Impacket to determine whether identified weaknesses could realistically contribute to compromise or lateral movement.

**5. Mitigation & Governance**

Developed layered security recommendations and mapped them to relevant areas of the NIST CSF 2.0 Protect function.

---

## 🔎 Reconnaissance & Enumeration

Initial reconnaissance identified four primary systems across the two simulated sites:

| Host           | Operating System    |   IP Address | Key Role          |
| -------------- | ------------------- | -----------: | ----------------- |
| S-DC           | Windows Server 2022 |  192.168.1.1 | Domain Controller |
| Swansea Client | Windows 10          | 192.168.1.10 | Domain Client     |
| L-CD           | Debian Linux        |  192.168.2.1 | Server / Gateway  |
| London Client  | Debian Linux        | 192.168.2.10 | Linux Client      |

Service enumeration identified infrastructure commonly associated with the enterprise environment, including:

* DNS
* Kerberos
* SMB
* SSH
* TLS/SSL services

The assessment also evaluated network reachability between the two simulated sites and identified conditions that could support cross-subnet movement.

---

## 📊 Vulnerability Assessment

Nessus Essentials was used to perform an **uncredentialed vulnerability assessment** of the environment.

The assessment identified vulnerabilities and security weaknesses across different severity levels.

### Key Findings

| Finding                           | Severity   | Validation / Identification     | Security Impact                                                                            |
| --------------------------------- | ---------- | ------------------------------- | ------------------------------------------------------------------------------------------ |
| SWEET32 / Weak 64-bit TLS Ciphers | **High**   | Nessus / Nmap                   | Potential confidentiality impact through weaknesses in legacy cryptographic configurations |
| Legacy SSL/TLS Protocol Support   | **High**   | Nessus / Nmap                   | Increased exposure to downgrade and interception risks                                     |
| IPv4 Forwarding Enabled           | **Medium** | System configuration inspection | Could facilitate network pivoting between subnets                                          |
| Password-based SSH Authentication | **Medium** | Service/security assessment     | Increases exposure of administrative access to credential-based attacks                    |
| SMB Signing Not Required          | **Low***   | SMB security-mode assessment    | Creates a security precondition relevant to SMB relay attacks                              |
| Service Version Disclosure        | **Low**    | Nmap / banner analysis          | Provides additional information for reconnaissance                                         |

> **Important Context:** The severity of an individual configuration finding does not always represent its complete attack-chain impact. For example, SMB signing not being required becomes more significant when combined with an applicable authentication coercion and relay scenario.

---

## 🧪 Exploitation Validation

A key objective of the assessment was to determine whether identified weaknesses could be practically associated with compromise or lateral movement.

### Negative Exploitation Results

Not all exploitation attempts were successful.

A controlled assessment was performed against the Windows client to determine whether it was vulnerable to the well-known **MS17-010 / EternalBlue** vulnerability. The assessment indicated that the host did not appear vulnerable.

A controlled SSH authentication assessment against the Debian gateway also did not result in successful compromise.

### Security Significance

These unsuccessful attempts are important assessment results rather than failures of the project.

They provided evidence that certain baseline security controls, such as patching and account security, were resisting common attack techniques.

> **Key takeaway:** Effective penetration testing is not measured only by obtaining a shell or administrative access. Demonstrating that known attack techniques are unsuccessful is also valuable evidence of security resilience.

---

## 🔀 Attack Path Analysis

The assessment identified two important security conditions that could contribute to lateral movement in a realistic attack scenario.

### Attack Path A — Network Pivoting

IPv4 forwarding was identified as enabled on the London server/gateway.

This configuration allowed the host to participate in routing traffic between network segments, creating a potential pivot point if the system were compromised.

```text
[Kali Assessment Host]
          │
          ▼
[London Server / Gateway]
          │
          │ IPv4 forwarding enabled
          ▼
[Remote Network Segment]
          │
          ▼
[Swansea Network]
```

**Security implication:**

A compromised intermediary system with routing capabilities could potentially provide an attacker with additional network visibility or access beyond the originally compromised segment.

---

### Attack Path B — SMB Relay Precondition

The assessment identified that SMB message signing was enabled but **not required** on a relevant host.

This configuration represents an important security weakness because mandatory SMB signing is a key defensive control against certain SMB relay scenarios.

```text
[Authentication Traffic]
          │
          ▼
[SMB Service]
          │
          ▼
[Signing Enabled]
[Signing Not Required]
          │
          ▼
[Relay Risk Increased]
```

The assessment confirmed the relevant security precondition, but a complete live relay attack was **not successfully demonstrated** because the required victim authentication event was not available during testing.

This distinction is important: the repository documents a **validated security condition**, rather than claiming successful unauthorised access where it was not achieved.

---

## 🛡️ Security Recommendations

The identified findings were mapped to security improvements aligned with the **Protect (PR)** function of the NIST CSF 2.0.

| Security Area        | Finding                   | Recommended Action                                                                 |
| -------------------- | ------------------------- | ---------------------------------------------------------------------------------- |
| Cryptography         | SWEET32 / Legacy Ciphers  | Disable 3DES and legacy cryptographic protocols; enforce modern TLS configurations |
| Network Security     | IPv4 Forwarding           | Disable forwarding on systems that do not require routing functionality            |
| Network Segmentation | Cross-site movement risk  | Implement restrictive inter-site firewall rules and ACLs                           |
| Authentication       | Password-based SSH        | Prefer SSH key authentication and stronger authentication controls                 |
| SMB Security         | Signing not required      | Enforce mandatory SMB signing through appropriate domain security policies         |
| Network Architecture | Broad trust between sites | Use routed and filtered VPN architecture with least-privilege communication        |

### Recommended Security Architecture

A more secure design would introduce explicit filtering and segmentation between the two sites.

```text
                         INTERNET / BACKBONE
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │ Firewall / VPN      │
                       │ Gateway             │
                       └──────────┬──────────┘
                                  │
                                  ▼
                       ┌─────────────────────┐
                       │ Inter-Site ACL &    │
                       │ Traffic Filtering   │
                       └──────────┬──────────┘
                                  │
                   ┌──────────────┴──────────────┐
                   ▼                             ▼

          SWANSEA NETWORK                  LONDON NETWORK
          192.168.1.0/24                   192.168.2.0/24

       ┌─────────────────┐              ┌─────────────────┐
       │ Active Directory│              │ Internal        │
       │ / Domain        │              │ Clients &       │
       │ Controller      │              │ Services        │
       └─────────────────┘              └─────────────────┘
```

> **Implementation Status:** The controls described in this section are **security recommendations** derived from the assessment. They should not be interpreted as controls that were fully implemented during the laboratory assessment.

---

## ⚠️ Assessment Limitations

Several limitations affected the scope and interpretation of the assessment.

### Uncredentialed Vulnerability Scanning

Nessus was operated without administrative credentials. Consequently, the assessment primarily evaluated externally visible services and configurations rather than providing complete host-level visibility.

### Virtual Laboratory Environment

The assessment was performed in an isolated virtualised environment. The laboratory did not reproduce all controls normally present in a production enterprise environment, such as enterprise-grade firewalls, IDS/IPS, EDR platforms and centralised security monitoring.

### Tool Stability

Large-scale Nmap discovery scans experienced occasional instability within the laboratory environment. The discovery process was therefore adapted to smaller scanning batches.

### Interpretation of Results

The findings should be interpreted as evidence from a controlled academic security assessment rather than as a complete production penetration test.

---

## 📁 Repository Structure

The repository is organised to separate assessment evidence, analysis and security recommendations.

```text
Enterprise-Cybersecurity-Assessment/
│
├── README.md
│
├── architecture/
│   └── network-diagram.png
│
├── discovery-recon/
│   ├── nmap-subnet-discovery.txt
│   └── service-enumeration-summary.md
│
├── vulnerability-assessment/
│   ├── nessus-scan-summary.pdf
│   └── cvss-risk-analysis.md
│
├── exploitation-validation/
│   ├── exploitation-results.md
│   ├── smb-signing-assessment.md
│   └── pivoting-assessment.md
│
├── mitigations/
│   └── hardening-guide-nist-csf.md
│
└── documentation/
    └── Technical-Assessment-Summary.pdf
```

---

## 🎯 Project Outcomes

This project provided practical experience in:

* Network reconnaissance and service enumeration
* Vulnerability assessment using Nessus
* Network security analysis using Nmap
* Windows and Active Directory security assessment
* Linux server security assessment
* SMB and SSH security analysis
* Cryptographic and TLS configuration assessment
* Controlled penetration testing
* Network pivoting and lateral movement analysis
* Security risk assessment
* NIST-based security recommendations
* Professional cybersecurity reporting

The assessment demonstrated that an enterprise environment can resist common exploitation techniques while still containing architectural and configuration weaknesses that may increase the risk of lateral movement.

---

## 📚 Frameworks & Standards

This assessment used or referenced:

* **NIST SP 800-115** — Technical Guide to Information Security Testing and Assessment
* **NIST Cybersecurity Framework (CSF) 2.0**
* **Common Vulnerability Scoring System (CVSS)**
* **Active Directory security principles**
* **Network segmentation and least-privilege concepts**

---

## 📄 Documentation

Additional project documentation can be stored in the `documentation/` directory.

The supporting documentation provides deeper information regarding the assessment methodology, findings, technical evidence and security analysis.

---

## ⚖️ Disclaimer

All security testing documented in this repository was conducted within a **fully isolated and authorised laboratory environment** for educational and professional portfolio purposes.

The techniques and findings presented here should not be applied to systems or networks without explicit authorisation.

---

## 👤 Author

**Pedro Gaetjens Molongua**

BSc Applied Computing Student
Networking & Cybersecurity Enthusiast

**GitHub:** [@PedrxG](https://github.com/PedrxG)

**Focus Areas:**
Networking · Cybersecurity · IT Support · Cloud Infrastructure · Network Security
