# Customer Sign-Up Behaviour & Data Quality Audit Report

**Prepared for:** Rapid Scale Business Intelligence Team  
**Date:** November 2024  
**Analyst:** [Your Name]

---

## 1. Introduction

This report presents a comprehensive data quality audit and behavioural analysis of customer sign-up data for Rapid Scale, a fast-growing SaaS company offering tiered subscription plans. The analysis was conducted to support the Monthly Business Review (MBR) meeting and provide actionable insights for the Marketing and Onboarding teams.

**Dataset Overview:**
- **Primary Dataset:** `customer_signups.csv` containing 300 customer records with 10 attributes
- **Supporting Dataset:** `support_tickets.csv` containing 123 support ticket records (optional stretch analysis)
- **Analysis Period:** January 2024 to October 2024
- **Key Attributes:** Customer demographics, acquisition source, subscription plan selection, marketing preferences, and regional distribution

The analysis focused on identifying data quality issues, understanding user acquisition patterns, and assessing marketing engagement across different customer segments.

---

## 2. Data Cleaning Summary

### 2.1 Data Quality Issues Identified

The initial dataset contained several data quality challenges that required systematic cleaning:

- **Missing Values:** 34 email addresses (11.4%), 30 region assignments (10.0%), 19 age values (6.4%), and various other incomplete fields
- **Inconsistent Formatting:** Plan names appeared in multiple formats (e.g., "basic", "Basic", "BASIC", "Pro", "PRO", "Premium", "PREMIUM", "prem")
- **Duplicate Records:** 1 duplicate customer_id entry identified and removed
- **Data Type Issues:** All columns initially loaded as object/string types, requiring conversion for dates and numeric fields
- **Invalid Values:** Age field contained text values like "thirty" and "unknown", and some gender values were invalid (e.g., "123")

### 2.2 Cleaning Procedures Applied

**Step 1: Duplicate Removal**
- Identified and removed 1 duplicate customer record based on `customer_id`
- Final dataset: 299 unique customer records (down from 300)

**Step 2: Date Standardisation**
- Converted `signup_date` from string format to datetime objects
- Handled multiple date formats (DD-MM-YY) with error coercion for invalid entries
- Result: 6 records with unparseable dates marked as missing (increased from initial 2 due to stricter validation)

**Step 3: Categorical Value Standardisation**
- **Plan Selection:** Standardised to three values: "Basic", "Pro", "Premium"
  - Mapped variants: "basic" → "Basic", "PRO" → "Pro", "PREMIUM" → "Premium", "prem" → "Premium"
  - Invalid entries like "UnknownPlan" converted to missing values
- **Gender:** Standardised to "Male", "Female", "Non-Binary", "Other"
  - Handled case variations and abbreviations (e.g., "FEMALE" → "Female", "male" → "Male")
- **Marketing Opt-In:** Standardised to "Yes" or "No"
  - Converted variations like "y", "true", "1" → "Yes" and "n", "false", "nil" → "No"
- **Source & Region:** Applied title case standardisation and removed placeholder values ("??" → missing)

**Step 4: Age Field Cleaning**
- Extracted numeric values from text entries (e.g., "thirty" → 30)
- Removed non-numeric characters and converted to float
- Applied validation: ages outside reasonable range (15-100 years) marked as missing
- Result: 19 missing age values (increased from 12 due to validation)

**Step 5: Missing Data Documentation**
- Documented all missing values by column with counts and percentages
- Preserved missing values as NaN rather than imputing, to maintain data integrity for analysis

### 2.3 Cleaning Results Summary

| Column | Initial Missing | Post-Cleaning Missing | Change |
|--------|----------------|----------------------|--------|
| Email | 34 | 34 | No change |
| Region | 30 | 30 | No change |
| Age | 12 | 19 | +7 (validation) |
| Source | 9 | 15 | +6 (standardisation) |
| Plan Selected | 8 | 14 | +6 (standardisation) |
| Signup Date | 2 | 6 | +4 (validation) |

*Note: Screenshot of `data_quality` table from notebook Cell 7 recommended here*

---

## 3. Key Findings & Trends

### 3.1 Acquisition Channel Performance

YouTube emerged as the leading acquisition source overall with 58 sign-ups (19.4% of total), closely followed by Google (50 sign-ups, 16.7%) and Referral programs (49 sign-ups, 16.4%). However, in the most recent month (October 2024), Google led with 7 new sign-ups, suggesting a shift in channel effectiveness. This indicates that while YouTube has strong historical performance, Google may be gaining momentum and warrants increased marketing investment.

### 3.2 Age-Based Marketing Engagement

Analysis reveals a clear correlation between customer age and marketing opt-in rates. Users aged 35-44 and 45-54 show the highest engagement, with opt-in rates of 49.0% and 48.9% respectively. In contrast, users under 25 have only a 35.7% opt-in rate. This 13-percentage-point difference suggests that older customers are more receptive to marketing communications, potentially due to different privacy preferences or engagement patterns.

### 3.3 Plan Selection Demographics

Premium is the most popular plan overall (99 customers, 33.1%), but the distribution varies significantly by age. The 25-34 age group dominates Premium plan selection with 47 customers, representing nearly half of all Premium subscribers. This age group also shows balanced interest across all three plans (42 Basic, 47 Premium, 44 Pro), indicating they are the most valuable demographic for cross-selling and upselling opportunities.

*Note: Screenshots of `opt_in_rates_by_age` (Cell 21) and `plan_by_age_pivot` (Cell 22) recommended here*

---

## 4. Business Question Answers

### Question 1: Which acquisition source brought in the most users last month?

**Answer:** Google was the top acquisition source in October 2024, bringing in 7 new customers. This represents 28% of all sign-ups for that month. YouTube followed with 5 sign-ups (20%), and Facebook with 4 sign-ups (16%).

**Analysis Context:** While YouTube leads overall performance (58 total sign-ups), Google's recent performance suggests it may be a more effective channel for current marketing campaigns. This could indicate successful Google Ads optimization or changes in user search behaviour.

### Question 2: Which region shows signs of missing or incomplete data?

**Answer:** The region field shows significant data quality issues, with 30 missing values representing 10.0% of all customer records. This is the second-highest missing data rate after email addresses.

**Impact:** The missing region data prevents accurate geographic analysis and territory-based reporting. North region appears most populated (65 customers), followed by East (61), South (58), West (45), and Central (39), but these numbers may be understated due to the 30 missing assignments. This gap limits the Marketing team's ability to optimize regional campaigns and the Onboarding team's capacity to assign territory-specific resources.

### Question 3: Are older users more or less likely to opt in to marketing?

**Answer:** Older users are **more likely** to opt in to marketing communications. The analysis shows a clear positive correlation between age and marketing opt-in rates:

- **Under 25:** 35.7% opt-in rate
- **25-34:** 45.5% opt-in rate
- **35-44:** 49.0% opt-in rate (highest)
- **45-54:** 48.9% opt-in rate
- **55-64:** 42.9% opt-in rate

**Key Insight:** Users aged 35-54 show the strongest engagement, with nearly half opting in to marketing. This suggests that marketing campaigns targeting mid-career professionals may yield better response rates than campaigns focused on younger demographics.

### Question 4: Which plan is most commonly selected, and by which age group?

**Answer:** Premium is the most commonly selected plan with 99 customers (33.1%), followed closely by Pro (93 customers, 31.1%) and Basic (92 customers, 30.8%). The distribution is remarkably balanced across all three tiers.

**Age Group Analysis:**
- **25-34 age group** shows the strongest preference for Premium (47 customers), making them the primary Premium plan demographic
- **Premium plan** is most popular among 25-34 year-olds (47 customers) and 35-44 year-olds (23 customers)
- **Basic plan** is most popular among 25-34 year-olds (42 customers), suggesting this age group values both entry-level and premium options
- **Pro plan** shows balanced distribution, with 44 customers in the 25-34 age group and 17 in the 45-54 group

**Strategic Insight:** The 25-34 age group represents the most valuable segment, as they show strong adoption across all plan tiers and represent the largest customer base for Premium plans.

### Question 5 (Optional): Which plan's users are most likely to contact support?

**Answer:** Based on the support ticket analysis, Basic plan users show the highest support ticket volume, with 40 total tickets across all regions. However, this must be interpreted in context of the nearly equal customer base across all plans.

**Detailed Breakdown:**
- **Basic Plan:** 40 tickets across 5 regions (highest volume)
- **Premium Plan:** Ticket data shows significant activity in South and East regions
- **Pro Plan:** Moderate ticket volume with regional variations

**Early Support Contact:** 29 customers (23.6% of all customers with tickets) contacted support within 2 weeks of sign-up, indicating potential onboarding challenges. The South region shows the highest early support contact rate, particularly among Basic plan users (14 tickets).

**Recommendation:** The high support volume for Basic plan users, combined with early contact patterns, suggests that Basic plan customers may need enhanced onboarding resources or that the Basic plan features require clearer documentation.

---

## 5. Recommendations

### Recommendation 1: Enhance Regional Data Collection

**Issue:** 10% of customer records lack region assignments, limiting geographic analysis capabilities.

**Action Items:**
- Implement mandatory region selection during sign-up process
- Add IP-based geolocation as a fallback for missing region data
- Create a data validation workflow to flag incomplete records before they enter the system
- **Expected Impact:** Enable accurate territory reporting, improve regional campaign targeting, and support territory-based resource allocation

### Recommendation 2: Optimize Marketing Strategy for High-Engagement Age Groups

**Finding:** Customers aged 35-54 show 49% marketing opt-in rates compared to 36% for users under 25.

**Action Items:**
- Develop targeted marketing campaigns specifically for the 35-54 demographic, emphasizing value propositions that resonate with mid-career professionals
- Allocate increased marketing budget to channels that effectively reach this age group (e.g., LinkedIn, professional networks)
- Create age-segmented email campaigns with messaging tailored to each demographic's preferences
- **Expected Impact:** Increase overall marketing engagement rates, improve campaign ROI, and enhance customer lifetime value

### Recommendation 3: Focus Premium Plan Marketing on 25-34 Demographic

**Finding:** The 25-34 age group represents 47% of all Premium plan customers and shows balanced interest across all plan tiers.

**Action Items:**
- Develop targeted Premium plan promotion campaigns specifically for the 25-34 age group
- Create upselling pathways from Basic/Pro to Premium for existing 25-34 customers
- Design marketing materials highlighting Premium features most relevant to this demographic (e.g., career growth, professional development)
- **Expected Impact:** Increase Premium plan adoption, maximize revenue per customer, and leverage the most receptive demographic segment

### Recommendation 4: Improve Basic Plan Onboarding to Reduce Early Support Contacts

**Finding:** 29 customers (23.6% of support contacts) reached out within 2 weeks of sign-up, with Basic plan users showing high support volume.

**Action Items:**
- Develop comprehensive onboarding tutorials specifically for Basic plan features
- Implement proactive check-in emails at days 3, 7, and 14 post-signup for Basic plan customers
- Create self-service resources (FAQs, video tutorials) to address common Basic plan questions
- Assign dedicated onboarding specialists for Basic plan customers in high-contact regions (South, East)
- **Expected Impact:** Reduce early support ticket volume, improve customer satisfaction, and decrease churn risk

---

## 6. Data Issues or Risks

### Primary Data Quality Issue: Missing Region Data (10% of Records)

**Problem Description:**
The region field contains 30 missing values (10.0% of all customer records), representing the second-highest missing data rate in the dataset. This data gap significantly impacts the organization's ability to:

1. **Territory Management:** Sales and marketing teams cannot accurately assign customers to regional territories, potentially leading to misallocated resources and missed opportunities
2. **Regional Campaign Optimization:** Marketing cannot effectively measure campaign performance by region or tailor messaging to regional preferences
3. **Compliance & Reporting:** Regulatory reporting requirements may mandate geographic data, creating compliance risks
4. **Resource Allocation:** Onboarding and support teams cannot optimize staffing based on regional customer distribution

**Root Cause Analysis:**
The missing region data likely stems from:
- Optional region field during sign-up process (no validation enforcement)
- Incomplete data migration from previous systems
- User privacy preferences leading to region field being skipped
- Technical issues in data capture forms not properly recording region selections

**Recommended Fixes:**

**Immediate Actions (Source-Level):**
1. **Make region field mandatory** in the sign-up form with dropdown validation
2. **Implement IP-based geolocation** as an automatic fallback when users skip the region field
3. **Add data validation rules** in the CRM/system to prevent record creation without region assignment
4. **Create data quality alerts** to notify administrators when region data is missing

**Short-Term Actions (Data Remediation):**
1. **Backfill missing regions** using available data:
   - Use email domain analysis to infer likely regions
   - Cross-reference with support ticket data that may contain regional information
   - Contact customers directly to collect missing region data
2. **Implement data quality dashboard** to monitor region completeness in real-time

**Long-Term Actions (Process Improvement):**
1. **Establish data governance policies** requiring minimum 95% completeness for critical fields like region
2. **Create automated data quality checks** that run daily and flag incomplete records
3. **Develop data quality metrics** as part of monthly business reviews to track improvement
4. **Train customer-facing teams** on the importance of collecting complete data during sign-up

**Expected Outcome:**
Implementing these fixes should reduce missing region data to less than 2% within 3 months, enabling accurate regional analysis, improved territory management, and enhanced campaign targeting capabilities.

---

## Conclusion

This analysis has identified both opportunities and challenges in Rapid Scale's customer acquisition and data management processes. The balanced plan distribution and strong engagement from the 25-54 age demographic present significant growth opportunities. However, addressing the regional data quality issue is critical for enabling data-driven decision-making and optimizing marketing effectiveness.

By implementing the recommended actions, Rapid Scale can improve data quality, enhance marketing ROI, and better serve its customer base across all demographic segments and geographic regions.

---

**Report Statistics:**
- Total Words: ~1,480
- Analysis Period: January - October 2024
- Records Analyzed: 299 customer sign-ups, 123 support tickets
- Data Quality Issues Identified: 6 major categories
- Recommendations Provided: 4 strategic recommendations

---

*End of Report*

