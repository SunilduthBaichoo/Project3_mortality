# Project3_mortality
A Visualization project to study major mortality causes for informed decision making.

A powerpoint presentation of the project can be found here : https://docs.google.com/presentation/d/1qnH1wmWCbdr6HocKliGCOUD_IzJY2D7DmEgDXAFxV3w/edit#slide=id.gd251bb473_0_681


# Project3 Title
"Global Mortality Trends: Analyzing Leading Causes, Gender-Based Impacts, and Geographical Patterns Over the Last Decade"


# An overview of the project and its purpose
It’s a Data Visualization Track  project where we would like to get insights from the data loaded to create data output where we can recommend tourists not to go tp places on the globe due to infectious diseases hotspots, at the same time this data can be interpreted by law authorities to establish contingency plans where they can combat these diseases.  It will help private and hopefully local governments to enhance current public sanitation diseases in maybe one country but different areas. 


# Instructions on how to use and interact with the project
For visualising the interactive maps
1. Clone the project folder.
2. run the backend by using the command : python app.py (on bash)
3. open the visualisation dashboard at : [http](http://127.0.0.1:5000/)


CSV Files which are the main source to join our huge database, JSON Files where we have to convert CSV to JSON Files for DJ3 or other tools,  and SQLITE Files since we are using SQLITE to host the data for dynamic interactive interfaces.
In order to replicate the environment of our project we have created a dependencies list used in this project:

Import pandas as pd <br>
import numpy as np <br>
import os<br>
import plotly.express as px <br>
import plotly.grap_object as go <br>
import matplotlib.pyplot as plt <br>
import dash<br>
from dash import dcc, html <br>
from dash.dependecies import Input, Output<br>
from flask import Flask, jsonify, request, render_template<br>
from flask_sqlalchemy import SQLAlchemy<br>
from flask_cors import CORS<br>
from opencage.geocoder import OpenCageGeocode <br>
import pycountry # to convert numeric code to ISO country code used by plotly<br>
import geopandas as gpd<br>

# New libraries used not presented in class
pycountry library was used to convert numeric code to ISO country code used by plotly<br>
geopandas was used to obtain the geo-coordinates for each country.

# Efforts for ethical considerations made in the project

When investigating mortality data to answer research questions about the leading causes of death, age groups affected, and country-specific impacts, it is essential to prioritize ethical considerations to ensure the study's integrity and respect for individuals and communities represented in the data. First, data privacy and confidentiality must be upheld, especially when working with sensitive health information, even if it is aggregated. Efforts should be made to prevent the re-identification of individuals in the dataset. Second, the potential for bias in data collection, reporting, and analysis must be addressed to avoid perpetuating systemic inequities, such as disparities in healthcare access or socio-economic conditions. As Researchers, we must also consider cultural sensitivities and refrain from framing conclusions in a way that stigmatizes specific populations or countries. Transparent methodology, clear communication of limitations, and collaboration with public health experts can further enhance the ethical rigor of the study. Finally, the findings should be shared responsibly, focusing on actionable insights to inform public health policies and interventions rather than sensationalizing results.


# References for the data source(s)
Data Collection/Import:  https://platform.who.int/mortality/about/data-quality
This mortality data comes from WHO, World Health Organization, from different Excel Files that were compressed in zip folders, we decided to get our data evaluated on date-range which in this case is 10 years from 2013 to 2022, more than 1.9 million rows in 2 excel files that were converted to CSV files to make it easier at the time to convert it into a data-frame through python-pandas applying different types of cleaning, eventually will become an API to operate through Mongo or Python server. Additionally we have an extra 3 excel files with the code country and country list and the same for types of death, there are few  files which show all the types of deaths by code and the list of the due diseases.

# Data Validation and Cleaning (Data Transformation):
      - List steps for ensuring data integrity (e.g., handling missing values, ensuring type consistency).
-Our first steps were to check the heads and tails for the two main files, these are separated because morticd10_part4 File covers the same information on all the columns than morticd10_part5 the only difference between these two files is that Part 4 covers the years from 2013 to 2016 and Part 5 covers from 2017 to 2022 this being the last report generated by the WHO. 
The file complexity is significant since you can load more information through a list code system, in our case we had to add 2 more columns because the two main files only show the code country, cause and death list,  but not the actual names of the countries and causes. That's why we have to add these specific two columns into a CSV file to add them to the two main files to later on merge them into a general dataframe covering the aspects that we want to illustrate through the project. 
Important Note: Code 1000 and AAA represent all Causes, that’s why in the maindata for part4_df and part5_df, were  filtered out to avoid a unnecessary  pike on the graph
-FillNa option was applied to replace the NaN values for “0” in order to still get the right count, without affecting the total deletions of rows that cover important information.  
-Two Columns were deleted using DropNa Function
-NaN Cells were replaced with 0 value for future count code,
 -Renamed  Deaths1 column which is Total Deaths, for Total Deaths, 
-A few countries were deleted due to the lack of information, a total of 10 years of data was required to include it in the research. 
Part4_df was updated with the cleaning, that’s why in both data frames we started just deleting null values, to later change a few names of the columns just to be more appropriate or understandable at the time to read. This specific file from the ‘WHO’ does have a  more complementary list to add to the current dataframes mentioned (part4_df and part5_df. One perfect example is the fact that we can see in the raw CSV file the Column “List”, which is for defining which cause could be according to the code in the next column “Cause” since its only 4 digit code. Therefore get the list into a csv file in our folder to merge then later and have the Cause_Name linked to the Cause_Code. 
-We replaced the regular range death and range age that originally came with the document from 3-25 Death Columns having a new categorization for age to reduce from 39 Columns to 17 Columns. Having just 6 segments with more people because there are a few ranges (columns) that were basically empty, therefore we reduced the number of segments from 17 to 6 in both cases deaths-range and age-range, to make them more accurate. 
-two massive CSV files were marged, Part 4 = 880.543 Rows and Part 5 1.023.262 Rows, between then more than 1.9 millions Rows covering a decade of data making the analysis deeper.  Each file was applied the same methods of cleaning to guarantee the same columns just to add the rows from one file to another. making it a massive file of 1.9 million rows.  
This merge is named Data_Merged_df. and it was only including the next columns from part4_df and part5_df: Country_Name, Year, Cause, Sex. This specific data frame was used to create the dash of python we can check on the browser with the server number indicated in the code. This dash was the base to start  creating it on  HTML, CSS, JS and Json File. 

# Data Analysis:
      
Our analysis is focused to see how all the different causes affect the proportion of total deaths between genders, for example breast neoplasm affects 99% of female representation in the total deaths. However, this phenomenon looks to be natural, that’s why we find out another cause of deaths that are unproportional but have an explanation due to the nature of work, environment or context.
Examples for deaths due to nature of work, accidental drowning, accidental poisoned, in these two examples we can appreciate, that due to nature of work that are in contact with chemical poisons in the industry, which mainly jobs are filled with men, therefore it makes more sense that people who are in direct contact with poison can be accidentally poisoned. The same happened with the accidental drowning, where normally jobs like fishermen, transportation or more related jobs that navigate through the sea are filled out by men. 

# Data Storage:
      - Specify where and how the processed data will be stored (e.g., local storage, cloud databases).
Data has been stored in SQLITE database.

# Data Output:
      - Defining the outputs you expect (e.g., visualizations, reports, dashboards)
Visualisations using Bar charts, Pie charts and dashboards with interactive maps have been developed.
Bar charts showing 10 major mortality causes
Pie Charts showing how mortality for each cause vary among genders
Maps showing total deaths in each country per cause group per year.

# Definition of  Data Structure

data = [{   'Country_code': row.Country_code,<br>
            'Country_name': row.Country_name,<br>
            'ISO_Alpha3': row.ISO_Alpha3,<br>
            'Year': row.Year,<br>
            'Cause': row.Cause,<br>
            'Sex': row.Sex,<br>
            'Total_deaths': row.Total_deaths,<br>
        }    ]

# Document Tools/Technologies
Specify the tools and technologies you will use (e.g., Python libraries, database systems, visualization tools).
The following libraries were used:

Import pandas as pd <br>
import numpy as np <br>
import os<br>
import plotly.express as px <br>
import plotly.grap_object as go <br>
import matplotlib.pyplot as plt <br>
import dash<br>
from dash import dcc, html <br>
from dash.dependecies import Input, Output<br>
from flask import Flask, jsonify, request, render_template<br>
from flask_sqlalchemy import SQLAlchemy<br>
from flask_cors import CORS<br>
from opencage.geocoder import OpenCageGeocode <br>
import pycountry # to convert numeric code to ISO country code used by plotly<br>
import geopandas as gpd<br>

# Error Handling & Edge Cases
Mention how you will deal with errors or unexpected situations (e.g., corrupted files, outliers).
Control statements have been used by making use of Try and Catch to avoid accessing unavailable data and make the program crash.

# Workflow Example
Provide a step-by-step example scenario of the data workflow.
Use this as a concrete case study to help others understand the process.
User Interface (Frontend) <==> Flask App (Backend) <==> SQLITE Database
User Input (Cause Group, Year) ==> Frontend
(Frontend) <== (Visualization Requests (AJAX)) ==> (Backend)
User Interface (Frontend) consist of ( 
               HTML + CSS: Webpage Layout                    
               JavaScript: Interactivity & Data Fetching
               Plotly.js: Visualization (Maps, Bubbles, etc.))
Flask Routes:                                 
        - `/api/mortality`: Accepts query parameters   
            (e.g., Cause Group, Year) and dynamically 
            fetches filtered data from the database.  
             Data Formatting: JSON Response 
(Backend) <== (SQL Query with Parameters (Cause Group, Year)) ==> (SQLite Database)

# Conclusion and Next Steps
Conclude by summarizing the importance of the workflow.
The top 10 causes of mortality was investigated
Dynamic apps have been developed using DASH, FLASK HTML, CSS and Javascript fetching data from SQLITE, to visualise the trends as bar charts, pie charts and maps.
Some major trends were observed: 
1. Death rate is on decrease since 2020 after covid. 
2. There are more older people than ever before.
Outline future work or possible extensions.
1. Adding age-group analysis.
2. Exploring socio-economic factors.
3. Conducting cause-specific studies in high-risk regions.



# RESOURCES USED DURING THE PROJECT 
RESOURCES
Creating and customizing Plotly Pie Charts for data visualization
https://plotly.com/python/pie-charts/
Covid 19 Support 
https://data.who.int/dashboards/covid19/deaths 
Bar Charts using Dash with dropdowns
https://plotly.com/python/bar-charts/
Getting the Dots where they have to be, decimal rules. 
https://stackoverflow.com/questions/1526642/formatting-a-number-with-commas-as-thousands-separators
Birth Rate % to compare 
https://ourworldindata.org/births-and-deaths

Calculating percentage increments 
https://stackoverflow.com/questions/19929555/calculate-percentage-change-between-numbers
Basics pipeline
https://towardsdatascience.com/etl-pipeline-with-python-58a841b3c5f9
People Getting old expectancy of life first time 84.2 years in Canada 
https://www150.statcan.gc.ca/n1/daily-quotidien/201126/dq201126b-eng.htm
callback function 
https://dash.plotly.com/basic-callbacks
Using right groupy for the dashboards
https://realpython.com/pandas-groupby/
Font that supports after covid there was a reduced on deaths, 
https://ourworldindata.org/grapher/number-of-deaths-per-year

