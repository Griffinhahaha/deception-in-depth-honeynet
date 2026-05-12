# **Deception-in-Depth HoneyNet: Measurement Framework (Tentative)**

This handout defines how to measure **attacker misdirection, delay, and
engagement** in the HoneyNet stack. All metrics are session-based and
derived from observable logs. The environment consists entirely of
deceptive assets; therefore, misdirection is measured as deviation
within attacker interactions rather than interaction with real assets.

## **1. Misdirection** 

**Definition:** Degree to which attackers follow decoy-induced paths or
interact with explicitly deceptive elements instead of minimal-effort
paths.

**Metrics:**

### **1.1 Decoy Path Engagement Rate (DPER)**

DPER = Actions on Decoy Elements / Total Actions

**Examples of decoy elements:** fake AD users, honey API keys, decoy S3
buckets, non-functional admin shares

### **1.2 Path Expansion Factor (PEF)**

PEF = Observed Action Count / Minimal Path Action Count

**Example:** Minimal path requires 5 actions; attacker performed 20 →
PEF = 4.0

### **1.3 Dead-End Interaction Count (DIC)**

Number of actions on assets that cannot lead to meaningful progression
(e.g., repeated failed login attempts on fake credentials).

## **2. Delay**

**Definition:** Additional time attackers spend interacting with
deceptive assets.

**Metrics:**

- Session Duration: first log → last log

- Layer Dwell Time: time spent per layer

- Time-to-Abandonment: period of inactivity after last action

## **3. Engagement**

**Definition:** Depth and diversity of attacker actions.

**Metrics:**

- Action Count: number of logged events

- Distinct Techniques: number of different ATT&CK tactics/techniques
  observed

- Layer Depth: highest layer reached (1, 2, or 3)

## **4. Sample Metrics Table (Session-based)**

  ----------------------------------------------------------------------------------------------------------
  **Session   **Layers    **Duration   **Total     **Decoy     **DPER**   **PEF**   **Distinct     **Cloud
  ID**        Reached**   (min)**      Actions**   Actions**                        Techniques**   Pivot**
  ----------- ----------- ------------ ----------- ----------- ---------- --------- -------------- ---------
  S001        1 → 2       47           30          22          0.73       3.5       6              No

  S002        1           12           12          9           0.75       2.0       3              No

  S003        1 → 2 → 3   61           50          40          0.80       4.2       8              Yes
  ----------------------------------------------------------------------------------------------------------

**Usage:** Aggregate across sessions to compare the effect of layering,
rotation, or other experimental variables.

## **5. Methodology Rationale (for thesis chapter)**

Because the HoneyNet environment is entirely composed of deceptive
assets, traditional misdirection metrics comparing real vs. fake assets
are not applicable. Instead, misdirection is operationalized as
**attacker path deviation and interaction with decoy-induced elements**,
which reflects the behavioral impact of deception layers. Delay and
engagement metrics are measured based on observable timestamps, action
counts, and depth across layers. This approach ensures that findings are
**replicable, quantifiable, and academically defensible**, while
providing actionable insights into layered deception effectiveness.
