**Capstone Project Proposal**

Design and Evaluation of a Deception-in-Depth Honeynet Architecture for
Observing Multi-Stage Attacker Behaviour

# Contents {#contents .TOC-Heading}

[Executive Summary [4](#executive-summary)](#executive-summary)

[1. Introduction [4](#introduction)](#introduction)

[2. Project Objectives [5](#project-objectives)](#project-objectives)

[3. Project Scope [5](#project-scope)](#project-scope)

[3.1 Research Questions [6](#research-questions)](#research-questions)

[3.2 Study Hypothesis [6](#study-hypothesis)](#study-hypothesis)

[4. System Architecture [7](#system-architecture)](#system-architecture)

[4.1 Telemetry Instrumentation
[8](#telemetry-instrumentation)](#telemetry-instrumentation)

[4.2 Interaction Sequence Reconstruction
[8](#interaction-sequence-reconstruction)](#interaction-sequence-reconstruction)

[4.3 Attacker Path Visualization
[8](#attacker-path-visualization)](#attacker-path-visualization)

[4.4 Deception Control Plane
[9](#deception-control-plane)](#deception-control-plane)

[5. Experimental Design [9](#experimental-design)](#experimental-design)

[6. Behavioural Metrics
[10](#behavioural-metrics)](#behavioural-metrics)

[6.1 Evaluation Method [10](#evaluation-method)](#evaluation-method)

[6.2 Threat Model [11](#threat-model)](#threat-model)

[7. MITRE ATT&CK Mapping
[11](#mitre-attck-mapping)](#mitre-attck-mapping)

[8. Feasibility and Risk Management
[11](#feasibility-and-risk-management)](#feasibility-and-risk-management)

[9. Project Schedule and Learning Hours
[12](#project-schedule-and-learning-hours)](#project-schedule-and-learning-hours)

[10. Expected Deliverables
[12](#expected-deliverables)](#expected-deliverables)

[11. Expected Contributions
[13](#expected-contributions)](#expected-contributions)

[Appendix A -- Implementation Plan
[14](#appendix-a-implementation-plan)](#appendix-a-implementation-plan)

[A.1 Purpose [14](#a.1-purpose)](#a.1-purpose)

[A.2 Deployment Platform
[14](#a.2-deployment-platform)](#a.2-deployment-platform)

[A.3 Planned Virtual Machine Layout
[14](#a.3-planned-virtual-machine-layout)](#a.3-planned-virtual-machine-layout)

[A.4 Rationale for Feasibility
[15](#a.4-rationale-for-feasibility)](#a.4-rationale-for-feasibility)

[A.5 Proposed Network Layout
[16](#a.5-proposed-network-layout)](#a.5-proposed-network-layout)

[A.6 Logical Traffic Flow
[16](#a.6-logical-traffic-flow)](#a.6-logical-traffic-flow)

[A.7 Planned Role of Each VM
[17](#a.7-planned-role-of-each-vm)](#a.7-planned-role-of-each-vm)

[A.8 Deception Asset Placement Plan
[19](#a.8-deception-asset-placement-plan)](#a.8-deception-asset-placement-plan)

[A.9 Implementation Sequence
[20](#a.9-implementation-sequence)](#a.9-implementation-sequence)

[A.10 Snapshot and Reset Strategy
[21](#a.10-snapshot-and-reset-strategy)](#a.10-snapshot-and-reset-strategy)

[A.11 Key Manageability Considerations
[21](#a.11-key-manageability-considerations)](#a.11-key-manageability-considerations)

[A.12 Summary [22](#a.12-summary)](#a.12-summary)

[Appendix B -- Experimental Telemetry Schema
[22](#appendix-b-experimental-telemetry-schema)](#appendix-b-experimental-telemetry-schema)

[B.1 Purpose [22](#b.1-purpose)](#b.1-purpose)

[B.2 Logging Architecture Overview
[23](#b.2-logging-architecture-overview)](#b.2-logging-architecture-overview)

[B.3 Core Telemetry Datasets
[23](#b.3-core-telemetry-datasets)](#b.3-core-telemetry-datasets)

[B.4 Session Log [24](#b.4-session-log)](#b.4-session-log)

[B.5 Event Log [24](#b.5-event-log)](#b.5-event-log)

[B.6 Artifact Interaction Log
[25](#b.6-artifact-interaction-log)](#b.6-artifact-interaction-log)

[B.7 Interaction Edge Log
[26](#b.7-interaction-edge-log)](#b.7-interaction-edge-log)

[B.8 Control Plane Trigger Log
[27](#b.8-control-plane-trigger-log)](#b.8-control-plane-trigger-log)

[B.9 Attacker Path Reconstruction
[28](#b.9-attacker-path-reconstruction)](#b.9-attacker-path-reconstruction)

[B.10 Alignment with Behavioural Metrics
[28](#b.10-alignment-with-behavioural-metrics)](#b.10-alignment-with-behavioural-metrics)

[Appendix C -- Telemetry Processing Pipeline
[29](#appendix-c-telemetry-processing-pipeline)](#appendix-c-telemetry-processing-pipeline)

[C.1 Purpose [29](#c.1-purpose)](#c.1-purpose)

[C.2 Pipeline Architecture
[29](#c.2-pipeline-architecture)](#c.2-pipeline-architecture)

[C.3 Log Sources [30](#c.3-log-sources)](#c.3-log-sources)

[C.4 Event Normalization
[30](#c.4-event-normalization)](#c.4-event-normalization)

[C.5 Session Identification
[31](#c.5-session-identification)](#c.5-session-identification)

[C.6 Artifact Identification
[31](#c.6-artifact-identification)](#c.6-artifact-identification)

[C.7 Interaction Edge Generation
[32](#c.7-interaction-edge-generation)](#c.7-interaction-edge-generation)

[C.8 Structured Data Export
[32](#c.8-structured-data-export)](#c.8-structured-data-export)

[C.9 Analysis and Visualization
[33](#c.9-analysis-and-visualization)](#c.9-analysis-and-visualization)

[C.10 Implementation Considerations
[33](#c.10-implementation-considerations)](#c.10-implementation-considerations)

**\**

# Executive Summary

Cyber deception technologies such as honeypots are widely used to
observe malicious activity and study attacker behaviour within
controlled environments. However, many honeypot deployments simulate
only isolated services and therefore provide limited visibility into
attacker activity after initial access.

This project proposes the design and evaluation of a
**Deception-in-Depth honeynet architecture** that integrates deception
mechanisms across multiple infrastructure layers, including
network-level honeypots, host-level enterprise deception, and data-level
decoy assets.

The system will simulate a small enterprise environment using
virtualization technology. Attackers interacting with the environment
will encounter multiple deceptive assets intended to influence their
navigation and behaviour. To support structured behavioural observation,
the architecture introduces a **Deception Control Plane**, which
conditionally activates deeper deception layers when specific attacker
actions are detected.

The project evaluates attacker interaction using three behavioural
indicators:

- **misdirection** -- interaction with deception artifacts

- **delay** -- time spent navigating deceptive paths

- **engagement depth** -- progression through multiple layers of the
  environment

Telemetry collected from the honeynet will be normalized into structured
interaction records that allow reconstruction of attacker navigation
paths and identification of interactions with deception artifacts.

Observed activities will also be mapped to **MITRE ATT&CK tactics** to
provide structured interpretation of attacker behaviour.

The environment will be deployed on a **Proxmox-based virtualization
platform** using approximately four to six virtual machines representing
honeypot services, enterprise hosts, and monitoring infrastructure.

The final deliverables include a working layered honeynet environment, a
documented experimental methodology, and behavioural analysis of
observed attacker activity.

The project prioritizes **experimental clarity, reproducibility, and
manageable system scale**, ensuring feasibility within the available
hardware resources and capstone timeframe.

# 1. Introduction

Cyber deception technologies such as honeypots are widely used to
observe malicious activity and collect intelligence about attacker
behaviour. Traditional honeypots typically simulate a single vulnerable
service and record interactions with that service. While effective for
detecting automated attacks, isolated honeypots provide limited insight
into attacker behaviour once access to the system has been established.

In real intrusion scenarios, attackers often perform multiple stages of
activity including reconnaissance, credential discovery, lateral
movement, and data access. Recent research suggests that **layered
deception architectures**, where deception mechanisms exist across
different infrastructure layers, can provide richer behavioural insights
into attacker decision-making.

This project proposes the design and evaluation of a
**Deception-in-Depth honeynet architecture** that integrates deception
across network services, enterprise hosts, and data-level assets.

The honeynet environment will simulate a small enterprise network and
allow researchers to observe attacker behaviour across multiple stages
of activity. Behavioural indicators such as misdirection, delay, and
engagement depth will be used to evaluate how deception influences
attacker navigation decisions.

The experimental design is guided by a set of research questions
focusing on how layered deception influences attacker navigation,
engagement, and decision-making within the honeynet environment.

The project emphasizes **controlled experimentation and behavioural
observation** rather than large-scale production deployment.

# 2. Project Objectives

The primary objective of this project is to design, implement, and
evaluate a layered honeynet environment capable of observing multi-stage
attacker behaviour.

**Objective 1 -- Design a layered deception architecture**

Develop an experimental honeynet architecture integrating deception
mechanisms across network, host, and data layers.

**Objective 2 -- Implement a controlled honeynet environment**

Deploy the architecture within a virtualized infrastructure using
open-source tools and simulated enterprise hosts.

**Objective 3 -- Capture multi-stage attacker interactions**

Collect telemetry describing attacker behaviour including scanning,
login attempts, command execution, and access to deception artifacts.

**Objective 4 -- Evaluate behavioural impact of layered deception**

Measure how deception influences attacker navigation using behavioural
metrics including misdirection, delay, and engagement depth.

**Objective 5 -- Interpret attacker behaviour using MITRE ATT&CK**

Map observed activities to high-level MITRE ATT&CK tactics to provide
structured interpretation of the behaviour captured during experiments.

# 3. Project Scope

The project focuses on constructing a **controlled honeynet environment
for behavioural observation** rather than developing a production
security platform.

The scope includes:

- designing a layered deception architecture

- deploying the environment using virtualization

- collecting telemetry from attacker interaction

- analysing behavioural patterns across deception layers

The project does not attempt to simulate a full enterprise
infrastructure or implement advanced detection systems. Instead, the
emphasis is on **controlled experimentation and behavioural
observation**.

## 3.1 Research Questions

The study is guided by the following research questions.

**RQ1 -- How does layered deception influence attacker navigation within
the honeynet environment?**

This question examines whether attackers interact with additional
systems beyond the initial honeypot when deeper deception layers are
present.

**RQ2 -- To what extent do deception artifacts influence attacker
decisions?**

This question investigates whether artifacts such as decoy credentials
or documents lead attackers to attempt credential reuse or access
additional hosts.

**RQ3 -- Does staged activation of deception layers increase attacker
engagement?**

This question evaluates whether progressive activation of deception
layers results in deeper exploration of the environment.

**RQ4 -- How does layered deception affect the time attackers spend
interacting with the system?**

This question examines whether deception mechanisms introduce measurable
delays in attacker progression.

**RQ5 -- What attacker activities can be observed across different
stages of the attack lifecycle?**

This question focuses on identifying behavioural patterns and mapping
observed actions to MITRE ATT&CK tactics.

## 3.2 Study Hypothesis

The central hypothesis of this study is that **a staged
Deception-in-Depth architecture will encourage attackers to interact
more extensively with deceptive assets and explore more layers of the
environment than a single-layer honeypot deployment**.

Specifically, the study expects that layered deception will:

- increase engagement depth across multiple hosts

- influence attacker navigation through deception artifacts

- introduce delays in attacker progression toward higher-value assets

These effects will be evaluated using the behavioural metrics defined in
this proposal.

# 4. System Architecture

The honeynet environment consists of three deception layers supported by
monitoring and virtualization infrastructure.

**Layer 1 -- Network Deception**

The first layer consists of network-accessible honeypot services
deployed using an open-source honeypot platform.

These services simulate commonly targeted protocols such as SSH, web
services, and file sharing.

This layer captures initial scanning activity and authentication
attempts.

**Layer 2 -- Host-Level Deception**

The second layer simulates an internal enterprise environment containing
several host systems.

Example hosts include:

- workstation systems

- file servers

- administrative servers

- a domain controller

These hosts contain decoy credentials, documents, and configuration
artifacts designed to encourage exploration and lateral movement
attempts.

**Layer 3 -- Data-Level Deception**

The third layer includes decoy data assets such as simulated cloud
credentials and references to data storage resources.

These artifacts represent higher-value targets that attackers may
attempt to access after interacting with earlier deception layers.

**Monitoring and Logging Layer**

A centralized monitoring node collects telemetry from honeypot services
and host systems.

The monitoring system aggregates logs and converts them into structured
records used for behavioural analysis.

**Virtualization Infrastructure**

The entire environment will be deployed on a **Proxmox virtualization
platform** using approximately four to six virtual machines representing
honeypots, enterprise hosts, and monitoring services.

Virtualization snapshots allow the environment to be reset between
experiments.

## 4.1 Telemetry Instrumentation

To improve behavioural analysis, the system includes lightweight
telemetry instrumentation that records interactions with deception
artifacts and reconstructs attacker activity sequences.

Deception artifacts embedded in the environment will include unique
identifiers that allow their usage to be tracked during analysis.
Examples include decoy credentials, internal documentation, and cloud
API keys.

When attackers interact with these artifacts, the telemetry system
records the associated identifier together with the event details.

This allows the study to trace how specific artifacts influence attacker
decisions.

## 4.2 Interaction Sequence Reconstruction

Collected telemetry will be normalized into structured event records
representing transitions between hosts and services.

Example interaction sequence:

Attacker → Honeypot service\
Honeypot → Internal workstation\
Workstation → File server\
Workstation → Domain controller

Representing interactions as structured sequences allows the system to
reconstruct attacker navigation paths through the environment.

## 4.3 Attacker Path Visualization

Interaction sequences may also be rendered as simple attacker path
diagrams to improve interpretability of observed behaviour.

These visualizations illustrate how attackers move between deception
layers and interact with decoy assets during a session.

Path visualizations will be used primarily as an analysis and reporting
aid rather than as a separate analytical subsystem.

## 4.4 Deception Control Plane

The architecture introduces a **Deception Control Plane** that
conditionally activates deeper deception layers based on observed
attacker behaviour.

Instead of exposing all decoys simultaneously, additional hosts and
artifacts may become visible only after specific trigger events occur.

Example trigger conditions include:

- successful honeypot login

- command execution on a compromised host

- interaction with a decoy credential

This staged activation simulates progressive discovery of infrastructure
during an attack and allows researchers to observe how attackers respond
to newly revealed resources.

# 5. Experimental Design

The honeynet environment will support several experimental scenarios
designed to evaluate the behavioural effects of layered deception.

**Scenario A -- Network deception only**

Only network honeypots are exposed.

Purpose: establish baseline attacker behaviour.

**Scenario B -- Network and host deception**

Internal hosts and decoy credentials are introduced.

Purpose: observe exploration and lateral movement attempts.

**Scenario C -- Full layered deception**

Data-level decoys such as cloud credentials are added.

Purpose: observe progression toward higher-value assets.

The experimental scenarios described in this section are designed to
evaluate the behavioural hypotheses outlined earlier in the proposal.

# 6. Behavioural Metrics

Three behavioural metrics are used to evaluate the influence of layered
deception on attacker activity.

**Misdirection**

Measures the number of interactions with deception artifacts such as
decoy credentials or documents.

**Delay**

Measures the time attackers spend navigating the environment before
reaching higher-value assets.

**Engagement Depth**

Represents the level of progression within the environment.

Example depth levels include:

  ----------------------------------
  **Level**   **Behaviour**
  ----------- ----------------------
  1           scanning activity

  2           authentication
              attempts

  3           command execution

  4           host exploration

  5           credential discovery

  6           lateral movement
              attempts

  7           interaction with
              data-level decoys
  ----------------------------------

## 6.1 Evaluation Method

The evaluation compares attacker behaviour across the experimental
scenarios described above.

Telemetry collected from the honeynet environment will be used to
reconstruct interaction sequences and derive the behavioural metrics
defined in this study.

The analysis examines whether layered deception results in observable
differences in misdirection, delay, and engagement depth compared to the
baseline scenario.

Selected sessions may also be visualized as attacker path diagrams to
illustrate typical interaction patterns.

The results will be interpreted descriptively, focusing on behavioural
patterns rather than statistical significance testing.

## 6.2 Threat Model

The honeynet environment is designed to observe the behaviour of
opportunistic attackers and security practitioners interacting with
exposed services.

The threat model assumes that attackers may perform activities such as:

- network scanning

- authentication attempts

- host exploration

- credential reuse

- access to data resources

The environment does not attempt to simulate highly targeted advanced
persistent threat campaigns. Instead, it focuses on capturing
behavioural patterns associated with attackers who interact with
accessible services and explore the environment after gaining initial
access.

# 7. MITRE ATT&CK Mapping

Observed activities will be mapped to high-level MITRE ATT&CK tactics.

Examples include:

  ---------------------------------
  **Behaviour**    **ATT&CK
                   Tactic**
  ---------------- ----------------
  network scanning Reconnaissance

  authentication   Credential
  attempts         Access

  command          Execution
  execution        

  host discovery   Discovery

  credential reuse Lateral Movement

  data access      Collection
  attempts         
  ---------------------------------

This mapping provides a structured framework for interpreting the
captured behaviour.

# 8. Feasibility and Risk Management

The project remains feasible due to several design choices:

- limited environment size (four to six virtual machines)

- use of established open-source honeypot platforms

- virtualization snapshots enabling rapid environment reset

- emphasis on behavioural observation rather than complex detection
  systems

These choices ensure the project can be completed within the capstone
timeframe.

# 9. Project Schedule and Learning Hours

  ---------------------------------
  **Milestone**        **Learning
                       Hours**
  -------------------- ------------
  Literature review    20

  Architecture design  15

  Infrastructure setup 15

  Network honeypot     20
  deployment           

  Host-level deception 25
  setup                

  Data-level deception 15
  setup                

  Telemetry            15
  integration          

  Experimental         20
  execution            

  Behaviour analysis   20

  Final report         15
  preparation          
  ---------------------------------

Total estimated learning hours: **180 hours**

# 10. Expected Deliverables

The project will produce:

- a layered deception honeynet architecture

- a working virtualized deployment

- documented experimental methodology

- behavioural analysis of observed attacker interactions

- attacker path visualizations illustrating interaction patterns

- MITRE ATT&CK mapping of observed activities

# 11. Expected Contributions

This project contributes:

- a **reproducible Deception-in-Depth honeynet architecture**

- a structured methodology for observing attacker behaviour across
  multiple deception layers

- behavioural insights into how attackers interact with deceptive
  environments

The study emphasizes controlled experimentation and behavioural
observation rather than large-scale operational deployment.

# Appendix A -- Implementation Plan 

## A.1 Purpose 

This appendix provides a practical implementation plan for deploying the
proposed Deception-in-Depth honeynet stack on the
available **Proxmox-based mini server with 32 GB RAM**. It translates
the proposal into a concrete execution layout, including virtual
machines, resource allocation, network segmentation, and deployment
sequence. 

The design intentionally prioritizes **manageability, reproducibility,
and experimental usefulness** over production-scale realism. 

 

## A.2 Deployment Platform 

The environment will be hosted on: 

- **Platform:** Proxmox VE 

<!-- -->

- **Available memory:** 32 GB RAM 

<!-- -->

- **Storage:** local SSD / available local storage on the mini server 

<!-- -->

- **Deployment model:** isolated virtual lab 

<!-- -->

- **Reset mechanism:** Proxmox snapshots and rollback 

Proxmox is suitable because it supports: 

- multiple virtual machines 

<!-- -->

- isolated virtual networks 

<!-- -->

- easy snapshotting and rollback 

<!-- -->

- efficient local experimentation 

This makes the environment appropriate for repeated behavioural
experiments. 

 

## A.3 Planned Virtual Machine Layout 

The proposed environment will use **5 core virtual machines**, with one
optional sixth VM if resources remain stable during testing. 

  -----------------------------------------------------------------------------------
  **VM Name**   **Role**       **Purpose**                **Suggested   **Suggested
                                                          RAM**         vCPU** 
  ------------- -------------- -------------------------- ------------- -------------
  VM1-LOG01     Monitoring /   central log collection,    4 GB          2 
                Logging Node   basic dashboards, timeline               
                               reconstruction                           

  VM2-TPOT01    Network        internet-facing network    8 GB          4 
                Honeypot Node  deception services                       

  VM3-DC01      Domain         Active Directory services  6 GB          2 
                Controller     and identity-layer                       
                               deception                                

  VM4-WS01      Employee       internal attacker          4 GB          2 
                Workstation    exploration target with                  
                               planted artifacts                        

  VM5-FS01      File /         shared folders, decoy      4 GB          2 
                Internal       documents, credential                    
                Server         artifacts                                

  VM6-ADM01     Admin / Jump / additional lateral         2--4 GB       2 
  (optional)    Decoy Server   movement target and                      
                               admin-themed artifacts                   
  -----------------------------------------------------------------------------------

**Estimated baseline RAM usage** 

Without optional VM6: 

- LOG01: 4 GB 

<!-- -->

- TPOT01: 8 GB 

<!-- -->

- DC01: 6 GB 

<!-- -->

- WS01: 4 GB 

<!-- -->

- FS01: 4 GB 

**Total: 26 GB** 

With optional VM6 at 2--4 GB: 

**Total: 28--30 GB** 

This leaves a small operating margin for Proxmox overhead and short-term
resource spikes. For stability, the recommended default deployment is
the **5-VM baseline**, with the sixth VM enabled only after validation. 

 

## A.4 Rationale for Feasibility 

Although the project includes multiple hosts, the
implementation remains manageable for several reasons. 

**Use of a small enterprise model** 

The environment is intentionally limited to a **small enterprise
simulation**, not a full corporate lab. This is sufficient
for observing: 

- initial access 

<!-- -->

- internal discovery 

<!-- -->

- credential interaction 

<!-- -->

- lateral movement attempts 

<!-- -->

- data-decoy interaction 

**Reuse of established tools** 

The plan relies on existing open-source components rather than custom
platform development. The main effort is integration and experimental
design. 

**Controlled scope** 

Each VM serves a clear experimental role. There is no requirement to
emulate every enterprise service. 

**Expandable but not dependent on expansion** 

The optional admin server improves realism, but the project does not
depend on it. The baseline 5-VM layout is already enough for the
proposed research questions. 

 

## A.5 Proposed Network Layout 

The environment should be segmented into **three main logical
networks**. 

  --------------------------------------------------------------------
  **Network**    **Purpose**              **Connected VMs** 
  -------------- ------------------------ ----------------------------
  NET-EXT        attacker-facing /        TPOT01 
                 external services        

  NET-INT        internal enterprise      DC01, WS01, FS01, optional
                 network                  ADM01 

  NET-MGMT       management and logging   LOG01, Proxmox management
                 network                  access 
  --------------------------------------------------------------------

**Network design principles** 

- **NET-EXT** exposes only the network deception layer. 

<!-- -->

- **NET-INT** simulates the internal enterprise environment. 

<!-- -->

- **NET-MGMT** is not exposed to the attacker and is used for
  administration, logging, and reset operations. 

This design supports safe containment and clearer behavioural analysis. 

 

## A.6 Logical Traffic Flow 

The environment is designed to support a staged attacker journey. 

**Stage 1 -- External interaction** 

Attackers first interact with services exposed on **TPOT01**. 

Examples: 

- SSH honeypot 

<!-- -->

- web honeypot 

<!-- -->

- selected service decoys 

**Stage 2 -- Internal progression** 

If the attacker discovers or is presented with internal artifacts, the
next likely targets are: 

- **WS01** for host exploration 

<!-- -->

- **FS01** for file shares and decoy documents 

<!-- -->

- **DC01** for identity-related discovery 

**Stage 3 -- Higher-value decoys** 

If internal artifacts are followed successfully, attackers may reach: 

- fake service credentials 

<!-- -->

- decoy internal references 

<!-- -->

- fake API keys or storage references 

This staged layout directly supports the proposal's measurement of: 

- misdirection 

<!-- -->

- delay 

<!-- -->

- engagement depth 

 

## A.7 Planned Role of Each VM 

**VM1 -- LOG01** 

This system acts as the central monitoring and analysis node. 

Planned functions: 

- collect logs from honeypots and hosts 

<!-- -->

- store authentication and access records 

<!-- -->

- support manual timeline reconstruction 

<!-- -->

- support ATT&CK-oriented analysis 

This VM is feasible because the project only requires **basic
centralized telemetry**, not a heavy enterprise SIEM deployment. 

 

**VM2 -- TPOT01** 

This is the entry-facing deception node. 

Planned functions: 

- host T-Pot CE services 

<!-- -->

- capture scans and login attempts 

<!-- -->

- log initial attacker interaction 

This is one of the more resource-intensive VMs, which is why it is
assigned the highest RAM allocation. 

Manageability is still reasonable because it consolidates multiple
network deception services into one host. 

 

**VM3 -- DC01** 

This system provides a minimal Active Directory environment. 

Planned functions: 

- domain services 

<!-- -->

- user and service account simulation 

<!-- -->

- selected deceptive AD objects or relationships 

This component may appear challenging, but it is manageable because the
plan uses **GOAD as the base**, reducing setup complexity
significantly. 

 

**VM4 -- WS01** 

This workstation supports host-level observation. 

Planned functions: 

- simulate a user endpoint 

<!-- -->

- contain planted files and decoy notes 

<!-- -->

- provide a believable exploration target 

This VM does not require heavy services, so moderate resources are
sufficient. 

 

**VM5 -- FS01** 

This system acts as the internal resource server. 

Planned functions: 

- host shared folders 

<!-- -->

- contain decoy documents 

<!-- -->

- expose planted credentials or references 

<!-- -->

- encourage attacker browsing and movement 

This VM is strategically important because it can naturally bridge host
deception and data-level deception. 

 

**VM6 -- ADM01 (optional)** 

This optional VM improves realism and supports additional lateral
movement paths. 

Possible functions: 

- admin-themed scripts 

<!-- -->

- backup-related decoys 

<!-- -->

- extra credential artifacts 

<!-- -->

- additional decision points for attacker path selection 

This VM should be deployed only if memory usage remains stable after
baseline testing. 

 

## A.8 Deception Asset Placement Plan 

To make the system experimentally useful, decoy assets should be
distributed across the environment. 

**On TPOT01** 

- exposed service banners 

<!-- -->

- realistic login prompts 

<!-- -->

- basic attacker-facing lure points 

**On WS01** 

- admin notes 

<!-- -->

- mapped drive references 

<!-- -->

- configuration files 

<!-- -->

- workstation artifacts suggesting internal systems 

**On FS01** 

- shared folders 

<!-- -->

- documents with fake credentials 

<!-- -->

- references to higher-value resources 

<!-- -->

- cloud-style credential artifacts 

**On DC01** 

- believable user accounts 

<!-- -->

- selected service accounts 

<!-- -->

- identity objects supporting enterprise realism 

**Data-level decoys** 

- fake API keys 

<!-- -->

- decoy bucket names 

<!-- -->

- monitored references to cloud resources 

To improve analysis quality, each major decoy should have a **unique
identifier** so that later it is possible to trace which artifact
influenced attacker behaviour. 

 

## A.9 Implementation Sequence 

The deployment should follow a staged sequence to reduce risk and
troubleshooting complexity. 

**Phase 1 -- Base infrastructure** 

- create Proxmox virtual networks 

<!-- -->

- deploy LOG01 

<!-- -->

- deploy TPOT01 

<!-- -->

- confirm isolated external-facing deception works 

**Phase 2 -- Internal enterprise setup** 

- deploy DC01 

<!-- -->

- deploy WS01 

<!-- -->

- deploy FS01 

<!-- -->

- verify internal connectivity and domain operation 

**Phase 3 -- Deception artifact placement** 

- add fake credentials 

<!-- -->

- add decoy files and internal references 

<!-- -->

- add selected admin-themed artifacts 

**Phase 4 -- Data-level decoys** 

- add fake API keys 

<!-- -->

- add cloud-style references 

<!-- -->

- configure monitoring for decoy credential usage 

**Phase 5 -- Logging and validation** 

- forward logs to LOG01 

<!-- -->

- verify event correlation across VMs 

<!-- -->

- test reconstruction of simple attacker paths 

**Phase 6 -- Experiment preparation** 

- snapshot the environment 

<!-- -->

- document baseline state 

<!-- -->

- run Scenario A / B / C experiments 

This phased approach makes the project manageable because each stage can
be validated before moving to the next. 

 

## A.10 Snapshot and Reset Strategy 

A key part of feasibility is the ability to restore the lab quickly. 

**Planned reset method** 

- take a clean snapshot after stable deployment 

<!-- -->

- take an experiment-ready snapshot after deception assets are placed 

<!-- -->

- restore to the experiment-ready state between runs 

This reduces the operational burden of rebuilding systems manually and
makes multiple experimental scenarios realistic within capstone time
constraints. 

 

## A.11 Key Manageability Considerations 

**Resource constraint** 

The most likely limitation is RAM pressure from running multiple VMs
simultaneously. 

**Mitigation:** \
Use the 5-VM baseline first, keep ADM01 optional, and avoid overloading
LOG01 with heavyweight tooling. 

**AD lab complexity** 

Domain-based environments can be difficult to build manually. 

**Mitigation:** \
Use GOAD as the base and limit modifications to deception-oriented
changes only. 

**Logging complexity** 

Centralized telemetry can become hard to manage if the scope is too
broad. 

**Mitigation:** \
Collect only the logs needed for the proposal metrics and ATT&CK-aligned
interpretation. 

**Experiment reproducibility** 

Without reset capability, repeated tests may become inconsistent. 

**Mitigation:** \
Use Proxmox snapshots as part of the core workflow. 

 

## A.12 Summary 

The proposed implementation plan is feasible on the available **32
GB Proxmox mini server** because it uses: 

- a limited number of VMs 

<!-- -->

- a small-enterprise simulation model 

<!-- -->

- established open-source frameworks 

<!-- -->

- controlled deception placement 

<!-- -->

- snapshot-based reset 

The design is sufficiently rich to support the project's research goals,
while still remaining realistic for a capstone-level implementation. 

# Appendix B -- Experimental Telemetry Schema

## B.1 Purpose

To support behavioural analysis of attacker activity within the honeynet
environment, the project introduces a structured telemetry schema for
recording and analysing observed interactions.

While the honeynet components generate raw logs from multiple sources
(such as honeypot services, host systems, and monitoring tools), these
logs can be difficult to interpret directly. The telemetry schema
defined in this appendix provides a normalized format that enables
consistent analysis of attacker behaviour across deception layers.

The schema supports several analysis objectives described in the
proposal, including:

- reconstruction of attacker interaction sequences

- identification of interactions with deception artifacts

- measurement of behavioural metrics such as misdirection and engagement
  depth

- generation of attacker path visualizations

The schema is designed to remain lightweight and feasible within the
scope of the capstone project while providing sufficient structure for
meaningful behavioural analysis.

## B.2 Logging Architecture Overview

Telemetry collected from the honeynet environment will be processed
through a simple normalization pipeline.

Raw logs generated by deception services and host systems will first be
collected by the centralized monitoring node. A lightweight parsing
script will then extract relevant information and convert it into
structured records following the telemetry schema.

The resulting datasets will be stored locally in formats suitable for
analysis, such as CSV files or a lightweight SQLite database.

The telemetry pipeline can be summarized as follows:

  ----------------------------------------------------------------------
  Raw Honeynet Logs\
  (T-Pot, host logs, control-plane events)\
  │\
  ▼\
  Log Normalization Script\
  │\
  ▼\
  Structured Telemetry Records\
  (session, events, artifacts, interaction edges)\
  │\
  ▼\
  Analysis and Visualization\
  (metrics, interaction graphs, attacker paths)
  ----------------------------------------------------------------------

  ----------------------------------------------------------------------

This approach allows the project to preserve original logs while
enabling structured behavioural analysis.

## B.3 Core Telemetry Datasets

The telemetry schema consists of several structured datasets that
capture different aspects of attacker interaction.

The primary datasets include:

1.  **Session Log** -- tracks individual attacker sessions

2.  **Event Log** -- records normalized security events

3.  **Artifact Interaction Log** -- records interactions with deception
    artifacts

4.  **Interaction Edge Log** -- records transitions between hosts and
    services

5.  **Control Plane Trigger Log** -- records activation of deception
    layers

Together, these datasets provide a comprehensive view of attacker
activity within the honeynet environment.

## B.4 Session Log

The session log tracks each observed attacker interaction session.

A session represents a sequence of related actions originating from the
same source actor or IP address. Grouping events into sessions allows
the system to measure session duration and reconstruct attacker activity
timelines.

Example fields include:

  ---------------------------------------------
  **Field**        **Description**
  ---------------- ----------------------------
  session_id       unique identifier for the
                   attacker session

  actor_id         identifier for the observed
                   attacker

  source_ip        originating IP address

  entry_point      first service contacted
                   (e.g., SSH honeypot)

  scenario_id      experimental scenario
                   identifier

  first_seen_ts    timestamp of first observed
                   event

  last_seen_ts     timestamp of last observed
                   event

  session_status   session state (active,
                   closed, timeout)
  ---------------------------------------------

Session data allows the project to evaluate metrics such as interaction
duration and engagement depth.

## B.5 Event Log

The event log records normalized security events derived from honeypot
logs and host telemetry.

Each record represents a single observed action, such as a login
attempt, command execution, or file access event.

Typical fields include:

  -----------------------------------------
  **Field**       **Description**
  --------------- -------------------------
  event_id        unique identifier for the
                  event

  session_id      linked attacker session

  timestamp       time of the event

  event_type      normalized event category

  source_node     host or actor initiating
                  the event

  target_node     system or service
                  targeted

  username        account name used, if
                  applicable

  artifact_id     identifier of related
                  deception artifact

  protocol        communication protocol

  action          concise description of
                  activity

  result          outcome of the action

  attack_tactic   mapped MITRE ATT&CK
                  tactic
  -----------------------------------------

Examples of normalized event types include:

- network scan

- login attempt

- login success

- command execution

- file access

- credential usage

- API key access

The event log forms the foundation for behavioural analysis and MITRE
ATT&CK mapping.

## B.6 Artifact Interaction Log

To improve interpretability of attacker decisions, deception artifacts
will include **unique identifiers** that allow their usage to be
tracked.

Examples of artifacts include:

- decoy credentials

- configuration files

- internal documentation

- cloud API keys

- references to internal systems

Example artifact identifiers:

  ---------------------------------------
  **Artifact   **Example Identifier**
  Type**       
  ------------ --------------------------
  credential   svc_backup_pw_A17

  document     finance_archive_note_B04

  API key      aws_key_C09

  host         fileserver_ref_D02
  reference    
  ---------------------------------------

When attackers interact with these artifacts, the system records an
artifact interaction event.

Typical fields include:

  -----------------------------------------------
  **Field**           **Description**
  ------------------- ---------------------------
  artifact_event_id   unique artifact event
                      identifier

  session_id          associated attacker session

  timestamp           event time

  artifact_id         identifier of the deception
                      artifact

  artifact_type       artifact category

  host                host where artifact
                      interaction occurred

  interaction_type    type of interaction
                      (opened, copied, used)

  resulting_target    next host or resource
                      accessed
  -----------------------------------------------

Artifact interaction logging allows the project to trace **causal
relationships between deception artifacts and attacker navigation
decisions**.

## B.7 Interaction Edge Log

The interaction edge log represents transitions between hosts or
services as structured relationships.

Instead of viewing attacker behaviour as isolated events, this dataset
represents the activity as a sequence of directed interactions.

Example fields include:

  -------------------------------------------
  **Field**          **Description**
  ------------------ ------------------------
  edge_id            unique edge identifier

  session_id         associated attacker
                     session

  timestamp          time of interaction

  source_node        source system or actor

  target_node        destination system or
                     service

  interaction_type   type of interaction

  artifact_id        linked artifact
                     identifier if applicable

  outcome            result of interaction

  depth_level        calculated engagement
                     depth
  -------------------------------------------

These edges can be used to reconstruct attacker navigation paths and
generate interaction graphs.

## B.8 Control Plane Trigger Log

Because the architecture includes a Deception Control Plane that
conditionally activates deeper deception layers, trigger events will
also be recorded.

Example fields include:

  ------------------------------------------
  **Field**           **Description**
  ------------------- ----------------------
  trigger_id          unique trigger
                      identifier

  timestamp           time of activation

  session_id          related attacker
                      session

  trigger_condition   event that caused
                      activation

  action_taken        action performed by
                      control plane

  affected_asset      host or resource
                      activated

  result              trigger execution
                      status
  ------------------------------------------

These records allow researchers to correlate attacker behaviour with
dynamic changes in the deception environment.

## B.9 Attacker Path Reconstruction

Using the interaction edge dataset, attacker activity can be
reconstructed as sequential paths through the honeynet environment.

Example interaction path:

  ----------------------------------------------------------------------
  Attacker\
  │\
  ▼\
  TPOT01\
  │\
  ▼\
  WS01\
  ↙ ↘\
  FS01 DC01
  ----------------------------------------------------------------------

  ----------------------------------------------------------------------

These reconstructed paths help illustrate how attackers navigate between
deception layers and interact with decoy assets.

In the final report, selected sessions may be visualized as **attacker
path diagrams** to demonstrate typical behavioural patterns observed
during experiments.

## B.10 Alignment with Behavioural Metrics

The telemetry schema supports the behavioural metrics defined in the
main proposal:

  --------------------------------------
  **Metric**      **Supporting Data**
  --------------- ----------------------
  Misdirection    artifact interaction
                  log

  Delay           session log and event
                  timestamps

  Engagement      interaction edge log
  Depth           

  Interaction     event and edge
  sequences       datasets
  --------------------------------------

This structured logging approach ensures that the experimental results
can be analysed systematically while remaining feasible within the scope
of the capstone project.

# Appendix C -- Telemetry Processing Pipeline

## C.1 Purpose

To support behavioural analysis within the deception-in-depth honeynet
environment, the project includes a lightweight telemetry processing
pipeline that converts raw logs generated by honeypot services and host
systems into structured datasets.

Honeypot frameworks and operating systems typically produce large
volumes of raw log entries that are difficult to interpret directly. The
telemetry pipeline addresses this limitation by extracting relevant
security events and converting them into normalized records following
the telemetry schema described in Appendix B.

This pipeline enables the project to reconstruct attacker interaction
sequences, identify interactions with deception artifacts, and generate
behavioural metrics used in the evaluation of the honeynet architecture.

The pipeline is intentionally designed to remain **lightweight and
manageable within the scope of the capstone project**, while still
providing sufficient structure for systematic behavioural analysis.

## C.2 Pipeline Architecture

The telemetry processing pipeline follows a simple sequential
architecture consisting of four main stages:

1.  **Log collection**

2.  **Event normalization**

3.  **Session and artifact enrichment**

4.  **Structured data export**

The processing flow is illustrated below.

  ----------------------------------------------------------------------
  Raw Honeynet Logs\
  (T-Pot, host systems, control plane events)\
  │\
  ▼\
  Log Parsers\
  (extract relevant security events)\
  │\
  ▼\
  Event Normalization\
  (convert logs into structured event records)\
  │\
  ▼\
  Session & Artifact Enrichment\
  (session assignment, artifact identification)\
  │\
  ▼\
  Interaction Edge Generation\
  (reconstruct attacker navigation paths)\
  │\
  ▼\
  Structured Telemetry Outputs\
  (session_log, event_log, artifact_log, edge_log)
  ----------------------------------------------------------------------

  ----------------------------------------------------------------------

This architecture allows raw logs to remain available for reference
while providing normalized datasets suitable for behavioural analysis.

## C.3 Log Sources

The telemetry pipeline processes logs generated from multiple components
within the honeynet environment.

Typical log sources include:

  ----------------------------------------------
  **Source**       **Example Data**
  ---------------- -----------------------------
  Network          login attempts, service
  honeypots        probes

  Host systems     authentication events,
                   command execution

  File shares      file access activity

  Deception        credential or document usage
  artifacts        

  Deception        activation triggers
  control plane    
  ----------------------------------------------

These logs are collected by the centralized monitoring node before being
processed by the normalization pipeline.

## C.4 Event Normalization

During normalization, raw log entries are converted into standardized
event records that share a consistent structure.

Each event record captures key information about the observed activity,
including:

- event timestamp

- event type

- source actor or system

- target host or resource

- username or credential used

- related artifact identifier (if applicable)

- outcome of the action

Normalization ensures that events originating from different log sources
can be analysed using the same schema.

This step simplifies subsequent analysis and allows behavioural patterns
to be compared across different experiment scenarios.

## C.5 Session Identification

After normalization, events are grouped into attacker sessions.

A session represents a sequence of related actions originating from the
same source actor or IP address within a defined time window.

Session grouping allows the project to:

- reconstruct attacker activity timelines

- measure session duration

- analyse engagement depth across the environment

A simple inactivity threshold is used to separate sessions when a source
actor becomes inactive for a defined period of time.

## C.6 Artifact Identification

As described in Appendix B, deception artifacts embedded in the
environment include unique identifiers.

Examples of artifacts include:

- decoy credentials

- internal documents

- configuration notes

- API keys

- references to internal hosts

When these identifiers appear within event records, the telemetry
pipeline associates the corresponding artifact identifier with the
event.

This allows the system to track how interactions with specific artifacts
influence attacker behaviour and navigation decisions.

## C.7 Interaction Edge Generation

To support reconstruction of attacker navigation paths, the telemetry
pipeline converts normalized events into **interaction edges**.

An interaction edge represents a transition between two entities within
the environment, such as:

- attacker → honeypot service

- compromised host → internal server

- workstation → domain controller

Each edge includes information describing the interaction type and the
depth of engagement within the environment.

These edges allow the project to model attacker activity as a sequence
of directed interactions rather than isolated log events.

## C.8 Structured Data Export

After processing, the telemetry pipeline exports structured records into
datasets corresponding to the schema described in Appendix B.

These datasets include:

  -----------------------------------------------
  **Dataset**            **Purpose**
  ---------------------- ------------------------
  session_log            attacker session
                         tracking

  event_log              normalized event records

  artifact_log           interactions with
                         deception artifacts

  interaction_edge_log   attacker navigation
                         paths

  control_plane_log      activation of deception
                         layers
  -----------------------------------------------

The datasets may be stored in simple formats such as CSV files or a
lightweight SQLite database to facilitate analysis and visualization.

## C.9 Analysis and Visualization

The structured telemetry outputs allow the project to perform several
forms of behavioural analysis, including:

- reconstruction of attacker activity timelines

- measurement of engagement depth

- identification of interactions with deception artifacts

- generation of attacker path visualizations

For example, an observed session may produce a path similar to the
following:

  ----------------------------------------------------------------------
  Attacker\
  │\
  ▼\
  TPOT01\
  │\
  ▼\
  WS01\
  ↙ ↘\
  FS01 DC01
  ----------------------------------------------------------------------

  ----------------------------------------------------------------------

Such visualizations help illustrate how attackers navigate between
deception layers and interact with decoy assets within the environment.

These reconstructed paths will be used in the final report to
demonstrate behavioural patterns observed during the experiments.

## C.10 Implementation Considerations

The telemetry pipeline will be implemented as a lightweight script
executed on the monitoring node.

The implementation will focus on:

- simple log parsing

- minimal dependencies

- straightforward data export

Because the pipeline relies primarily on log processing and basic data
transformation, the required implementation effort remains modest and
suitable for the scope of a capstone project.

This design ensures that the telemetry processing component enhances the
research value of the honeynet environment while remaining practical to
implement within the project timeframe.
