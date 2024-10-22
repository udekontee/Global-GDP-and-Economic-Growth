# Global GDP and Economic Growth Analysis (2015-2024)
This project analyzes and compares leading economies' GDP and average growth rates from 2019 to 2024. It includes data visualizations that show the economic contributions of top global economies, highlighting the overall GDP growth trends and the percentage contribution of each country to the global economy. The insights from this analysis can help inform broader discussions on economic development and potential impacts on sustainable development goals.

# Project Overview
This project conducts a thorough analysis and visualization of global GDP trends and growth rates from 2015 to 2024, leveraging data from diverse countries. It aims to uncover critical economic patterns and insights that can guide informed decision-making in economic policy and strategic investments.

## Tools Used
- MySQL: For data storage and initial cleaning.
- R: For data analysis and visualization using packages such as ggplot2, dplyr, janitor, and skimr.
- Excel: For additional data manipulation and visualization.
- Tableau: For creating interactive dashboards and visualizations.

## Data Mapping, Cleaning, visualizing and Analysis using R
This R code conducts a thorough analysis of GDP data, identifying the top 10 countries by total GDP and average growth rate for 2023-2024. It utilizes data manipulation with dplyr to calculate percentages and create visualizations, including bar and pie charts using ggplot2, effectively illustrating economic trends and performance across countries.

# Description of R Code

## Top 10 countries by total GDP
top_10_gdp <- cleaned_gdp_data1 %>%
  arrange(desc(total)) %>%
  slice(1:10)  # Get the top 10
  
## Top 10 countries by growth rate (2023-2024) with specific columns
top_10_growth <- cleaned_gdp_data1 %>%
  arrange(desc(avg_growth_rate_2023_2024)) %>%
  slice(1:10) %>%
  select(country, avg_growth_rate_2023_2024)

## Bar chart for top 10 GDP
ggplot(top_10_gdp, aes(x = reorder(country, -total), y = total)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Horizontal bar chart
  labs(title = "Top 10 Countries by Total GDP", x = "Country", y = "Total GDP")

  ![image](https://github.com/user-attachments/assets/c3215f76-08a1-4977-89da-5a9e8f200905)

# Line chart for top 10 countries by growth rate
ggplot(top_10_growth, aes(x = reorder(country, -avg_growth_rate_2023_2024), y = avg_growth_rate_2023_2024, group = 1)) +
  geom_line() +
  geom_point() +
  labs(title = "Top 10 Countries by Growth Rate (2023-2024)", x = "Country", y = "Growth Rate (%)")

## Calculate the percentage of total GDP
top_10_gdp <- top_10_gdp %>%
  mutate(percentage = total / sum(total) * 100)

## Donut chart with percentages
top_10_gdp %>%
  ggplot(aes(x = 2, y = percentage, fill = country)) +
  geom_bar(width = 1, stat = "identity") +
  coord_polar(theta = "y") +
  xlim(0.5, 2.5) +  # Creates the donut hole
  labs(title = "Top 10 Countries by Total GDP (Percentage)", x = NULL, y = NULL) +
  geom_text(aes(label = paste0(round(percentage, 1), "%")), 
            position = position_stack(vjust = 0.5), 
            size = 4) +  # Adjust text size for clarity
  theme_void()  # Removes background and axis for a cleaner look

ggplot(data = cleaned_gdp_data1, aes(x = country, y = total, fill = region)) +
  geom_bar(stat = "identity")

ggplot(data = cleaned_gdp_data1, aes(x = country, y = total, fill = country)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Horizontal bar chart for better readability
  labs(title = "Total GDP by Country")

str(cleaned_gdp_data1)

ggplot(data = cleaned_gdp_data1, aes(x = reorder(country, -total), y = total, fill = country)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Horizontal bar chart for better readability
  labs(title = "Total GDP by Country", x = "Country", y = "Total GDP")

## Ensure the total is numeric
cleaned_gdp_data1$total <- as.numeric(cleaned_gdp_data1$total)

## Ensure country is a factor
cleaned_gdp_data1$country <- as.factor(cleaned_gdp_data1$country)

ggplot(data = cleaned_gdp_data1, aes(x = reorder(country, -total), y = total, fill = country)) +
  geom_bar(stat = "identity") +
  coord_flip() +  # Horizontal bar chart for better readability
  labs(title = "Total GDP by Country", x = "Country", y = "Total GDP")

## Calculate the percentage of growth rates
top_10_growth <- top_10_growth %>%
  mutate(percentage = avg_growth_rate_2023_2024 / sum(avg_growth_rate_2023_2024) * 100)

## Create a pie chart for average growth rates
ggplot(top_10_growth, aes(x = "", y = percentage, fill = reorder(country, -avg_growth_rate_2023_2024))) +
  geom_bar(stat = "identity", width = 1) +
  coord_polar(theta = "y") +
  labs(title = "Top 10 Countries by Growth Rate (2023-2024)", x = NULL, y = NULL) +
  geom_text(aes(label = paste0(round(percentage, 1), "%")), 
            position = position_stack(vjust = 0.5), 
            size = 4) +  # Adjust text size for clarity
  theme_void()  # Removes background and axes for a cleaner look

## SQL Queries and Analysis
This collection of SQL queries processes and analyzes GDP data from the GDPS_PER_COUNTRY database. Key operations include retrieving all records, handling missing values with COALESCE, and calculating growth rates for each country across multiple years. The queries also identify the top countries by total GDP and average growth rates, providing insights into economic performance. Additionally, the results are exported to a CSV file for further analysis and visualization in tools like R and Excel.

SELECT* FROM GDPS_PER_COUNTRY;

SET y2016 = COALESCE(y2016, y2015),
    y2017 = COALESCE(y2017, y2016),
    y2018 = COALESCE(y2018, y2017),
    y2019 = COALESCE(y2019, y2018),
    y2020 = COALESCE(y2020, y2019),
    y2021 = COALESCE(y2021, y2020),
    y2022 = COALESCE(y2022, y2021),
    y2023 = COALESCE(y2023, y2022),
    y2024 = COALESCE(y2024, y2023);
    
   SELECT Country, y2015, y2016, y2017, y2018, y2019, y2020, y2021, y2022, y2023, y2024
FROM GDPS_PER_COUNTRY
LIMIT 10;  -- Check the first 10 rows

## Check for Remaining Missing Values:

SELECT
    SUM(CASE WHEN y2015 IS NULL THEN 1 ELSE 0 END) AS y2015_nulls,
    SUM(CASE WHEN y2016 IS NULL THEN 1 ELSE 0 END) AS y2016_nulls,
    SUM(CASE WHEN y2017 IS NULL THEN 1 ELSE 0 END) AS y2017_nulls,
    SUM(CASE WHEN y2018 IS NULL THEN 1 ELSE 0 END) AS y2018_nulls,
    SUM(CASE WHEN y2019 IS NULL THEN 1 ELSE 0 END) AS y2019_nulls,
    SUM(CASE WHEN y2020 IS NULL THEN 1 ELSE 0 END) AS y2020_nulls,
    SUM(CASE WHEN y2021 IS NULL THEN 1 ELSE 0 END) AS y2021_nulls,
    SUM(CASE WHEN y2022 IS NULL THEN 1 ELSE 0 END) AS y2022_nulls,
    SUM(CASE WHEN y2023 IS NULL THEN 1 ELSE 0 END) AS y2023_nulls,
    SUM(CASE WHEN y2024 IS NULL THEN 1 ELSE 0 END) AS y2024_nulls
FROM GDPS_PER_COUNTRY;

## Proceed with Data Analysis or Visualization:

SELECT *
INTO OUTFILE 'C:/path/to/cleaned_gdp_data.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM GDPS_PER_COUNTRY;

SHOW VARIABLES LIKE 'secure_file_priv';

## Calculating the Average GDP Growth for Each Country (Across Years)
SELECT Country,
       ( (y2024 - y2023) / y2023 * 100 ) AS growth_2023_to_2024,
       ( (y2023 - y2022) / y2022 * 100 ) AS growth_2022_to_2023,
       ( (y2022 - y2021) / y2021 * 100 ) AS growth_2021_to_2022,
       ( (y2021 - y2020) / y2020 * 100 ) AS growth_2020_to_2021,
       ( (y2020 - y2019) / y2019 * 100 ) AS growth_2019_to_2020,
       ( (y2019 - y2018) / y2018 * 100 ) AS growth_2018_to_2019,
       ( (y2018 - y2017) / y2017 * 100 ) AS growth_2017_to_2018,
       ( (y2017 - y2016) / y2016 * 100 ) AS growth_2016_to_2017,
       ( (y2016 - y2015) / y2015 * 100 ) AS growth_2015_to_2016
FROM GDPS_PER_COUNTRY;

## Find Top Countries by GDP for a Specific Year

SELECT Country, y2024
FROM GDPS_PER_COUNTRY
ORDER BY y2024 DESC
LIMIT 10;  -- To get the top 10 countries

SELECT Country, y2022, y2023, y2024
FROM GDPS_PER_COUNTRY
ORDER BY y2024 DESC
LIMIT 10;

## Calculate Total and Average GDP Over Multiple Years

SELECT Country, 
       (y2015 + y2016 + y2017 + y2018 + y2019 + y2020 + y2021 + y2022 + y2023 + y2024) / 10 AS avg_gdp
FROM GDPS_PER_COUNTRY;

# SQL Query for Total GDP (Across Years):

SELECT Country, 
       (y2015 + y2016 + y2017 + y2018 + y2019 + y2020 + y2021 + y2022 + y2023 + y2024) AS total_gdp
FROM GDPS_PER_COUNTRY;

# Identify Countries with the Highest GDP Growth & SQL Query for GDP Growth from 2015 to 2024:

SELECT Country,
       ((y2024 - y2015) / y2015 * 100) AS total_growth
FROM GDPS_PER_COUNTRY
ORDER BY total_growth DESC
LIMIT 10;  -- To find the top 10 countries with the highest growth

# Finding Countries with Declining GDP
# SQL Query for Countries with Negative Growth:
SELECT Country,
       ((y2024 - y2015) / y2015 * 100) AS total_growth
FROM GDPS_PER_COUNTRY
WHERE ((y2024 - y2015) / y2015 * 100) < 0
ORDER BY total_growth ASC;

SELECT * 
INTO OUTFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/cleaned_gdp_data.csv'
FIELDS TERMINATED BY ','
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
FROM GDPS_PER_COUNTRY;

SHOW VARIABLES LIKE 'secure_file_priv';

### Key Features of the Dataset:
- Columns include country names, GDP for each year, total GDP, and average growth rates.
- The dataset is free from missing values and duplicates, ensuring accuracy in analysis.
  
### Data Source:
The data used for this project is sourced from **[data.gov](https://www.data.gov)**, a comprehensive resource for U.S. government data.

## Analysis and Visualizations
### Key Analyses Conducted:
1. Top 10 Countries by Total GDP: Identified the leading economies based on total GDP.
2. Highest Growth Rates (2023-2024): Analyzed the fastest-growing economies within this timeframe.

### Types of Visualizations:
- Bar Chart: Total GDP by Country.
- Line Chart: Growth rates for the top 10 countries.
- Donut Chart**: Distribution of total GDP among the top 10 countries.
- Scatter Plot**: Relationship between total GDP and growth rates.

  ## Visualizations
## GDP Comparison of Leading Economies, 2019 - 2024
  The chart titled **"GDP Comparison of Leading Economies, 2019 - 2024"** illustrates the GDP trends for ten major economies, including Brazil, China, France, Germany, India, Italy, Japan, Korea, the United Kingdom, and the United States over five years. Each colored line represents GDP values for the years 2019 through 2024. Notably, China** leads significantly in GDP, followed closely by the United States, indicating their dominant positions in the global economy. Germany and France show stable GDP figures, while India demonstrates a steady upward trajectory. The chart highlights fluctuations in Brazil's GDP, suggesting economic volatility, while other countries like Korea and Italy maintain moderate growth. Overall, the data underscores the resilience and shifting dynamics of global economic power.
  ![image](https://github.com/user-attachments/assets/20e75d4d-90d2-43ad-9652-e7cad0ccc896)

## Average Growth Rate for Top 10 Countries 2023 - 2024
The chart titled **"Top 10 Countries by Growth Rate (2023-2024)"** presents a clear comparison of the projected growth rates for the fastest-growing economies during this period. **Guyana** leads significantly with a growth rate of approximately **45%**, followed by **Macau** and **Libya**, which also exhibit strong growth. However, the remaining countries—**Senegal, Ukraine, Sri Lanka, Rwanda, Mauritania, Maldives,** and **Mozambique**—show markedly lower growth rates, stabilizing around **10% or less**. This visualization highlights the disparity in economic growth potential among these nations, emphasizing the significant economic opportunities in countries like Guyana while illustrating the challenges faced by others.

![image](https://github.com/user-attachments/assets/44afbb89-f67c-4fec-afdc-fcb0d19d8bb8)

## Percent of Total GDP/Country 2015 - 2024
The chart titled **"Percent of Total GDP/Country 2015 - 2024"** illustrates the distribution of total GDP among the leading economies from 2015 to 2024. Each segment of the donut chart represents the percentage share of total GDP for countries such as the **United States, China, Japan, Germany, the United Kingdom, India, France, Italy,** and **Brazil**. The **United States** holds the largest share at **36%**, reflecting its dominant position in the global economy. **China** follows with **26%**, showcasing significant economic strength. The other countries have smaller shares, with **Germany**, **Japan**, and the **United Kingdom** ranging from **3% to 8%**. This chart provides a clear visual representation of how GDP is distributed among these nations, highlighting the substantial economic influence of the U.S. and China compared to others.
![image](https://github.com/user-attachments/assets/103bdf84-8740-41de-a2a7-c7f42180f149)

## Total GDP per Country 2015 - 2024
The chart titled "Total GDP per Country 2015 - 2024" presents a comparison of total GDP values for various countries over the specified period. The bar chart clearly illustrates the significant economic disparity among these nations, with the United States and China leading by a substantial margin. The United States shows the highest total GDP, reaching nearly $20 trillion, while China follows closely with similar figures. Other countries, such as Germany, the United Kingdom, and India, display markedly lower GDP figures, emphasizing the dominance of the U.S. and China in the global economy. The visualization effectively highlights the economic landscape and the relative size of each country's economy.
![image](https://github.com/user-attachments/assets/b2f2874b-32f3-482b-be44-b5d1ecb48de5)


### Prerequisites
- Install the necessary software (MySQL, R, Excel, Tableau).
- Clone this repository or download the dataset files.

### Running the Analysis
1. Import the cleaned data into R for analysis.
2. Execute the provided R scripts to generate visualizations.
3. Use Tableau for interactive visualizations and dashboards.

## Acknowledgments
Thanks to the various sources of data and libraries that made this analysis possible, especially data.gov for providing the necessary datasets.
