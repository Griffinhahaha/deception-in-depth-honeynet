  -----------------------------------------------------------------------
  Project Title:     
  ------------------ ----------------------------------------------------
                     Design and Evaluation of a Deception-in-Depth
                     Honeynet Architecture for Observing Multi-Stage
                     Attacker Behaviour

  Mentor:            Ho Chen

  Student 1 (Leader) Yip Cheuk Yui

  Student 2          Sun Manqi

  Student 3          Ruan Chenyu

  Student 4          Chan Ching Laam

  Student 5          
  -----------------------------------------------------------------------

**Aim**

+----------------------------------------------------------------------+
| The aim of this project is to design, implement, and evaluate a      |
| layered Deception-in-Depth honeynet environment for observing        |
| attacker behaviour across multiple stages of interaction.            |
|                                                                      |
| The project investigates how deception mechanisms deployed across    |
| network, host, and data layers influence attacker decision-making,   |
| navigation, and engagement.                                          |
|                                                                      |
| A controlled virtual environment will be constructed to simulate a   |
| small enterprise system. The environment will incorporate a          |
| Deception Control Plane, enabling staged activation of deception     |
| layers based on attacker actions.                                    |
|                                                                      |
| The study evaluates attacker behaviour using three key metrics:      |
|                                                                      |
| **Behavioural Metrics**                                              |
|                                                                      |
| To evaluate attacker behaviour within the controlled honeynet        |
| environment, this study uses three behavioural observation metrics   |
| that capture how attackers interact with deception layers and        |
| assets.                                                              |
|                                                                      |
| These metrics do not measure defensive effectiveness, but rather     |
| describe how attackers respond to the deceptive environment.         |
|                                                                      |
| 1.  **Misdirection**                                                 |
|                                                                      |
| Misdirection refers to the extent to which attackers interact with   |
| deception artifacts within the environment.                          |
|                                                                      |
| This includes:                                                       |
|                                                                      |
| - use of decoy credentials                                           |
|                                                                      |
| - access to decoy files or resources                                 |
|                                                                      |
| - interaction with simulated high-value assets                       |
|                                                                      |
| Misdirection indicates whether the deception environment             |
| successfully influences attacker attention and decision-making.      |
|                                                                      |
| 2.  **Interaction Delay**                                            |
|                                                                      |
| Interaction delay measures the time attackers spend navigating the   |
| environment between key actions.                                     |
|                                                                      |
| Rather than representing defensive delay, this metric captures:      |
|                                                                      |
| - time between initial access and further exploration                |
|                                                                      |
| - time between artifact discovery and subsequent actions             |
|                                                                      |
| - time spent interacting with intermediate systems                   |
|                                                                      |
| This reflects how the deception environment affects the pace and     |
| progression of attacker activity.                                    |
|                                                                      |
| **Engagement Depth**                                                 |
|                                                                      |
| Engagement depth represents how far an attacker progresses within    |
| the layered environment.                                             |
|                                                                      |
| Example levels include:                                              |
|                                                                      |
| Level Behaviour                                                      |
|                                                                      |
| 1 initial scanning or probing                                        |
|                                                                      |
| 2 authentication attempts                                            |
|                                                                      |
| 3 successful access to a service                                     |
|                                                                      |
| 4 exploration of host systems                                        |
|                                                                      |
| 5 interaction with deception artifacts                               |
|                                                                      |
| 6 movement between hosts                                             |
|                                                                      |
| 7 interaction with data-level decoys                                 |
|                                                                      |
| Engagement depth captures the extent of multi-stage interaction      |
| within the environment, rather than success in compromising real     |
| systems.                                                             |
|                                                                      |
| The project aims to provide a structured methodology for behavioural |
| observation in cyber deception environments while maintaining a      |
| feasible implementation scope.                                       |
+======================================================================+

**Brief Literature Review**

+---------------------------------------------------------------------------------------------+
| Cyber deception has been widely studied as a proactive cybersecurity strategy aimed at      |
| influencing attacker behaviour rather than solely detecting attacks. Traditional honeypots  |
| typically simulate individual vulnerable services to attract attackers and collect data.    |
| However, these approaches are often limited in scope and may fail to capture attacker       |
| behaviour beyond initial interaction.                                                       |
|                                                                                             |
| Recent research highlights the importance of multi-layered deception architectures.         |
| Landsborough et al. (2024) propose a Deception-in-Depth strategy consisting of three        |
| logical layers: network, host, and data. This approach draws inspiration from military      |
| deception and defense-in-depth principles, aiming to influence attacker perception and      |
| behaviour across multiple stages of an attack.                                              |
|                                                                                             |
| A key insight from this work is that single-layer deception techniques may be insufficient, |
| as attackers can detect and bypass isolated honeypots. In contrast, combining multiple      |
| deception mechanisms increases complexity and may delay attacker progression (Landsborough  |
| et al., 2024).                                                                              |
|                                                                                             |
| The study also distinguishes between encouraging deception, which attracts attackers into   |
| controlled environments, and discouraging deception, which interferes with attacker         |
| activity within real systems. This distinction highlights the role of deception as a        |
| behaviour-influencing mechanism rather than purely a passive monitoring tool.               |
|                                                                                             |
| In addition to this work, prior research suggests that deception techniques can increase    |
| attacker effort and improve visibility into attacker behaviour (Gaddam, 2025). Furthermore, |
| research on honeynet systems highlights the importance of adaptive deployment and dynamic   |
| configuration in responding to attacker activity (Fraunholz et al., 2021).                  |
|                                                                                             |
| Despite these advances, there remains a gap in experimentally evaluating how layered        |
| deception influences attacker behaviour across multiple stages, particularly within         |
| controlled and reproducible environments.                                                   |
|                                                                                             |
| Landsborough, G., et al. (2024). *Deception-in-depth: A layered approach to cyber           |
| deception*. arXiv.                                                                          |
| [https://arxiv.org/abs/2412.16430](https://arxiv.org/abs/2412.16430?utm_source=chatgpt.com) |
|                                                                                             |
| Gaddam, N. (2025). *AI-enhanced honeypots for advanced cyber deception strategies*.         |
| International Journal of Cyber Security Research and Development, 5(1), 9--19.              |
| <https://doi.org/10.63374/QITP-IJCSRD_05_01_002>                                            |
|                                                                                             |
| Fraunholz, D., Zimmermann, M., & Schotten, H. D. (2021). *An adaptive honeypot              |
| configuration, deployment and maintenance strategy*. arXiv.                                 |
| <https://arxiv.org/abs/2111.03884>                                                          |
+=============================================================================================+

**Proposed Methodology**

+----------------------------------------------------------------------+
| The project adopts an experimental methodology based on constructing |
| a controlled honeynet environment and observing attacker             |
| interactions.                                                        |
|                                                                      |
| **1. System Design**                                                 |
|                                                                      |
| A layered honeynet architecture will be implemented consisting of:   |
|                                                                      |
| - Layer 1 -- Network Deception\                                      |
|   Network-accessible honeypot services capturing scanning and        |
|   authentication attempts                                            |
|                                                                      |
| - Layer 2 -- Host-Level Deception\                                   |
|   Simulated enterprise hosts (workstations, file servers, domain     |
|   controller) containing decoy artifacts                             |
|                                                                      |
| - Layer 3 -- Data-Level Deception\                                   |
|   Decoy credentials and cloud-style assets representing higher-value |
|   targets                                                            |
|                                                                      |
| The environment will be deployed using Proxmox virtualization with   |
| multiple virtual machines.                                           |
|                                                                      |
| **2. Deception Control Plane**                                       |
|                                                                      |
| A lightweight control mechanism will be implemented to enable staged |
| activation of deception layers.                                      |
|                                                                      |
| Deeper layers (e.g. internal hosts or data assets) will only be      |
| exposed when specific attacker actions are detected, such as:        |
|                                                                      |
| - successful login attempts                                          |
|                                                                      |
| - command execution                                                  |
|                                                                      |
| - interaction with decoy credentials                                 |
|                                                                      |
| This allows simulation of progressive infrastructure discovery.      |
|                                                                      |
| **3. Telemetry Collection and Processing**                           |
|                                                                      |
| Telemetry will be collected from honeypot services and host systems  |
| and processed into structured records.                               |
|                                                                      |
| The system will include:                                             |
|                                                                      |
| - artifact identity tagging to track interactions with decoy assets  |
|                                                                      |
| - interaction sequence reconstruction to model attacker movement     |
|                                                                      |
| - event normalization to standardize logs                            |
|                                                                      |
| This enables reconstruction of attacker behaviour across multiple    |
| stages.                                                              |
|                                                                      |
| **4. Behavioural Analysis**                                          |
|                                                                      |
| Attacker behaviour will be evaluated using three metrics:            |
|                                                                      |
| - Misdirection -- interactions with decoy artifacts                  |
|                                                                      |
| - Delay -- time spent before reaching higher-value assets            |
|                                                                      |
| - Engagement Depth -- progression through system layers              |
|                                                                      |
| Interaction sequences may also be visualized as attacker path        |
| diagrams.                                                            |
|                                                                      |
| **5. Experimental Scenarios**                                        |
|                                                                      |
| Three scenarios will be conducted:                                   |
|                                                                      |
| - Scenario A: Network-only deception (baseline)                      |
|                                                                      |
| - Scenario B: Network + host deception                               |
|                                                                      |
| - Scenario C: Full layered deception                                 |
|                                                                      |
| Behaviour across scenarios will be compared to evaluate the impact   |
| of layered deception.                                                |
|                                                                      |
| **6. Evaluation Approach**                                           |
|                                                                      |
| The evaluation focuses on comparative behavioural analysis rather    |
| than statistical inference.                                          |
|                                                                      |
| The study examines whether layered deception:                        |
|                                                                      |
| - increases interaction depth                                        |
|                                                                      |
| - influences attacker decisions                                      |
|                                                                      |
| - introduces delays in progression                                   |
+======================================================================+

**Milestones**

+---------------------------------------------+------------------+------------------+
| ***Tasks***                                 | ***Estimated     | ***Estimated     |
|                                             | completion       | number of***     |
|                                             | time***          |                  |
|                                             |                  | ***learning      |
|                                             |                  | hours***         |
+=================+===========================+==================+==================+
| 1               | Literature review         | Week 1--2        | 30               |
+-----------------+---------------------------+------------------+------------------+
| 2               | Architecture design       | Week 3           | 25               |
+-----------------+---------------------------+------------------+------------------+
| 3               | Proxmox environment setup | Week 4           | 25               |
+-----------------+---------------------------+------------------+------------------+
| 4               | Network honeypot          | Week 5           | 35               |
|                 | deployment                |                  |                  |
+-----------------+---------------------------+------------------+------------------+
| 5               | Host-level environment    | Week 6--7        | 45               |
|                 | setup                     |                  |                  |
+-----------------+---------------------------+------------------+------------------+
| 6               | Deception artifact        | Week 8           | 25               |
|                 | implementation            |                  |                  |
+-----------------+---------------------------+------------------+------------------+
| 7               | Telemetry pipeline        | Week 9           | 30               |
|                 | development               |                  |                  |
+-----------------+---------------------------+------------------+------------------+
| 8               | Experiment execution      | Week 10          | 30               |
+-----------------+---------------------------+------------------+------------------+
| 9               | Behavioural analysis      | Week 11          | 30               |
+-----------------+---------------------------+------------------+------------------+
| 10              | Final report preparation  | Week 12          | 25               |
+-----------------+---------------------------+------------------+------------------+
|                 |                           |                  | ***Total: 300*** |
+-----------------+---------------------------+------------------+------------------+

**Deliverables**

+--------------------------------------------------------------------------------------------------------+
| ***Items***                                                                                            |
+=====================================+==================================================================+
| 1                                   | A layered Deception-in-Depth honeynet architecture design        |
+-------------------------------------+------------------------------------------------------------------+
| 2                                   | A working virtualized honeynet deployment                        |
+-------------------------------------+------------------------------------------------------------------+
| 3                                   | A documented experimental methodology                            |
+-------------------------------------+------------------------------------------------------------------+
| 4                                   | Structured telemetry datasets of attacker interactions           |
+-------------------------------------+------------------------------------------------------------------+
| 5                                   | Behavioural analysis using defined metrics                       |
+-------------------------------------+------------------------------------------------------------------+
| 6                                   | Attacker path visualizations                                     |
+-------------------------------------+------------------------------------------------------------------+
| 7                                   | MITRE ATT&CK mapping of observed behaviour                       |
+-------------------------------------+------------------------------------------------------------------+
| 8                                   | Deployment and system configuration documentation                |
+-------------------------------------+------------------------------------------------------------------+
| 9                                   | Experimental results and analysis report                         |
+-------------------------------------+------------------------------------------------------------------+
| 10                                  | Final capstone report                                            |
+-------------------------------------+------------------------------------------------------------------+
