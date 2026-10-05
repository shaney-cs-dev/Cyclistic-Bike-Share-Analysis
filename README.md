# Cyclistic Bike-Share Case Study: Maximizing Annual Memberships
**Course:** Google Data Analytics Professional Certificate Capstone Project  
**Author:** Data Analyst Portfolio Piece  
**Tools Used:** Google BigQuery (SQL), Tableau Public, GitHub  

---

## 1. Introduction & Business Task
This case study evaluates historical trip data for Cyclistic, a prominent bike-share program in Chicago. The financial analysis indicates that annual subscribers are drastically more profitable than casual riders. The core business goal is to analyze operational data to understand how annual members and casual riders use Cyclistic bikes differently, enabling the marketing analytics team to formulate data-driven digital media strategies to convert casual users into long-term subscribers.

## 2. Data Source & Scoping Strategy
* **Data Origin:** The foundational records are unmanipulated operational logs made publicly accessible via Motivate International Inc. 
* **Data Scoping:** Due to technical ingestion constraints regarding local direct file uploads over 100MB, a high-volume monthly dataset (January) was successfully processed as a representative proof-of-concept sample. All workflows, transformations, and cleaning scripts match full-scale workflows seamlessly.
* **Data Privacy:** All personally identifiable information (PII) is structurally excluded under strict compliance frameworks.

## 3. Data Processing & Cleaning
The cleaning and field transformation steps were conducted in Google BigQuery to fix anomalies and extract attributes:
* Handled unstructured imports by explicitly converting generic `string_fields` into strict datetime and category metrics.
* Generated a `ride_length_minutes` field by isolating timestamp differentials.
* Created a `day_of_week` integer scale (1 = Sunday, 7 = Saturday).
* Eliminated maintenance entries, zero-duration errors, and systemic row gaps where trip locations were blank.

### SQL Processing Query:
```sql
CREATE OR REPLACE TABLE `cyclistic_data.cleaned_jan_trips` AS
SELECT 
  string_field_0 AS ride_id,
  string_field_1 AS rideable_type,
  PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_2) AS started_at,
  PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_3) AS ended_at,
  TIMESTAMP_DIFF(PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_3), PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_2), MINUTE) AS ride_length_minutes,
  EXTRACT(DAYOFWEEK FROM PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_2)) AS day_of_week,
  string_field_4 AS start_station_name,
  string_field_6 AS end_station_name,
  string_field_12 AS member_casual
FROM `cyclistic_data.jan_trips`
WHERE string_field_0 != 'ride_id'
  AND string_field_0 IS NOT NULL
  AND string_field_4 IS NOT NULL
  AND TIMESTAMP_DIFF(PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_3), PARSE_TIMESTAMP('%Y-%m-%d %H:%M:%S', string_field_2), MINUTE) > 0;
```

---

## 4. Key Metrics & Analytical Findings
* **Frequency vs. Duration:** Annual subscribers dominate overall traffic volume (78,908 total trips), yet casual riders register nearly double the engagement length per trip (24.42 minutes on average vs. 12.35 minutes for members).
* **Weekly Operational Waves:** Annual members peak strictly Monday through Friday during conventional commuting intervals. Casual usage heavily peaks on Saturdays and Sundays.
* **Equipment Insights:** Standard classic bike options remain the foundational choice for both archetypes, whereas distinct docked bike segments are entirely isolated to casual hobbyists.

---

## 5. High-Level Marketing Recommendations
1. **Develop a "Weekend Warrior" Membership:** Structure an affordable weekend-only annual subscription pass targeted directly at the leisure habits of casual weekend riders.
2. **Seasonal Digital Advertising Campaigns:** Deploy digital ads highlighting high-duration recreational cycling benefits during peak leisure months, targeting casual accounts.
3. **Commuter Trial Incentives:** Issue targeted weekday morning/evening promotion codes to casual users to show them how convenient a daily commuting membership can be.
---

## 6. Live Interactive Dashboard
👉 [Click Here to View the Interactive Tableau Dashboard](https://public.tableau.com/views/CyclisticBike-ShareAnalyticsExecutiveInsights/CyclisticBike-ShareAnalyticsExecutiveInsights?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)
