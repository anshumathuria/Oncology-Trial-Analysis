# Oncology-Trial-Analysis
An end-to-end data pipeline and cohort analysis of 1,000 oncology clinical trials.
# Oncology Clinical Trial Data Pipeline & Cohort Analysis

## Project Overview
This repository contains an end-to-end data engineering and analytics pipeline built to extract, clean, and analyze a dataset of 1,000 oncology clinical trials. The goal of this project is to transform messy, multi-valued clinical data into a relational format and surface historical R&D insights regarding trial success rates.

## Part 1: Data Engineering & Normalization (`Part_A.ipynb`)
Raw clinical trial datasets often contain wide, multi-valued strings (e.g., lists of drugs or indications crammed into a single cell). 
* **1NF Normalization:** Migrated multi-valued text fields into a normalized First Normal Form (1NF) Star Schema to allow for safe, granular cohort grouping.
* **Data Cleansing:** Dynamically mapped text fields (Phase and Status) by stripping whitespaces and standardizing capitalization, ensuring 100% data capture during downstream aggregations.

## Part 2: Success Logic & Cohort Analysis (`Part_B.ipynb`)
A critical component of this pipeline was accurately defining trial "Success" without falling victim to **Right-Censoring Bias** (prematurely evaluating ongoing trials). 

### Data Governance & Success Logic
Below is the logical architecture used to filter and score the raw dataset:

1. **Exclude Ongoing Trials:** ~380 trials marked as *Recruiting*, *Active*, or *Unknown* were explicitly removed from the denominator to prevent statistical bias.
2. **Operational Success Proxy:** For the remaining definitive historical cohort (620 trials):
   * `Completed` = Success (1)
   * `Terminated` / `Withdrawn` / `Suspended` = Failure (0)
3. **Small-Strata Suppression:** Cohorts with fewer than 2 trials were suppressed from visualizations to eliminate statistical noise (e.g., artificially inflated 100% success rates on rare diseases).

## Key Business Insights
* **Portfolio Baseline:** The dataset reveals a baseline historical operational completion rate of **~73%** across all definitive oncology trials. 
* **Strategic Indications:** By stratifying success rates by specific diseases (e.g., Breast Cancer vs. Melanoma) and mapping them against the global average, we can identify which oncological indications carry lower operational R&D risk.
* **Temporal Stability:** A 3-Year Moving Average reveals the trajectory of trial success rates over the last two decades, smoothing out yearly volatility to show the true long-term operational trend.
