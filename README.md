# IBM_Data_Science_Capstone_project
This repository contains all all the completed notebooks and Python files
SpaceX Falcon 9 First-Stage Landing Prediction: Project Summary

Overall Project Structure: We developed an end-to-end data science pipeline to predict Falcon 9 first-stage landing success using historical launch data, enabling cost-saving insights; the workflow spanned acquisition, wrangling, EDA, geospatial visualization, interactive dashboarding, and ML modeling, with extensibility for real-time 2025 integrations.

Phase 1: Multi-Source Data Acquisition

File Number 1: API Data Collection for SpaceX Launches (jupyter-labs-spacex-data-collection-api-v2.ipynb)

We queried the SpaceX API in real-time using the requests library to retrieve comprehensive launch records, then enriched them with targeted API calls for details like booster versions, launch site coordinates, payload masses, orbits, and core outcomes.
The data was generated using Pandas DataFrame assembly (~100 rows focused on Falcon 9 missions, filtering out Falcon 1), with sequential reindexing for flight numbers, mean imputation on missing payload values, and preservation of NaN for non-attempted landings to maintain data integrity.
The cleaned dataset was generated using export functionality and saved as dataset_part_1.csv, creating a fresh, API-sourced foundation for subsequent temporal and geospatial explorations.


File Number 2: Web Scraping Wikipedia for Falcon 9 Launch Records (jupyter-labs-webscraping.ipynb)

We scraped the June 2021 Wikipedia table snapshot using BeautifulSoup and requests, implementing custom parsing functions to extract and clean elements such as dates, booster versions, launch sites, payload masses, orbits, customers, launch outcomes, and booster landings while stripping HTML noise like references and tags.
The data was generated using dictionary structuring, which was converted into a Pandas DataFrame of ~260 rows to capture detailed historical records, including early mission specifics for cross-validation.
The dataset was generated using export functionality and saved as spacex_web_scraped.csv, ensuring a robust archival layer to complement API data and support longitudinal trend analysis.



Phase 2: Data Wrangling and Label Engineering

File Number 3: Data Wrangling and Labeling for SpaceX Dataset (labs-jupyter-spacex-Data wrangling.ipynb)

We merged the API and scraped datasets into a unified structure, then binarized the textual outcomes (e.g., "True ASDS" or "False RTLS") into a 'Class' column where 1 denoted successful landings and 0 indicated failures, establishing an overall ~67% success rate as our baseline metric.
The aggregated data was generated using audits for null values and orbit-specific success counts (e.g., identifying 21 successes for ISS missions), alongside distributional reviews for key variables like boosters and launch sites to flag anomalies.
The processed dataset was generated using date-filtered export and saved as dataset_part_2.csv (~98 rows), yielding a clean, labeled dataset primed for exploratory and predictive workflows.



Phase 3: Exploratory Data Analysis

File Number 4: SQL-Based EDA on SpaceX Dataset (jupyter-labs-eda-sql-coursera_sqllite.ipynb)

We ingested the CSV into a SQLite database table named SPACEXTABLE (101 rows total), applied initial cleaning to drop null dates and recreate the table for reliability, and leveraged ipython-sql for efficient querying.
The insights data was generated using a series of SQL operations, such as listing unique launch sites, filtering early records (e.g., five from CCAFS), aggregating payload metrics (e.g., total ~45,596 kg for NASA CRS missions and ~2,928 kg average for F9 v1.1 boosters), pinpointing the first ground-pad success (December 22, 2015), and ranking landing outcomes chronologically (2010–2017, revealing 98 total successes with "No attempt" as the most frequent).
The summarized results were generated using query exports to guide visualization and modeling phases, quantifying critical trends like site-specific efficiencies.


File Number 5: Exploratory Data Analysis with Visualizations for Falcon 9 Landings (jupyter_labs_eda_dataviz_v2.ipynb)

We loaded the dataset into Pandas for initial inspections, including shape analysis, null checks, and descriptive statistics to baseline variables like payload distributions and success rates.
The visualizations were generated using Matplotlib and Seaborn, such as histograms for payload masses, scatter plots linking flight numbers to payloads (colored by outcomes), pie charts for launch site breakdowns, bar charts of per-site success rates (e.g., 92% at KSC LC-39A), and box plots comparing outcomes by payload to highlight variance.
The enhanced dataset was generated using feature engineering (deriving sequential flight numbers, binary flags for elements like grid fins, reuse, and legs, cumulative reuse counts, and one-hot encodings for categorical variables like orbit and site), then exported as dataset_part_3.csv (~90 features), which illuminated patterns like progressive improvements in booster recoverability.



Phase 4: Geospatial Analysis of Launch Sites

File Number 6: Folium-Based Analysis of Launch Site Locations (lab_jupyter_launch_site_location_v2_(1).ipynb)

We utilized Folium to create an interactive world map centered on primary coordinates, adding custom markers and pop-up details for key sites (e.g., CCAFS LC-40 at 28.5623°N, 80.5774°W) to visualize their global positioning.
The clustered data was generated using overlays distinguishing successful (green) from failed (red) launches per site, calculating and displaying rates (e.g., 92% success at KSC LC-39A) to correlate location with performance.
The proximity measurements were generated using polyline overlays to environmental features, such as ~0.4 km to the nearest coast for VAFB SLC-4E, ~100 km to railways, ~5 km to highways, and ~85 km to nearby cities like Orlando, ultimately deriving insights into safety-driven site placements like coastal access for recoveries and deliberate distancing from urban or rail infrastructure.



Phase 5: Interactive Dashboard Development

File Number 7: Interactive SpaceX Launch Dashboard Using Dash (Dashboard Script)

We loaded the processed spacex_launch_dash.csv dataset and computed dynamic bounds for payload masses (min/max) to inform UI controls, then constructed the app layout with Dash components including a styled header, a searchable site dropdown (defaulting to "All Sites" with options like CCAFS LC-40), and a range slider for payloads (0–10,000 kg in 1,000 kg increments).
The interactive charts were generated using two callbacks: one updating a pie chart to reflect aggregated success proportions across sites or normalized counts for a selected site (e.g., success vs. failure splits), and another generating a scatter plot to explore correlations between payload mass (x-axis) and success (y-axis, binary), color-coded by booster version category with hover details on launch sites.
The application was generated using local execution via app.run(), enabling users to interactively filter and uncover trends, such as optimal payload ranges for higher recovery rates, in a responsive web interface ready for stakeholder review or cloud deployment.



Phase 6: Predictive Modeling and Evaluation

File Number 8: Machine Learning Prediction Pipeline for First-Stage Landings (SpaceX_Machine_Learning_Prediction_Part_5_v1.ipynb)

We preprocessed the feature set with standardization using scikit-learn's StandardScaler and performed an 80/20 train-test split to prepare for supervised classification.
The tuned models were generated using GridSearchCV on Logistic Regression, SVM (optimized C=0.01 achieving ~75% accuracy), Decision Trees (max_depth=10), and KNN (peaking at ~83% accuracy), evaluating each with confusion matrices, classification reports, and a comparative bar chart to benchmark performance.
The predictive framework was generated using KNN as the superior model for landing predictions, leveraging the pipeline to forecast reuse probabilities and inform economic decisions, such as competitive bidding strategies based on predicted success rates.
