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

To get a summary of key data points as stated by the stakeholder, we need to bunch up indidividual trip records from the previous SQL Code to create summarised dataset. The logic for this query is as follows:

**1. The Power of Aggregation (GROUP BY and COUNT)**

The most significant change is the addition of COUNT(TRI.bikeid) AS trip_count and the GROUP BY clause at the bottom.

Instead of one row per ride, it creates one row for every unique combination of user type, start/end neighborhood, and day.

It drastically reduces the size of your dataset, making your Tableau dashboard run much faster without losing any accuracy in your totals.

**2. Broadening the Timeline (2014–2015)**

The WHERE clause has changed from a single summer to a full two-year window: BETWEEN 2014 AND 2015.

Stakeholder Need: This fulfills the requirement to analyze "at least one year" to see seasonality (the peaks and valleys) and calculate Year-over-Year (YoY) growth.

The 5-Year Shift: By keeping the DATE_ADD(..., INTERVAL 5 YEAR), you are effectively presenting data as if it happened in 2019 and 2020, making the report feel current for the "Business Plan for next year".

**3. Precision Geospatial Mapping**

The INNER JOIN using ST_WITHIN and ST_GEOGPOINT remains the "brain" of the operation.

It takes the specific longitude/latitude "dots" from the bike trip and identifies which "polygon" (Zip Code shape) those dots fall into.

By joining this to your cyclistic.zip_codes table, you convert technical coordinates into human-readable neighborhoods like "Upper West Side" or "Chelsea".

**4. Categorizing Trip Length**

ROUND(CAST(TRI.tripduration / 60 AS INT64), -1)

This rounds trip lengths to the nearest 10 minutes (e.g., 8 minutes becomes 10, 24 minutes becomes 20).

It creates "buckets" of time, allowing to build a Bar Chart in Tableau that shows which neighborhoods attract long-duration rides versus quick commutes.

## Dashboard in Tableau.md

The final visualisation is clumped into 3 dashboards helped with a navigation bar to move through the dashboards. Further description of the dashboards:

**1. Summer Trends**

This dashboard provides a detailed analysis of bike-sharing usage in New York City (likely Citi Bike data), focusing on neighborhood-level performance and seasonal trends. It combines geographic mapping with granular data tables to compare different user groups.

The dashboard is organized into three primary sections:

(a) Neighborhood Activity Map (Top Left)
The large choropleth map visualizes trip intensity across various NYC neighborhoods.

Key Regions: The highest activity (darker blue) is concentrated in Manhattan (Gramercy Park, Murray Hill, Lower East Side) and North Brooklyn (Bushwick, Williamsburg, and Greenpoint).

Neighborhood Labels: Specific areas like the Upper West Side, Chelsea, and Northwest Brooklyn are highlighted to show the extent of the service area.

(b) Detailed Trip Breakdown (Top Right)
This table provides a numerical deep dive into the performance of each neighborhood, categorized by Usertype (Customer vs. Subscriber).

Top Performer: Bushwick and Williamsburg recorded the highest volume with a Grand Total of 9,206 trips.

User Dynamics: Subscribers (likely locals or commuters) account for the vast majority of trips compared to "Customers" (likely tourists or short-term users). For example, in Greenpoint, there are 4,277 Subscriber trips compared to 1,217 Customer trips.

Metrics: The table tracks both the Sum of Trips and Average Trip Minutes, though some figures in the table appear to be duplicated or represent trip counts in both columns.

(c) Summer Trends by Month (Bottom)
This section uses three small-scale "spark-maps" to show how bike usage changes throughout the summer season (July, August, and September).

Growth Trend: There is a visible increase in intensity (shifting to darker oranges/reds) as the season progresses.

September Peak: September appears to be the densest month, suggesting that ridership peaks in late summer/early autumn as temperatures become more comfortable for commuting.

2. Weather Patterns

This dashboard analyzes how environmental factors and temporal patterns influence bike-sharing ridership, specifically tracking trip counts against weather and time variables from 2019 to 2021.

(a) Precipitation Impact (Top Left)The first chart demonstrates a strong inverse relationship between rainfall and ridership. 

-The "Zero Rain" Spike: The vast majority of trips occur on days with 0.0 inches of precipitation.

-Sensitivity to Rain: There is a drastic drop-off in trip volume the moment any measurable precipitation is recorded (e.g., 0.1 to 0.5 inches). Users are highly sensitive to even light rain, which significantly deters ridership.

(b) Cumulative Growth: Running Sum (Top Right)This area chart tracks the total growth of the system over a two-year period.

-Growth Milestone: The cumulative "Sum of Trip Count" rises steadily from January 2019, surpassing 17.5 million total trips by early 2021.

-Pandemic Flatline: There is a noticeable "leveling off" of the slope in early 2020 (around March/April), which aligns with COVID-19 lockdowns, followed by a rapid recovery as bike-sharing became a preferred socially-distanced transport method.

(c) Monthly Seasonality (Bottom Left)The stacked bar chart highlights the cyclical nature of bike usage throughout the year.

-Peak Performance: September is the highest ridership month, exceeding 2 million trips.Winter Lows: February represents the annual low point, with trip counts dropping to approximately half of the peak summer volume.

-Seasonal Shift: Ridership begins to climb sharply in April and maintains high levels through October before the winter decline.

(d) Temperature Correlation (Bottom Right)This line chart shows how "Day Mean Temperature" drives user behavior.Positive Correlation: As the mean temperature increases, the number of trips increases.

-The "Sweet Spot": Usage peaks and becomes most volatile between 70(degree)F and 85(degree)F.

-Subscriber vs. Customer: Both user groups (represented by the two colored lines) show the same temperature-driven peaks, though Subscribers consistently maintain higher base volume.

**3. Top Trips**

This dashboard provides a detailed look at trip data (likely from a bike-sharing service like Citi Bike) across various neighborhoods in New York City, specifically Manhattan and Brooklyn. It focuses on three key metrics: Trip Minutes, Geographic Locations, and User Types.

Here is a breakdown of the visualization’s components and the insights they reveal:

(a) Trip Minutes by Starting Point & Destination 
These two stacked bar charts compare how many minutes were spent on trips based on where they began and where they ended.

Subscriber vs. Customer: The bars are color-coded. Orange represents "Subscribers" (frequent users), while Blue represents "Customers" (casual/one-time users). In almost every neighborhood, Subscribers account for the vast majority of the total trip minutes.

High-Traffic Areas: Zip codes like 10011 (Chelsea and Clinton) and 10003 (Lower East Side) show the highest volume of trip minutes.

Symmetry: The "Starting Point" and "Destination" charts look very similar, suggesting that people generally start and end their trips within the same high-density clusters.

(b) Trip Counts by Starting Neighborhood 

This is a "Heatmap" or "Highlight Table" that tracks the total number of trips across the first three months of the year.

Seasonal Trends: There is a very clear temporal pattern across all neighborhoods:

-January: Moderate activity.

-February: A significant dip in trip counts (likely due to colder weather or fewer days in the month).

-March: A massive surge in activity. For example, in zip code 10003, trips jumped from roughly 25,000 in February to over 45,000 in March.

Manhattan Dominance: The data shows that Manhattan (specifically Chelsea, the Lower East Side, and Greenwich Village) sees much higher trip volumes than the listed Brooklyn neighborhoods.
This dashboard shows a comparison between the top trips through the bike-sharing service based on starting point and destination point as well ass through starting neighborhood.

A link is provided that leads you to the workbook containing the dashboards.

