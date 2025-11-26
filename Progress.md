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

---
2025/10/29
## Co-staying Detection
- Number of Co-staying: 4,365
- Number of unique pairs: 3,005
- Distribution of Building Categories
  
| summarized_category         | count |
|----------------------------|------:|
| Out of buildings           |  1638 |
| Office & Business          |   574 |
| Commercial Centers         |   563 |
| Other                      |   391 |
| Food & Dining              |   240 |
| Logistics & Infrastructure |   212 |
| Collective Housing         |   185 |
| Public & Medical           |   142 |
| Individual Housing         |   113 |
| Leisure & Hospitality      |    87 |
| Education & Learning       |    82 |
| Outside Hiroshima          |    78 |
| Retail                     |    44 |
| General Services           |    16 |

- Co-staying Counts by Building
  
| building_name                           | count | floors | summarized_category        |
|-------------------------------------------------|------:|-------:|----------------------------|
| イオンモール広島府中                            |   177 |    4.0 | Food & Dining              |
| 広島駅                                          |    92 |    2.0 | Logistics & Infrastructure |
| そごう広島店本館                                |    53 |   10.0 | Office & Business          |
| 西部高架下                                      |    44 |    3.0 | Office & Business          |
| ゆめタウン廿日市                                |    31 |    5.0 | Commercial Centers         |
| (株) ロジコム東広島（営）                       |    29 |    0.0 | Other                      |
| ひろしま駅ビルアッセ                            |    25 |    7.0 | Commercial Centers         |
| エリザベト音楽大学                              |    24 |    9.0 | Education & Learning       |
| ｅｋｉｅＤＩＮＩＮＧ・おみやげ館・エキエバル  |    24 |    2.0 | Office & Business          |
| 広島市役所行政棟                                |    23 |   16.0 | Public & Medical           |
| ＴＨＥＯＵＴＬＥＴＳＨＩＲＯＳＨＩＭＡ        |    23 |    2.0 | Commercial Centers         |
| 広島市民球場                                    |    22 |    0.0 | Leisure & Hospitality      |
| ＬＥＣＴ                                        |    22 |    4.0 | Commercial Centers         |
| イオンモール広島祇園                            |    21 |    3.0 | Commercial Centers         |
| イズミ新本社ビル                                |    21 |    6.0 | Commercial Centers         |
| 市立広島市民病院                                |    21 |   10.0 | Public & Medical           |
| 医療法人あかね会土谷総合病院                    |    20 |    9.0 | Public & Medical           |
| ｅｋｉｅＫＩＴＣＨＥＮ                          |    20 |    1.0 | Office & Business          |
| コストコホールセール広島倉庫店                  |    17 |    4.0 | Other                      |
| サンモール                                      |    17 |    6.0 | Commercial Centers         |


## Objective
- Introduce floor-based weighting using building floor count **F** (treat F=0　or missing as F=1) to suppress spurious co-stays in large or high-rise buildings.

## Features Changed (co-staying only)
- **Total Co-staying Count** → replaced with Σ(1/F).
- **Total Co-staying Duration** → replaced with Σ(duration/F).
- **Average Co-staying Duration** → replaced with Σ(duration/F) ÷ Σ(1/F).
- **Co-Staying Count by Duration Ranges** → based on duration/F.
- **Co-Staying Count by Location Category** → based Σ(1/F).
- **Co-Staying Counts by Time Band** *(Morning, Daytime, Evening, Night)* → based on Σ(1/F).

### Overall Metrics
| Metric        | Before | After  | Δ (After − Before) |
|---|---:|---:|---:|
| Accuracy      | 0.7553 | 0.7515 | −0.0038 |

### Discussion: Resolving Collinearity in Totals, Category Counts, and Time-Band Counts.

Is it okay to allocate the same amount to each floor? It would be better if each floor could be weighted according to its use.

20251104
## Future directions: 
### Analysis of the association between GPS-based SN characteristics and urban environment(urban transport and facility distribution) in Hiroshima

This study analyzes how transportation characteristics and the spatial distribution of commercial and public facilities influence social networks (SNs) generated from GPS data in cities within Hiroshima Prefecture.

### Target cities:
Hiroshima City
Fukuyama City
Higashihiroshima City
Kure City
(All have sufficient GPS data for SN generation)

| Office_City | count |
|---|---:|
| 中区 | 259 |
| 福山市 | 255 |
| 西区 | 154 |
| 南区 | 146 |
| 安佐南区 | 136 |
| 東広島市 | 124 |
| 呉市 | 116 |
| 安佐北区 | 67 |
| 尾道市 | 67 |
| 佐伯区 | 64 |
| 廿日市市 | 61 |
| 東区 | 61 |
| 三原市 | 45 |
| 安芸区 | 38 |
| 府中町 | 30 |
| 三次市 | 30 |
| 府中市 | 26 |
| 庄原市 | 13 |
| 大竹市 | 13 |
| 坂町 | 13 |
| 海田町 | 11 |
| 安芸高田市 | 7 |
| 熊野町 | 7 |
| 江田島市 | 7 |
| 大崎上島町 | 6 |
| 竹原市 | 6 |
| 世羅町 | 6 |
| 北広島町 | 5 |
| 神石高原町 | 4 |
| 安芸太田町 | 2 |

### Model direction:
An exploratory model in which social network (SN) characteristics of each city (under consideration) are used as dependent variables, and transportation, facility distribution, and geographical factors are used as explanatory variables.

### Explanatory variables (candidates):
(Based on the framework of Safira & Chikaraishi (2022))

- Transportation characteristics: mode share (car, train, walking, etc.), average travel time, average travel distance
- Facility distribution: number of commercial facilities, schools, and community centers, average distance between facilities
- Geographical and social attributes: population, population density, area size
(Data availability to be confirmed)

20251112

## Objective**
- Generate a social network (SN) using *MilesData(Hiroshima-2023-Oct)*.

## Issue Observed**
- Some pairs are classified as Strong ties or Relatively Strong ties even without any observed co-staying or co-moving events.

## Hypothesized Cause
- Continuous distance features (Office_distance, Home_distance) dominate the model, implicitly translating “closer distance = stronger tie.”

### Design Change (Feature Revision)**
- Replace continuous distances with binary proximity dummies that only fire when locations are clearly close.

### Options Considered
1. Building-based dummies using building point data (Voronoi partitioning, building matching).
2. Distance-based dummies (e.g., **within 100 m = 1**, else 0).

### Rationale
- Office/Home coordinates are cluster centroids, so they include positional noise.
- Building-based matching is sensitive to small offsets and may misclassify.
- Therefore, Option 2 (distance-based dummies) is preferable for robustness to small spatial shifts.

## Current Status
- Rebuilding the model using distance-threshold dummies for Office/Home proximity (e.g., 50–150 m bands tested; default 100 m).
- Goal: reduce spurious strong-tie assignments when co-location are absent.

20251119
## Feature Revision Based on Spatial Proximity

To better capture spatial relationships between user pairs, the conventional Home/Office distance (km) measures were replaced with the following binary indicators:

1. **Home Building Match Dummy (1/0)**  
   Set to 1 when both users’ Home coordinates fall within the same building polygon generated by the Weighted Voronoi Diagram.

2. **Office Building Match Dummy (1/0)**  
   Set to 1 when both users’ Office coordinates fall within the same building polygon generated by the Weighted Voronoi Diagram..

3. **Home 200m Proximity Dummy (1/0)**  
   Set to 1 when the distance between users’ Home locations is within 200 meters.

4. **Office 200m Proximity Dummy (1/0)**  
   Set to 1 when the distance between users’ Office locations is within 200 meters.

These revisions allow the model to explicitly incorporate  
- whether users share the same building (shared activity anchor),  
- whether users live or work in close proximity,  

representing spatial structure more clearly than simple continuous distance values.

### **Additively Weighted Voronoi Diagram (Power Diagram)**

In a standard Voronoi diagram, incorrect assignments (such as missing buildings) tend to occur.
To correct this, we incorporate **building floor area** as a weight, ensuring that large, influential buildings are more appropriately represented.

An *additively weighted Voronoi diagram* is a generalized form of the standard Voronoi diagram,  
where each site has an associated **weight**.  
The assignment is determined by the **power distance**:

The power distance from a point \(x\) to a site \(i\) is defined as:

$$
\pi_i(x) = \|x - p_i\|^2 - w_i^2
$$

A point \(x\) belongs to the region of site \(i\) if:

$$
\pi_i(x) \le \pi_j(x),\quad \forall j
$$

where:

- \(p_i\): position of site \(i\)  
- \(w_i\): weight of site \(i\)

Larger weights allow a site to dominate a wider area because the term \( -w_i^2 \) reduces its score.  

## Social network generated by the revised model (Miles data)
<img width="335" height="291" alt="image" src="https://github.com/user-attachments/assets/38bc1ba8-d870-476f-ba96-024bb9636784" />

## Future task: 
generate SNs for each city and summarize their features.

## Per-tie Feature Importance
========== Tie 1: 1 vs others ==========
                         feature  importance
* total_costay_duration_scaled    0.080561
* unique_locations_scaled    0.079872
* total_costay_count_scaled    0.070521
* average_costay_duration    0.069815
* costay_evening_scaled    0.066095
* Same_Age_Group    0.052111 *
* costay_daytime_scaled    0.045553
* costay_10_30_scaled    0.040528　
* costay_30_60_scaled    0.035589 * 
* costay_morning_scaled    0.034747

========== Tie 2: 1 vs others ==========
                         feature  importance
* Office_Close_200m    :0.082951
* total_costay_duration_scaled   : 0.081673
* average_costay_duration   :0.080360
* costay_evening_scaled    :0.070059
* costay_morning_scaled    :0.058216
* costay_daytime_scaled    :0.057206
* total_costay_count_scaled    :0.054777
* unique_locations_scaled    :0.049956
* Commercial Centers_scaled    :0.037996
* costay_10_30_scaled    :0.034423

========== Tie 3: 1 vs others ==========
                         feature  importance
* Office_Close_200m    :0.166972
* average_costay_duration    :0.076043
* costay_morning_scaled    :0.066082
* total_costay_duration_scaled    :0.062167
* Commercial Centers_scaled    :0.052955
* costay_daytime_scaled    :0.049090 
* total_costay_count_scaled   : 0.048196
* unique_locations_scaled    :0.047265
* costay_evening_scaled    :0.039545
* Education & Learning_scaled    :0.028375 * 

========== Tie 4: 1 vs others ==========
                         feature  importance
* average_costay_duration    :0.068472
* total_costay_duration_scaled    :0.062618
* total_costay_count_scaled    :0.059974
* Office_Close_200m    :0.057899 
* costay_evening_scaled    :0.053393
* costay_daytime_scaled    :0.052447
* unique_locations_scaled    :0.051978
* costay_morning_scaled    :0.044303
* costay_1_10_scaled    :0.044204 
* Commercial Centers_scaled    :0.042208

========== Tie 5: 1 vs others ==========
                         feature  importance
* Office_Close_200m    :0.119943
* total_costay_duration_scaled    :0.084839
*  average_costay_duration    :0.084512
* costay_morning_scaled    :0.069764
* costay_evening_scaled    :0.066421
* total_costay_count_scaled    :0.054634
* unique_locations_scaled    :0.052537
* costay_daytime_scaled    :0.048112
* Commercial Centers_scaled    :0.043156
* Out of buildings_scaled    :0.036592 *


### Summary
* Same_Age_Group and costay_30_60_scaled are important only for Strong ties, not for other tie categories.
* Office_Close_200m is a consistently important predictor for Relatively Strong → Stranger, and does not appear for Strong ties.
* Education & Learning_scaled is important only for Relatively Weak ties.
* Out of buildings_scaled is important only for Stranger ties.
* Important note: Feature importance indicates that a feature is useful for discrimination, but it does not tell whether the feature’s value is higher or lower for a given tie category.


