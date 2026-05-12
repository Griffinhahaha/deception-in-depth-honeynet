# **Deception-in-Depth HoneyNet: Layered Design Blueprint**

**Purpose:** This document provides a detailed, implementation-oriented
design for each deception layer in the HoneyNet stack. The goal is to
balance **realism** with **controlled vulnerability** while remaining
safe and academically defensible.

The design is **evidence-informed, not prescriptive**.

## **Design Principles (Applies to All Layers)**

**Plausibility over Perfection**

**Progressive Friction**

**Observable by Default**

**Safe Containment**

## **Layer 1 -- Network-Level Deception (Initial Access & Reconnaissance)**

**Objective:** Attract opportunistic attackers with exposed services and
plausible network topology.

**Core Components:** T-Pot CE, SSH, RDP, HTTP(S), SMB, FTP, simulated
subnets.

**Realism & Vulnerabilities:** Mixed banners, weak credentials on
selected services, minor misconfigurations.

**Expected Behavior:** Scanning, credential brute force.

**MITRE ATT&CK Coverage:**

  -------------------------------------------------
  **Tactic**            **Example Technique**
  --------------------- ---------------------------
  Reconnaissance        Scanning IPs/ports (T1595)
  (TA0001)              

  Initial Access        Brute force login attempts
  (TA0001)              (T1110)
  -------------------------------------------------

**Open Source Readiness:**

T-Pot CE is largely ready, requires network adjustments and credential
seeding.

Customization: moderate; \~1--2 weeks for realistic service banners,
credential placement.

Complexity: low-medium, mainly configuration.

## **Layer 2 -- Host-Level Deception (Privilege Escalation & Lateral Movement)**

**Objective:** Present believable internal hosts and AD structure.

**Core Components:** GOAD lab, Windows clients, servers, domain
controller.

**Realism & Vulnerabilities:** Non-optimal AD groups, legacy users, weak
passwords, fake admin accounts.

**Expected Behavior:** Enumeration, credential harvesting, privilege
escalation attempts.

**MITRE ATT&CK Coverage:**

  --------------------------------------------------------
  **Tactic**               **Example Technique**
  ------------------------ -------------------------------
  Credential Access        Kerberoasting (T1558.003)
  (TA0006)                 

  Privilege Escalation     Exploit misconfigured services
  (TA0004)                 (T1068)

  Lateral Movement         Remote Desktop Protocol
  (TA0008)                 (T1021.001)
  --------------------------------------------------------

**Open Source Readiness:**

GOAD provides a base AD environment.

Customization: moderate to heavy; 2--3 weeks for account setup, GPO
misconfigurations, and decoy placement.

Complexity: medium; requires understanding of AD structure.

## **Layer 3 -- Data / Cloud-Level Deception (Impact & Exfiltration Simulation)**

**Objective:** Simulate data assets and cloud access to attract advanced
attackers.

**Core Components:** HoneyLambda, decoy S3 buckets, fake API keys.

**Realism & Vulnerabilities:** Plausible naming, discoverable
credentials, accessible buckets.

**Expected Behavior:** Credential reuse, cloud enumeration, simulated
exfiltration.

**MITRE ATT&CK Coverage:**

  ----------------------------------------------
  **Tactic**         **Example Technique**
  ------------------ ---------------------------
  Collection         Access cloud storage
  (TA0009)           (T1530)

  Exfiltration       Cloud storage exfiltration
  (TA0010)           (T1567)
  ----------------------------------------------

**Open Source Readiness:**

HoneyLambda and fake S3 scripts are partially ready.

Customization: moderate; 1--2 weeks to seed keys, configure buckets, and
logging.

Complexity: low-medium, mainly scripting and API setup.

## **Balancing Realism vs. Lure**

  -----------------------------------------------------------------
  **Dimension**     **Higher       **Higher     **Chosen Balance**
                    Realism**      Lure**       
  ----------------- -------------- ------------ -------------------
  Vulnerabilities   subtle,        obvious      moderate, chained
                    chained                     

  Credentials       hard to find   easy         discoverable but
                                                scoped

  Progression       slow, logical  fast, noisy  guided but
                                                misleading

  Cost              high           low          academic-feasible
  -----------------------------------------------------------------

## **What NOT to Build (Anti-Patterns)**

**Real assets or live credentials** -- Never mix with actual production
data.

**Uncontained exploits** -- Avoid scripts or malware that can escape
environment.

**Overly predictable paths** -- Entire environment should not be trivial
to traverse.

**Overly complex services** -- Avoid deploying full enterprise apps that
increase setup time and instability.

**No logging** -- All actions must be observable.

## **What NOT to Build: Common Anti-Patterns**

These anti-patterns reduce realism, invalidate measurements, or create
operational risk. They should be explicitly avoided.

**Overtly Fake Assets**

Obvious placeholder hostnames (e.g., test123, admin_fake)

Empty files or static \"flag\" content

Perfectly uniform system configurations

*Impact:* Skilled attackers disengage early; engagement metrics
collapse.

**Single-Step Total Compromise**

One vulnerability granting immediate \"domain admin\"-like access

*Impact:* No measurable misdirection or delay; Layer 2 becomes
meaningless.

**Over-Hardened Systems**

Patched-to-the-edge services

Strong credentials everywhere

*Impact:* No attacker progression; project becomes a passive sensor
only.

**Unobservable Deception**

Missing authentication logs

No process or command telemetry

*Impact:* Actions cannot be mapped to ATT&CK; data unusable for thesis.

**Real External Trust or Credentials**

Real cloud accounts

Real API keys or outbound access

*Impact:* Ethical and institutional risk; violates academic safety
boundaries.

## **MITRE ATT&CK Coverage by Layer**

  ----------------------------------------------------------------------
  **Layer**   **Primary         **Example Techniques**  **Observable
              Tactics**                                 Evidence**
  ----------- ----------------- ----------------------- ----------------
  Layer 1:    TA0001            T1046 Network Service   Port scans,
  Network     Reconnaissance    Scanning                banner grabs

              TA0006 Credential T1110 Brute Force       Failed login
              Access                                    attempts

              TA0002 Resource   T1583 Acquire           Proxy usage
              Development       Infrastructure          patterns

  Layer 2:    TA0004 Privilege  T1068 Exploitation for  Process
  Host / AD   Escalation        Privilege Escalation    creation, token
                                                        abuse

              TA0008 Lateral    T1021 Remote Services   SMB/RDP
              Movement                                  authentication
                                                        logs

              TA0003            T1053 Scheduled Task    Registry and
              Persistence                               task creation

  Layer 3:    TA0009 Collection T1530 Data from Cloud   Object listing
  Cloud /                       Storage                 attempts
  Data                                                  

              TA0010            T1567 Exfiltration Over API usage
              Exfiltration      Web Services            anomalies

              TA0005 Defense    T1070 Indicator Removal Log tampering
              Evasion                                   attempts
  ----------------------------------------------------------------------

## **Open-Source Component Readiness and Customization Effort**

  ----------------------------------------------------------------------------------------
  **Component**   **Readiness    **Modification Needed**   **Customization   **Estimated
                  for Design**                             Complexity**      Effort**
  --------------- -------------- ------------------------- ----------------- -------------
  T-Pot CE        High           Service selection, banner Low--Medium       1--2 weeks
                                 tuning, logging alignment                   

  GOAD            Medium--High   AD topology tweaks, decoy Medium            2--3 weeks
                                 account design, logging                     

  HoneyLambda     Medium         Token placement, alert    Medium            1 week
                                 tuning, log parsing                         

  Decoy S3 /      Medium         Naming realism, access    Low               \<1 week
  Storage                        policy shaping                              

  Central Logging Medium         Normalization, session    Medium            1--2 weeks
                                 correlation                                 
  ----------------------------------------------------------------------------------------

**Notes:**

Most effort lies in **designing believable misconfigurations**, not
coding.

Infrastructure skills are more important than exploit development.

## **Overall Workload and Complexity Assessment**

**Total Project Duration (build + tuning):** \~6--8 weeks

**Technical Difficulty:** Moderate

**Primary Risk Areas:** Logging gaps, over-simplification, time overruns

**Best Parallelization:** Layer 1, Layer 2, and logging can be built
concurrently
