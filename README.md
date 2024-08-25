# Marketing Campaign Performance Analysis

## Project Overview

This project focuses on analyzing and aggregating marketing campaign data from Facebook Ads and Google Ads. The objective is to combine data from multiple sources and generate a unified view of campaign performance metrics across both platforms. This analysis provides insights into campaign effectiveness, allowing for better decision-making in future marketing strategies.

## Data Sources

The analysis utilizes data from four different tables:

- **facebook_ads_basic_daily**: Contains daily performance metrics for Facebook Ads.
- **facebook_adset**: Details about the ad sets within Facebook campaigns.
- **facebook_campaign**: Contains information about individual Facebook campaigns.
- **google_ads_basic_daily**: Contains daily performance metrics for Google Ads.

## Objectives

The project was completed in three main steps:

### 1. Combining Facebook Ads Data

Using Common Table Expressions (CTEs), the first step was to merge data from `facebook_ads_basic_daily`, `facebook_adset`, and `facebook_campaign` tables. The resulting table includes the following columns:

- **ad_date**: The date the ad was displayed on Facebook.
- **campaign_name**: The name of the Facebook campaign.
- **adset_name**: The name of the Facebook ad set.
- **spend**: The amount spent on the campaign.
- **impressions**: The number of times the ad was shown.
- **reach**: The number of unique users who saw the ad.
- **clicks**: The number of clicks the ad received.
- **leads**: The number of leads generated.
- **value**: The total conversion value.

### 2. Merging Facebook and Google Ads Data

In the second step, another CTE was used to combine the Facebook data with the Google Ads data from the `google_ads_basic_daily` table. This step created a unified table with the following structure:

- **ad_date**: The date the ad was displayed.
- **media_source**: The platform where the ad was displayed (`Facebook Ads` or `Google Ads`).
- **campaign_name**: The name of the campaign.
- **adset_name**: The name of the ad set.
- **spend**: The amount spent on the campaign.
- **impressions**: The number of times the ad was shown.
- **clicks**: The number of clicks the ad received.
- **value**: The total conversion value.

### 3. Aggregating Data

Finally, the data was grouped by `ad_date`, `media_source`, `campaign_name`, and `adset_name` to calculate the aggregated metrics:

- **total_spend**: The total amount spent.
- **total_impressions**: The total number of impressions.
- **total_clicks**: The total number of clicks.
- **total_value**: The total conversion value.

The result is a comprehensive table that shows the performance of campaigns on both Facebook and Google Ads platforms.

## SQL Code

```sql
WITH facebook_data AS (
    SELECT
        fabd.ad_date,
        fc.campaign_name,
        fa.adset_name,
        fabd.spend,
        fabd.impressions,
        fabd.reach,
        fabd.clicks,
        fabd.leads,
        fabd.value
    FROM facebook_ads_basic_daily fabd
    JOIN facebook_adset fa ON fabd.adset_id = fa.adset_id
    JOIN facebook_campaign fc ON fabd.campaign_id = fc.campaign_id
),
combined_data AS (
    SELECT
        ad_date,
        'Facebook Ads' AS media_source,
        campaign_name,
        adset_name,
        spend,
        impressions,
        clicks,
        value
    FROM facebook_data
    UNION ALL
    SELECT
        ad_date,
        'Google Ads' AS media_source,
        campaign_name,
        adset_name,
        spend,
        impressions,
        clicks,
        value
    FROM google_ads_basic_daily
)
SELECT
    ad_date,
    media_source,
    campaign_name,
    adset_name,
    SUM(spend) AS total_spend,
    SUM(impressions) AS total_impressions,
    SUM(clicks) AS total_clicks,
    SUM(value) AS total_value
FROM combined_data
GROUP BY ad_date, media_source, campaign_name, adset_name
ORDER BY ad_date, media_source, campaign_name, adset_name;
```

## Key Insights

- The code provides a clear method to aggregate and compare marketing performance metrics across different platforms.
- Using CTEs (Common Table Expressions) allows for modular and readable SQL code, making the analysis easier to maintain and extend.
- The final aggregated data is crucial for understanding how different campaigns perform over time, across different ad platforms, and helps in optimizing marketing spend.

## Conclusion

This project demonstrates how to effectively use SQL to combine and analyze large datasets from multiple sources. By aggregating data and performing cross-platform comparisons, valuable insights can be gained that drive better marketing decisions.
