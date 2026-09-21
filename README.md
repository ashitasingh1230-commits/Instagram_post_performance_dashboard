# Instagram Post Performance Dashboard — Power BI Project

## Overview
This project analyzes Instagram post performance data (119 posts) to understand what drives reach, engagement, and follower growth. Built end-to-end in Power BI Desktop, it goes beyond basic charting into custom data modeling, Power Query cleaning, and advanced DAX calculations.

## Business Questions
- Which impression source (Home, Hashtags, Explore, Other) drives the most reach?
- What type of engagement (likes, comments, saves) do posts get most?
- Does higher reach actually lead to higher engagement?
- What percentage of profile visitors convert into followers?
- Which specific hashtags and posts perform best?

## What I Built
- **Data model**: Two tables at different levels of detail — a "Posts" table (one row per post, for post-level analysis) and a hashtag-exploded table (one row per post-hashtag combination, for hashtag-level analysis) — built by duplicating and selectively rolling back transformation steps in Power Query.
- **Data cleaning (Power Query)**: Split the multi-hashtag text field into individual rows, standardized casing and whitespace, and merged spelling/pluralization variants (e.g., "job" vs "jobs") using Replace Values.
- **DAX measures**:
  - `Total_Sales`-style aggregations for Impressions, Likes, Comments, Saves, Shares
  - `Follow Conversion Rate` — Follows ÷ Profile Visits (41%)
  - `Engagement Rate` — (Likes + Comments + Saves + Shares) ÷ Impressions (~6%)
  - `Performance Tier` — a calculated column using SWITCH/TRUE logic to classify each post as High/Medium/Low engagement
  - `Engagement Rank` — RANKX-based ranking of every post by engagement rate
  - `Correlation coefficient` — Pearson correlation (via Quick Measure) between Impressions and Likes (0.87)
- **Visuals**: Clustered bar charts for impression sources and engagement types, scatter charts (with Legend forcing one point per post) for Impressions vs. Likes and Hashtag-impressions vs. Likes, a Top 10 hashtags chart, KPI cards, and a conditionally-formatted Performance Tier table.

## Key Insights
- The Home feed is the dominant source of impressions, well ahead of Hashtags and Explore.
- Likes are by far the most common engagement type — expected, since they require the least effort from viewers.
- Impressions and Likes show a strong overall correlation (0.87), though the relationship becomes less predictable above ~10,000 impressions.
- 41% of profile visitors convert into followers.
- Hashtag-driven impressions specifically show no clear correlation with Likes — hashtags may aid discovery without reliably driving deeper engagement.
- The top-ranked post by engagement rate was an educational "Complete roadmap to data science" post, suggesting step-by-step educational content resonates most with this audience.
- Most posts (~57%) fall into the "Medium" performance tier, with a smaller share of standout high performers.

## Challenges & Debugging
- **Hashtag duplication distorting post-level analysis**: Splitting hashtags into individual rows (needed for hashtag-level analysis) duplicated each post's other metrics across multiple rows, which broke post-level calculations and charts. Resolved by duplicating the table and rolling back the split steps to create a dedicated one-row-per-post table.
- **Formula errors caught through testing**: An early Performance Tier formula used incorrect decimal thresholds (0.8 instead of 0.08), causing every post to fall into the same category — caught by checking the actual data range before trusting the output.
- **Recurring memory/capacity errors**: Hit a resource-governing error when running DAX calculations on an older, RAM-constrained machine. Diagnosed it wasn't a data-size issue (table was only 119 rows) by testing in a fresh file, then resolved it by increasing Windows virtual memory and restarting — confirming the same formulas worked correctly once resources were available.

## Tools Used
Power BI Desktop, Power Query, DAX (including custom SWITCH, RANKX, and time/statistical logic)

## Files
- 'instagramdashboard.pbix` — the full Power BI file
