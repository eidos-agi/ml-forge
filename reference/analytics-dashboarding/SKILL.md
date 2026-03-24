---
name: analytics-dashboarding
description: Build live analytics dashboards connecting to databases (Snowflake, PostgreSQL, Azure SQL) using Power BI, Tableau, Apache ECharts, or Streamlit. Covers DAX formulas, Power Query transforms, and interactive web-based visualization. Use when building executive dashboards, portfolio monitoring, client reporting, or operational analytics.
---
# Business Analytics Dashboarding
## When to Use This Skill
Use when building any visualization layer: executive dashboards, client-facing analytics, portfolio monitoring, or operational reporting.

## Instructions
1. **Data Source Connection** — Connect to Snowflake/PostgreSQL/Azure SQL. Use read-only credentials. Prefer views over raw tables.
2. **Power BI Path** — Import data via Power Query. Transform: rename columns, set types, create calculated columns. Build measures with DAX (SUM, AVERAGE, CALCULATE, FILTER, time intelligence). Design pages: Overview → Detail → Drill-through.
3. **Tableau Path** — Connect data source. Build worksheets with drag-drop. Create calculated fields. Assemble dashboard with filters, actions, tooltips. Publish to Tableau Server/Online.
4. **Web Dashboard Path (ECharts/Streamlit)** — For custom web apps: use Apache ECharts (JavaScript) or Streamlit (Python). Connect to DB via SQLAlchemy. Render charts with st.plotly_chart or echarts options.
5. **Design Principles** — One key metric per card. Trend lines > point-in-time numbers. Filter from top (date range, entity) down. Color: green=good, red=bad, gray=neutral. Mobile-responsive layout.
6. **Refresh Strategy** — Scheduled refresh for Power BI/Tableau (hourly or daily). Real-time for Streamlit (query on page load). Cache expensive queries.
