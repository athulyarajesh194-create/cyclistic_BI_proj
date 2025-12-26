# Cyclistic Workplace Scenario

Cyclistic is a fictional bike-sharing company in New York City. It has partnered with the city of New York to provide shared bikes. Currently, there are bike stations located throughout Manhattan and neighboring boroughs. Customers are able to rent bikes for easy travel between stations at these locations.

This README would explain what each files contains and its summarised contents.

## Meeting Notes.md

These meeting notes serve as a Project Requirements Document that outlines the strategy for the Cyclistic data analysis project. Instead of just being a list of facts, they act as a roadmap for the data team to ensure their work aligns with what the executive leadership actually needs.

What is covered in these notes:
Business Context: It explains why the project is happening (to grow the customer base) and who the key decision-makers are.

Project Guardrails: It sets strict boundaries for the work, including a 6-week timeline, privacy requirements, and specific accessibility standards for the final dashboard.

Technical Roadmap: It identifies the different databases that must be joined (trips, geography, and weather) and the specific SQL/ETL steps needed to prepare them.

Success Criteria: It defines exactly what visualizations must be created and how "success" will be measured at the end of the 6 weeks.

Essentially, this document translates high-level business ideas into specific technical tasks so that there is no confusion between the analysts and the VPs.

## Stakeholder Requirements Document.md

It serves as the formal "blueprint" for a Business Intelligence (BI) project. It acts as a bridge between the business leadership's vision and the technical work required to build the dashboard.

Here is a breakdown of what this specific document establishes:

Strategic Alignment: It defines the Business Problem, ensuring that the final dashboard doesn't just show "cool data," but specifically addresses how to translate bike usage into customer growth and retention.

Roles and Responsibilities: It identifies the BI Professional (the person building the tool) and the Sponsors/Stakeholders (the people who will use the tool to make decisions, like Sara Romero and Jamal Harris).

User Intent: The "Stakeholder Usage Details" section explains how the tool will be used in the real world—specifically for planning new station locations and understanding the behavior differences between subscribers and non-subscribers.

Technical Scope (The "To-Do" List): The "Primary Requirements" section lists the exact visualizations and data insights that must be included, such as:

Geographic analysis (maps/tables by neighborhood or zip code).

Performance metrics (popular destinations by minutes and Year-over-Year growth).

External variables (the impact of summer trends and weather conditions).

Operational Health: By requesting insights on station congestion, the document ensures the BI tool will help manage the physical supply of bikes, not just the digital data.

In short, this document ensures that everyone is in agreement on what "success" looks like before any time is spent on development.

## Project Requirements Document.md

It is a more detailed version of the Stakeholder Requirements, specifically designed to guide the BI Analyst through the technical execution of the project while ensuring all organizational standards are met.

Here is an explanation of the specific components of this document:

1. Strategic Alignment & Purpose
Data-Driven Decision Making: It explicitly states that business plans based on customer insights are more successful than those based on internal observations.

Executive Vision: The purpose is to give leadership a "clear vision" of usage through summarized and aggregated data points.

2. Risk & Data Management (Key Dependencies)
Access Control: It identifies the gatekeepers (Jamal Harris) who must approve data requests.

Validation: It notes that product-specific teams must validate the interpretation of "bike trip duration" and "ID numbers" to prevent analytical errors.

3. Priority Framework (R/D/N)
Unlike a simple list, this document uses a Priority Ranking:

Required (R): Non-negotiable features like YoY growth and the impact of weather.

Desired (D): High-value but secondary, such as specific Summer trends.

Nice to Have (N): Features like "station congestion" that are added only if time permits.

4. The SMART Success Framework
This section turns vague goals into concrete technical benchmarks:

Specific: Focuses on what makes a "successful product".

Measurable: Commits to evaluating rides per location/day/month/year.

Action-Oriented: Aims to prove or disprove if factors like weather truly impact demand.

Relevant: Directly supports the question: "How are we going to improve the Cyclistic experience?"

Timely: Mandates at least one year of data to capture seasonal "peaks and valleys".

5. Technical Constraints & Ethics
Data Cleaning Assumptions: It warns the analyst that they will need to join a separate database to get Zip Codes/Boroughs since the raw data only has Lat/Long.

Environmental Assumptions: It establishes a "rule of thumb" for weather: if it rained at any point in a day, the analyst should treat the whole day as impacted.

Privacy & Compliance: It sets a hard rule on anonymization, forbidding the use of names, emails, or phone numbers to protect rider privacy.

6. The Project Lifecycle (Roll-out Plan)
It provides a 6-week timeline, moving from data validation (Week 1) to SQL/ETL (Weeks 2-3), and finally to dashboard testing (Weeks 5-6).

## Stratey Document.md

It acts as the final bridge between the business requirements and the actual build in Tableau. While the previous documents focused on what the business needs, this document defines how the dashboard will be technically structured.

Here is an explanation of the core components of this strategy:

1. Data Governance & Security
Source Identification: It specifies the primary dataset (NYC Citi Bike Trips) and the secondary dataset (Census Bureau US Boundaries) needed to map coordinates to neighborhoods.

Access Control: It establishes a "Read Only" permissions model, ensuring that while many stakeholders can view the data, they cannot accidentally edit or break the underlying logic.

2. Functional Scope
Field Inventory: It lists the specific fields to be included (Month, Year, Station, Zip Code, Weather, Neighborhood) to ensure the data team builds the correct "view" in the background.

Interactivity & Granularity: It outlines how users will navigate the data, specifically through Date/Month/Year filters and the ability to click on metrics to see deeper details.

3. The Visual Blueprint (Chart Specifications)
This is the most critical part for a developer, as it defines five specific charts required for the executive summary:

Trip Totals (Line Chart): Tracks volume over time.

Trip Counts by Neighborhood (Table): Provides a granular heat-map style view of location popularity.

Total Trip Minutes by Destination (Bar Chart): Focuses on where bikes are spending the most time, segmented by User Type.

Average Time to Arrive (Table): Analyzes efficiency and travel patterns across zip codes.

Seasonal Trends (Map): A complex geographic view correlating ride volume, duration, and weather patterns.

4. Accountability
Sign-off Matrix: It includes a formal section for the Proposer (Jamal Harris) to approve the status, moving the project from "Draft" to "Implemented".

How this differs from your other documents:
Meeting Notes: High-level conversation and "the story."

Project Planning Document (PPD): Logistics, timelines (6 weeks), and SMART goals.

Strategy Document: The technical recipe—exactly which chart types to use and which dimensions/metrics to drag onto each sheet.

## Target Table SQL Code - Seasonal Trends.md

This SQL Query follows ETL (Extract, Transfor, Load) concept. I am taking the raw data from multiple datasets and combining them to single clean target table for visualisation and analysis through Tableau to meet stakeholder requirements. This is what the code/query is doing in detail:

**1. The Core "Join" Logic (Connecting the Dots)**
The code combines four different types of data using Inner Joins:

Trip Data: The foundation is the NYC Citi Bike trips.

Geospatial Data: It uses a complex function called ST_WITHIN. It takes the Latitude and Longitude of the bike stations and checks which Zip Code Boundary they fall inside.

Weather Data: It joins with NOAA weather records by matching the date of the trip to the daily weather report from the Central Park station.

Neighborhood Mapping: It joins a custom table (cyclistic.zip_codes) to translate raw Zip Code numbers into readable names like "Borough" and "Neighborhood".

**2. Key Data Transformations**

The SELECT statement performs several calculations to prepare the data for your dashboard:

The "Time Shift": DATE_ADD(..., INTERVAL 5 YEAR) transforms 2015 data into 2020. This allows your "Business Plan for next year" to feel more current and relevant to stakeholders.

Unit Conversion: TRI.tripduration / 60 converts raw seconds into minutes.

Rounding: ROUND(..., -1) rounds the trip minutes to the nearest 10. This helps "clean" the data by grouping trip lengths into buckets (10 min, 20 min, etc.).

**3. Filtering for Strategy**

The WHERE clause ensures the dataset is manageable and focused on the specific requirements:

Weather Accuracy: It only uses weather data from the Central Park station (94728) to ensure consistency across all NYC trips.

Summer Focus: It filters specifically for July, August, and September 2015. This directly fulfills the stakeholder requirement to "focus on trends from the summer".
## Target Table SQL Code - Entire Year.md

## Dashboard in Tableau.md

## Executive Summary.md
