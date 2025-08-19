# 🏙️ SQL + Python Analysis of Berlin & Brandenburg Rental Market

This project analyzes Berlin and Brandenburg's housing market using **Zensus 2022 rental data**, exploring how rent prices vary by building age and district. By combining **SQL**, **Python**, and **machine learning (K-Means clustering)**, we extract actionable insights for renters, investors, and urban planners.

The goal is to transform raw public data into clean datasets, perform advanced analysis, and generate clear visualizations that highlight patterns in rent pricing and district-level market archetypes.

---

## 🗃️ Data Source

**Zensus 2022 (Germany Census) – Housing Module**  
- Publisher: **Statistisches Bundesamt (Destatis)**  
- File: `/data/raw/news-zensus-2022-miete.xlsx`  
- Coverage: District-level rental data for Berlin & Brandenburg.  
- Variables:  
  - Net cold rent (€/sqm)  
  - Year of construction (binned into categories: pre-1950, 1950–1969, …, post-2010)  
  - Geographic unit: District (Kreisebene)  

Source: [Zensus 2022 official portal](https://www.zensus2022.de)

---


## 📂 Repository Structure

- `Final_housing_BB.ipynb` → **Main analysis notebook** (SQL queries, clustering, visualizations).  
- `realestate_berlin_brandenburg.py` → Trial Python script for early ETL and queries.  
- `data/raw/news-zensus-2022-miete.xlsx` → Zensus 2022 rental dataset.  
- `requirements.txt` → Python dependencies.  

---

## ⚙️ Installation

Clone the repository and install dependencies:

- git clone https://github.com/Amirabs7/Berlin-Rental-Market-Clustering.git
- cd Berlin-Rental-Market-Clustering
- python -m venv venv
- source venv/bin/activate  # Windows: .\venv\Scripts\activate
- pip install -r requirements.txt

---

## ⚙️ Data Preparation 

Python, specifically the Pandas library, is the industry standard for data wrangling. Its powerful, flexible, and intuitive syntax is perfect for handling the messy, inconsistent formats often found in public data portals.

The raw data required significant transformation to be usable for analysis. The cleaning process involved:

- Handling Encoding: Converting files from Windows-1252 encoding to UTF-8 to properly display German umlauts (ä, ö, ü, ß).
- Reshaping Data: The rental data tables were in a "wide" format (e.g., construction periods as columns). We used pd.melt() to pivot them into a tidy "long" format, which is essential for effective analysis and visualization.
- Standardizing Values: Inconsistent naming (e.g., "Pankow" vs. "Berlin-Pankow") was standardized across all datasets.
-Type Conversion: Ensuring numerical columns (e.g., population counts, rent prices) were stored as correct data types, handling thousands separators (e.g., "2 345" -> 2345).

Handling Missing Data: Identifying and addressing NULL values, often represented by placeholder characters like . or - in the original data.

Data Storage & Management (SQL):

- Data Integrity: Enforcing relationships between tables (e.g., a dimension table for districts, a fact table for yearly statistics and rent prices) and ensuring data consistency.
- Efficient Querying: Performing complex joins, aggregations, and filters is faster and more memory-efficient than doing it in Pandas for large datasets. For example, joining rent data with population data to see if growth correlates with price.
- Reproducibility: SQL scripts provide a clear, version-controlled record of how the analysis was performed.




## 📈 Visualizations & Insights  

All figures are generated in the final analysis notebook [`Final_housing_BB.ipynb`](Final_housing_BB.ipynb).  
PNG files are in the repository root for direct GitHub display.  

### 1. Average Rent by Building Age (Berlin)  
![Average Rent by Age](average%20rent%20by%20building%20age%20in%20Berlin%20.png)  
➡️ **New apartments are almost 2× as expensive as pre-1950 units.**  
- The chart reveals a near-linear relationship between the year of construction and the average net cold rent. Apartments built post-2010 (€12.40/m²) are approximately 70% more expensive than those from the pre-1950 era (€7.80/m²). This premium is driven by:
- Higher Construction Standards: Modern energy efficiency (EnEV), sound insulation, and amenities.
- Market Demand: A strong preference for move-in-ready, low-maintenance housing.
- Cost-Plus Pricing: New developments must recoup high land, material, and regulatory compliance costs.

This trend holds true across the entire city, indicating that "newness" is a universally valued attribute, independent of location.
---

### 2. Rent Distribution by Construction Period  
![Rent Distribution](berlin%20rent%20levels%20by%20building%20age%20.png)  
➡️ **Altbau (pre-1950) shows the widest spread:** cheap in peripheral districts, premium in central areas.  
Location is the dominant factor for older buildings. Central Altbau apartments can rival modern units in price, while peripheral areas remain affordable. This highlights stark intra-city rent inequalities driven by urban desirability.  
This box plot provides a more nuanced story than the simple average:
- Pre-1950 (Altbau): Exhibits an extremely wide interquartile range. The price for a pre-war apartment is highly elastic and depends almost entirely on its district. A charlottenburg Altbau can cost €16.26/m², while a similar unit in Marzahn-Hellersdorf is only €7.04/m².
- 2010 and Later: While also showing significant spread, the distribution is tighter. New builds are expensive everywhere due to high construction costs, but they reach their absolute peak in the central, wealthy districts. This indicates that new supply does not automatically equalize prices across the city; the location premium remains powerful.




---

### 3. District Clusters (K-Means)  
![District Clusters](Rent%20trends%20by%20district%20cluster%20.png)  
➡️ Insight: The market is not a monolith but a mosaic of distinct sub-markets. 
- **Central & Premium**  
- **Up-and-Coming**  
- **Peripheral & Affordable**  
K-Means clustering uncovers patterns invisible to simple averages. High-demand central districts cluster together, emerging neighborhoods are distinct, and peripheral districts show lower but more uniform rents. This segmentation informs policy, investment, and urban planning.  
Applying K-Means clustering to the rent-by-age profiles of each district revealed 8 unique archetypes, moving beyond the simplistic "cheap vs. expensive" dichotomy:
- **Cluster 0 (Central & Wealthy)**: e.g., Mitte, Charlottenburg. High rents across all eras. Historic charm and modern luxury are both premium products.
- **Cluster 4 (Up-and-Coming)**: e.g., Neukölln, parts of Pankow. Characterized by a massive gap between moderately priced old stock and sharply rising prices for new builds. This signals intense gentrification and investment pressure.
- **Cluster 6 (Peripheral & Affordable)**: e.g., Marzahn-Hellersdorf, Spandau. The flattest profile. Offers the most affordable rents in Berlin, with even new construction remaining relatively accessible. This is the primary cluster for housing affordability.
This segmentation is powerful for targeted policy-making, identifying investment opportunities, and understanding the different dynamics at play in various parts of the city.


---

### 4. District Cluster Representatives  
![Cluster Representatives](berlin%20rent%20levels%20district%20cluster%20representatives.png)  
➡️ Insight: Concrete examples validate the clustering model and provide an intuitive lookup.
This visualization grounds the abstract clusters in reality by showing the actual rent curves of representative districts:

- **Charlottenburg-Wilmersdorf** (Cluster 0): The archetypal "high across the board" profile. Demand is location-based and insensitive to building age.
- **Neukölln** (Cluster 4): The "Kiez" effect. The price for a pre-1950 unit is still below the city average, but the cost of a new build has skyrocketed, pulled up by the area's popularity.
- **Marzahn-Hellersdorf** (Cluster 6): The affordability anchor. The entire market operates at a lower price point, providing crucial less-expensive housing stock for the city.
  
This chart allows users to quickly find a district's cluster and understand its relative position in Berlin's complex rental ecosystem.



---

## 🔑 Key Findings

- **The Dual Drivers of Rent:** Berlin's rental prices are determined by two primary, interacting factors: Location (the dominant force) and Building Age (a significant modifier). The influence of each varies dramatically by district.
- **The Altbau Divide:** The pre-1950 housing stock is not a single market. It is deeply fractured along geographic lines, representing both the most exclusive and most affordable segments of the market.
- **New Builds are not an Equalizer:** While new construction is universally more expensive, it amplifies rather than diminishes existing geographic inequalities. The most expensive new builds are in the already-expensive core.  
- **Data-Driven District Typology:** The 8-cluster model provides a sophisticated, empirical framework for categorizing neighborhoods based on economic behavior rather than tradition or perception, revealing patterns like "transitional" districts that are critical for understanding urban development.


---



## 👩‍💻 Author
**Amira Ben Salem**  
📫 Email: besamira77@gmail.com  
📍 Berlin, Germany  

