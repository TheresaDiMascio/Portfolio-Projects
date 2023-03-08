# Portfolio-Projects
Theresa DiMascio's portfolio projects for professional resume.

The purpose of completing these projects is to demonstrate my proficiency in the skills required for a career in corporate research or business process analysis.

### **Project List:**

  - [**Project #1:** Multivariate Analysis of SAT Data](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%231:%20Multivariate%20Analysis%20of%20SAT%20Data)
  - [**Project #2:** ABC Analysis and Cycle Counts](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%232:%20ABC%20Analysis%20and%20Cycle%20Counts)
  - [**Project #3:** Health Insurance Dashboard](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%233:%20Health%20Insurance%20Dashboard)
  - [**Project #4:** Video Game Sales and Ratings](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%234:%20Video%20Game%20Sales%20and%20Ratings)

### **Project Summaries:**

  **Project #1: Multivariate Analysis of SAT Data**
  
  1. Found public dataset of New York City public high school SAT data (2014-2015) on Kaggle (Source: [Average SAT Scores for NYC Public Schools](https://www.kaggle.com/datasets/nycopendata/high-schools)).
  2. Uploaded CSV file (435 rows, 22 columns) to GitHub to use as primary data source.
  3. Used Power Query's Web.Content and CSV.Document data connectors to load the data from GitHub into Excel (worksheet "NYC SAT Data"): **PICTURE**
  4. Used Power Query's Web.Content and Pdf.Tables data connectors to read SAT score percentiles from College Board's official report on SAT scores (Source: [Understanding SAT Scores](https://satsuite.collegeboard.org/media/pdf/understanding-sat-scores.pdf)).
     - Original table import needed to be cleaned in Power Query by promoting headers, breaking double-column format into two tables, and appending: **PICTURE**
     - Removed null rows and changed data types to percentiles
     - Left with two tables (Total SAT Percentiles and SAT Section Percentiles) for looking up percentiles for SAT subsection and total scores
  5. Used Power Query's Web.Content and Excel.Workbook data connectors to import IRS's tax return data by ZIP code for New York (Source: [IRS New York Tax Returns by ZIP](https://www.irs.gov/pub/irs-soi/20zp33ny.xlsx)).
     - Initial load had many extraneous fields and needed to be transformed before use: **PICTURE**
     - Removed all fields except ZIP Code, Income Bracket, Count of Returns Filed, and Reported Income
     - Aggregated rows by ZIP code to calculate Average Total Income for each New York ZIP code: **PICTURE**
  6. Loaded all tables into Excel then added 18 calculated columns (orange headers) to NYC SAT Data worksheet:
     - If a school name contained certain words "=IF(ISNUMBER(SEARCH("SCIENCE", UPPER(\[@\[School Name]]))), TRUE(), FALSE())"
     - Test duration in "hrs" and "hrs+min" given start/end times
     - Total SAT 1600 and 2400 scores given three subsection scores.
     - School's percentile among the NYC high schools subpop. in math, reading, and total SAT 1600 score "=PERCENTRANK.INC(Z:Z, \[@\[Average Score (SAT Math)]])"
     - School's percentile among College Board's national SAT scores report "=XLOOKUP(10 * ROUND(\[@\[Average Score (SAT Math)]] / 10, 0), 'SAT Section Percentiles'!$A:$A, 'SAT Section Percentiles'!$D:$D, 0)"
     - True/false column for whether a school is in the top 50 for NYC public schools "=IF(RANK(\[@\[SAT 1600]], AD:AD, 0) <= 50, TRUE, FALSE)"
     - True/false column for if a school is above College Board's reported national average "=IF(\[@\[National Sample LOOKUP Total]] > 0.5, TRUE, FALSE)"
     - Relative strong subject "=IF(\[@\[National Sample LOOKUP Math]] > \[@\[National Sample LOOKUP Reading]], "Math", IF(\[@\[National Sample LOOKUP Reading]] > \[@\[National Sample LOOKUP Math]], "Reading", "Balanced Math/Reading"))"
  7. Created new worksheet (Exploratory Graphs) to visualize and perform multivariate analysis on the data:
     - Calculated inter-correlations between subsection scores. Reading/writing scores were highly correlated with each other. Math scores were strongly correlated with reading/writing, but not as much as reading with writing: **PICTURE**
     - Used scatterplot clustering to visualize a weak yet detectable correlation between school racial breakdown and subsection/total SAT scores: **PICTURE**
     - Proved a non-correlation between school name attributes (e.g. branding as a "science" school) and total SAT 2400 score.
     - Used Excel map visual to show a moderate geographical clustering of total SAT 2400 scores by ZIP code and provided side-by-side comparison of average reported income by ZIP code. Suburbs showed a similar clustering, implying correlation; however, Manhattan's business districts seem to skew data away from correlation in those particular ZIP codes: **PICTURE**
     - Visualized strong relationship between average SAT 2400 score and racial breakdown when data is statified by burough. Schools predominated by whites are more present in Staten Island, which performed higher on the SAT. Schools predominated by Hispanic/black students were more present in the Bronx/Brooklyn, which performed worse on the SAT: **PICTURE**
     - Used linear regression to model a moderate positive correlation between average SAT 2400 scores and percentage of student body that took the SAT: **PICTURE**

  
