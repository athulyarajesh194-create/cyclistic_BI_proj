# Project Requirements Document: Cyclistic

**BI Analyst:** Athulya Rajesh

**Client/Sponsor:** Jamal Harris, Director, Customer Data

**Purpose:** Cyclistic’s Customer Growth Team is creating a business plan for next year. The team wants to understand how their customers are using their bikes; their top priority is identifying customer demand at different station locations. The dataset includes millions of rides, so the team wants a dashboard that summarizes key insights. Business plans that are driven by customer insights are more successful than plans driven by just internal staff observations. he executive summary must include key data points that are summarized and aggregated in order for the leadership team to get a clear vision of how customers are using Cyclistic.

**Key Dependencies:**

**Stakeholder Requirements:** The main summary of their requirements is to understand the customer demand of their service in different station locations with varioius factors contributing to that demand - eg. weather, time, distance etc. The relevant insights should include  (R - required, D - Desired, N - Nice to Have):

* A table or map visualization exploring starting and ending station locations, aggregated by location. I can use any location identifier, such as station, zip code, neighborhood, and/or borough. This should show the number of trips at starting locations. (R)

* A visualization showing which destination (ending) locations are popular based on the total trip minutes. (R)

* A visualization that focuses on trends from the summer of 2015. (D)

* A visualization showing the percent growth in the number of trips year over year. (R)

* Gather insights about congestion at stations. (N)

* Gather insights about the number of trips across all starting and ending locations. (R)

* Gather insights about peak usage by time of day, season, and the impact of weather. (R)

**Success Criteria:** To define the success criteria we will use the SMART framework:

*Specific:* The resulting BI insights should clearly demonstrate what makes a successful product and how the current usage of these bikes make informed desicions on future service development and what impacts the current demand of bikes in various station locations

*Measurable:*  Evaluate each trip on the number of rides per starting location and per day/month/year to understand trends. For example, do customers use Cyclistic less when it rains? Or does bikeshare demand stay consistent? Does this vary by location and user types (subscribers vs. nonsubscribers)? We will have these outcomes to find out more about what impacts customer demand.

*Action-Oriented:* The resulting insights hould prove or diprove that criteria such as time, location, season, weather etc. would impact user demand. This inforamtion would then be used by the Customer Data team to refine their service/product

*Relevance:* The merics will support the question at hand: How are we going to improve the Cyclistic experience

*Timeliness:* Analyze data that spans at least one year to see how seasonality affects usage. Exploring data that spans multiple months will capture peaks and valleys in usage.

**User Journey:** A deeper-dive into trip trends will help decision-makers explore how customers are currently using Cyclistic bikes and how that experience can be improved.

**Assumptions:** The dataset includes latitude and longitude of stations but does not identify more geographic aggregation details, such as zip code, neighborhood name, or borough. The team will provide a separate database with this data. 

The weather data provided does not include what time precipitation occurred; it’s possible that on some days, it precipitated during off-peak hours. However, for the purpose of this dashboard, I should assume any amount of precipitation that occurred on the day of the trip could have an impact.

Starting bike trips at a location will be impossible if there are no bikes available at a station, so we might need to consider other factors for demand.

**Compliance and Privacy:** the data must not include any personal info (name, email, phone, address). Personal info is not necessary for this project. Anonymize users to avoid bias and protect their privacy. 

**Accessibility:** The people that have access to the dashboard include:
Adhira, Brianne, Ernest, Jamal, Megan, Nina, Rick, Shareefah, Sara, Tessa

**Roll-out Plan:**

* Week 1: Dataset assigned. Initial design for fields and BikeIDs validated to fit the requirements.

* Weeks 2–3: SQL and ETL development

* Weeks 3–4: Finalize SQL. Dashboard design. 1st draft review with peers.

* Weeks 5–6: Dashboard development and testing


