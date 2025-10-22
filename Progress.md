# Research-Progress

2025/10/15
## **Next Steps: Applying the Model to Secondary GPS Data**

As a next step, we plan to extend the NEST-CL framework by applying the trained tie strength estimation model to **secondary GPS datasets (e.g., Ichinose-data)**. This allows us to evaluate the **generalizability** of the model beyond the original survey participants.

### **0. Defining Data**
- Target Period : 1 month (including many target users) 
- Target Users : staying in Hiroshima on daytime
  
### **1. Identifying User's Affiliation from GPS Data**
To apply the model, we first need to estimate each user’s affiliation using GPS data:
- Detect **workplace (employment location)** based on daytime stay patterns.
- Match the detected workplace with **building point data** (e.g., building name or land-use category).
- Assign individuals to organizations based on the building they frequently stay in.

### **2. Defining Target Affiliations**
We will select Affiliations that satisfy specific criteria, such as:
- Sufficient number of individuals (sample size threshold)
- Appropriate building-categories (e.g., office, public facility)

### **3. Model Application to External Affiliations**
- Use the model trained on survey-based tie strengths.
- Apply the model to individuals who were **not part of the original survey**.
- Objective: infer tie strengths in external Affiliations and validate model performance.


Only affiliations meeting these criteria will be used for inference.

### **4. Two Modes of Model Application**
Once organizations are identified, we will attempt:
1. **Within-organization inference**  
   - Apply the model to all pairs *within the same organization*  
   - Similar to session- or organization-level inference in the original study

2. **Across-organization inference**  
   - Apply the model to pairs *across different organizations*  
   - Test whether the model can capture inter-organizational social ties

### **Discussion Points**
- **How to determine organizational affiliation?**  
  - e.g., “Workplace (daytime stay) is in the same building”

- **How to select valid affiliations?**  
  - Based on number of individuals, type of building, or other thresholds

- **Should the same model be used for within-affiliation and across-affiliation inference?**  
  - It is possible that different mechanisms are needed
  - Comparison or model adaptation may be required

---
2025/10/22
## Data Processing Flow (To Ichinose User Data)

### 1. Handling Missing Values
- Extract only the records without missing values in the following three attributes:
  - Age  
  - Gender  
  - Office location (identified using Azis-Office-Identification)
- → Exclude users with missing values, keeping only those with complete attribute information.


### 2. Filtering by Office Location
- Select users whose workplaces are located within Hiroshima Prefecture.  
  - using administrative boundary data to identify the prefectural area.  


### 3. Setting the Target Period
- Target period: `October 1–31, 2023`
- Extract users who have GPS data observed within this period.


### 4. Filtering by Observation Days
- Select users with 14 days or more of GPS observations within the target period.  
  - `Observation_days >= 14`
- → Exclude users with less than 14 days of GPS data.


### Defining the Target Users for Analysis
- Users who meet all the above conditions are defined as target users for analysis.
- Filterd users : 1,779 

## **Discussion Points**
- It is difficult to determine Affiliation accurately because the office location data are sparsely distributed.
 What unit (boundary) should be used for co-location determination and SN generation?　
