# Power-BI-UDF-Library

A curated collection of reusable **DAX measures, calculation patterns, and helper logic** for Power BI projects.

This repository is designed to centralize common analytical logic used across dashboards, helping reduce duplication, improve consistency, and speed up Power BI development.


## Purpose

In Power BI projects, the same DAX logic is often rewritten across different models and reports.
This library serves as a reusable reference for **common analytical patterns** that can be adapted and applied across projects.

The goal is to:
- Reduce repetitive DAX writing
- Improve readability and consistency
- Standardize KPI logic
- Support scalable and maintainable Power BI models


## What’s Included

### Common DAX Patterns
- Time intelligence (YTD, MTD, rolling periods)
- Color formatting
- Number formatting
- Percentage and variance measures 
- SVGs
- Context-aware calculations

### Best Practices
- Clear measure naming
- Performance-conscious patterns
- Readable and maintainable DAX


## How to Use

Each file contains:
- The DAX pattern (UDF)
- Explanation of the logic
- When and why to use it
- Notes on performance and context

You can:
- Copy UDF directly into your model
- Adapt patterns to fit your schema
- Use the repository as a reference during development

## Notes

- These patterns are **generic by design** — always adapt them to your data model.
- Performance may vary depending on relationships, cardinality, and model size.
- Always test measures in your own environment.

-----

## 👤 Maintainer

Maintained by **Sajjad Ahmadi**  
Power BI & Data Visualization Consultant

