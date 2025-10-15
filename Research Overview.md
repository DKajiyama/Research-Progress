# **Background and Objective**

- In recent years, the risk of **social exclusion** has increased due to:
  - Decline of public transportation in rural areas
  - Demographic changes such as population aging

- Prior studies have shown that:
  - Lack of mobility restricts access to activities
  - This may exclude individuals from economic, social, and political life

- People’s physical mobility supports:
  - Face-to-face social interactions
  - Formation and maintenance of **social networks (SN)**

- In particular, weak ties (e.g., acquaintances, familiar strangers):
  - Often emerge from casual or incidental encounters in public spaces
  - Play an important role in social integration and access to information

---

## **Limitations of existing SN studies in transportation**

- Most empirical studies use **egocentric SN surveys**:
  - Respondents list close contacts using name generators

- **Two major limitations:**
  1. Weak ties are underreported
     - Respondents recall only salient/strong relationships
  2. Network structure cannot be observed
     - Egocentric surveys capture only ego–alter ties
     - Structural characteristics (density, centrality, clustering) are not observable

→
  then:
  - Weak ties and structural features **must be observed**
  - Local egocentric observations are not enough
  - Population- or regional-level inference is required

---

## **Objective of this study**

- To address these challenges, this study proposes the  
  Networked Social Ties and Co-Location (NEST-CL) Survey

- **Key idea:**
  - Observe weak ties and overall SN structures
  - Combine SN survey data with GPS trajectory data
  - Estimate unobserved ties beyond direct observations

- **Ultimate goal:**
  - Build a basis for estimating **tie strength** and **structural characteristics**  
    of social networks at the **population level**
 
# **Framework Overview**

- The NEST-CL (Networked Social Ties and Co-Location) Survey is a framework designed to
  - efficiently **observe weak ties and structural characteristics** of social networks (SN),
  - and **statistically infer unobserved ties** using GPS trajectory data.

- The framework consists of **three integrated components**:
  1. **Sampling Design**
  2. **Social Network Observation**
  3. **Tie Strength Inference Model**

---

## **1. Sampling Design**

- Uses a **two-stage sampling strategy**:
  - **Stage 1:** Select organizations (e.g., university, company, city hall) as primary sampling units (clusters).
  - **Stage 2:** Randomly sample individuals within each selected organization.

- Rationale:
  - Real-world social networks are **sparse**, and randomly sampling individuals across the entire population would result in most pairs being **Strangers**.
  - By sampling within organizations, the framework increases the likelihood of observing existing social ties (strong or weak).
  - The two-stage design allows the framework to strategically adjust the density of observed ties, capturing a balanced range of strong to weak ties.

- Participants are further divided into online survey sessions (20–30 people each) due to tool limitations and respondent burden.

---

## **2. Social Network Observation**

- Within each session, participants **recognize each other via videoconferencing** (e.g., Zoom, Teams).
- Each participant classifies the tie strength to every other participant in the same session using **five categories**:
  - **Strong ties**
  - **Relatively Strong ties**
  - **Relatively Weak ties**
  - **Weak ties**
  - **Stranger**

- Features:
  - Enables the observation of weak ties and unfamiliar relationships that are often missed by egocentric surveys.
  - Because participants visually recognize each other, the framework can also capture “familiar strangers.”
  - The resulting directed SN data serve as ground-truth labels for model training.

- Limitation:
  - Only ties within the same session are observed.
  - Ties across sessions or organizations are unobserved, creating an **incomplete induced subgraph**.
  - These missing ties will be inferred by the model in the third component.

---

## **3. Tie Strength Estimation Model**

- Participants install a **location-tracking smartphone app** to gather GPS trajectory data over a certain period.
- From GPS data, two behavioral indicators are extracted:
  - **Co-staying**: staying in close spatial proximity for a minimum duration.
  - **Co-moving**: moving together while maintaining spatial proximity.

- In addition to co-location features, the model includes:
  - **Spatial proximity** (distance between home and workplace)
  - **Homophily** (e.g., gender match, age group match)

- The tie strength \( y_{ij} \) between individuals \( i \) and \( j \) is estimated using a supervised learning model (Random Forest):
  \[
  y_{ij} = f(x_{ij}^{\text{Co-location}},\; x_{ij}^{\text{Spatial Proximity}},\; x_{ij}^{\text{Homophily}})
  \]

- The model is trained using:
  - Observed tie strengths from the online survey (within-session pairs)
  - Corresponding features derived from GPS, proximity, and demographics

- The trained model is applied to:
  - Across-session pairs (within the same organization)
  - Across-organization pairs

- This enables inference of unobserved ties, expanding the network beyond direct observation.

---

## **Overall Contribution**

- By integrating:
  - Strategic sampling (to capture diverse ties),
  - Direct observation (to obtain ground-truth SN structure), and
  - Behavior-based inference (to estimate missing ties),

- The NEST-CL framework provides a **basis for estimating the strength and structure of social networks at the population level, overcoming the limitations of traditional egocentric surveys.

## Case Study (Applied Example) — NEST-CL

### Study Context & Participants
- Deployed the NEST-CL survey across **three organizations in Hiroshima Prefecture**:
  - **Hiroshima University** (students): 151
  - **Fukken Co., Ltd.** (employees): 76
  - **Higashi-Hiroshima City Hall** (employees): 47
  - **Total:** 274 participants. :contentReference[oaicite:0]{index=0}
- **Observation method**:
  - Two-stage sampling (organizations → individuals), videoconference-based sessions,
  - Five tie-strength categories: **Strong**, **Relatively Strong**, **Relatively Weak**, **Weak**, **Stranger**. :contentReference[oaicite:1]{index=1}

### Behavioral Signals from GPS
- **Co-staying detection**:
  - Pairwise overlap of stay intervals with time threshold ≥ 30 s and distance threshold ≤ 30 m; proximity stays in open public spaces (not only within buildings) are counted. :contentReference[oaicite:2]{index=2} :contentReference[oaicite:3]{index=3}
- **Co-moving detection**:
  - Trajectories discretized at Δt = 30 s; if pairwise distance ≤ 30 m is maintained (allowing short gaps ≤ 3 min), segments ≥ 0.5 min are detected as co-moving. :contentReference[oaicite:4]{index=4}

### Model & Overall Performance
- **Supervised classification** of five tie classes using features from co-staying / co-moving, spatial proximity (home/work), and homophily.
- **Overall Accuracy:** **0.7553** (N = 1,823). :contentReference[oaicite:5]{index=5}
- **Per-class performance (Precision / Recall / F1 / Support):**
  - Strong: 0.7184 / 0.5873 / 0.6463 / 126  
  - Relatively Strong: 0.2909 / 0.2623 / 0.2759 / 122  
  - Relatively Weak: 0.4667 / 0.4733 / 0.4700 / 281  
  - Weak: 0.1316 / 0.0962 / 0.1111 / 52  
  - Stranger: 0.8803 / 0.9122 / 0.8960 / 1,242. :contentReference[oaicite:6]{index=6}
- **Key interpretation**:
  - High discrimination for Strong and Stranger, while intermediate ties are harder to separate due to overlapping behavioral patterns. :contentReference[oaicite:7]{index=7}

### Feature Importance (Top Signals)
- **Workplace distance** (0.160) and **Home distance** (0.117) rank highest → **spatial proximity** dominates.  
  Other top features include total/average co-location time, counts, time-of-day frequencies, unique locations, etc. :contentReference[oaicite:8]{index=8}
- Aggregate interpretation: **spatial proximity is the primary explanatory factor**, whereas co-staying/co-moving variables show **limited marginal contribution** relative to proximity. :contentReference[oaicite:9]{index=9}

### Cross-session Inference & Bias Note
- Applying the model **across online survey sessions** suggests:
  - Reduced shares of Strong / Relatively Strong ties in inferred distributions,
  - Indicating mitigation of self-selection bias inherent in session composition. :contentReference[oaicite:10]{index=10}
- Organization-specific context (e.g., lifestyles, residential patterns) appears to influence model behavior, underscoring the need for bias mitigation and generalizable modeling in future work. :contentReference[oaicite:11]{index=11}
