# Research-Progress

2025/10/25
## **Next Steps: Applying the Model to Secondary GPS Data**

As a next step, we plan to extend the NEST-CL framework by applying the trained tie strength inference model to **secondary GPS datasets (e.g., Ichinose-data)**. This allows us to evaluate the **generalizability** of the model beyond the original survey participants.

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

- **How to select valid organizations?**  
  - Based on number of individuals, type of building, or other thresholds

- **Should the same model be used for within-organization and across-organization inference?**  
  - It is possible that different mechanisms are needed
  - Comparison or model adaptation may be required

---

