1. Load & Clean the Data
• Identify missing values, data types, and column structure
• Convert signup_date to datetime
• Standardise inconsistent text values (plan_selected, gender, etc.)
• Remove duplicate rows based on customer_id
• Handle missing values (e.g., region, email, age)

2. Data Quality Summary
• Count of missing values per column
• % of missing values
• Number of duplicates removed
• Mention inconsistent category values corrected (e.g., PRO → Pro)

3. Summary Outputs (Using Pandas Aggregations)
Use .groupby() or .value_counts() to summarise:
• Sign-ups per week (grouped by signup_date)
• Sign-ups by source, region, and plan_selected
• Marketing opt-in counts by gender
• Age summary: min, max, mean, median, null count

4. Answer These Business Questions
Answer the following using your analysis. Write clear, concise answers in your PDF report:
1. Which acquisition source brought in the most users last month?
2. Which region shows signs of missing or incomplete data?
3. Are older users more or less likely to opt in to marketing?
4. Which plan is most commonly selected, and by which age group?
5. (Optional) Which plan’s users are most likely to contact support?

5. Optional Stretch Task
• Load the support_tickets.csv dataset
• Join it to customer_signups.csv on customer_id
• Count how many customers contacted support within 2 weeks of sign-up
• Summarise support activity by plan and region (Group by plan and region)