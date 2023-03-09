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

  **Project #2: ABC Analysis and Cycle Counts**
  
  1. The first goal of this project was to systematically generate an item master of 5,000 parts for a fictitious musical instrument company then assign inventory, cost, and consumption values to each item. The second goal was to define 3 valid methodologies for giving each item an ABC value (see [Wikipedia](https://en.wikipedia.org/wiki/ABC_analysis) for more information) so that they could be efficiently cycle counted according to auditor standards.
  2. The general categorizations for items was sketched out in the "Instrument Table" worksheet. This corporation sells 28 musical instruments/accessories within 6  instrument families (pianos, strings, woodwinds, brass, percussion, and other). The features of the item master summarized on this page are:
     - Each instrument has between 1 and 25 sellable variations (models). Software, instructional materials, and accessories have between 40 and 80 variations. These comprise 300 retail items in the item master.
     - Instruments have between 5 and 20 related retail parts (e.g. cases, bows, reeds, stands, etc.) that are directly related to a sellable instrument. Including the 200 of these related retail parts, there are 500 total retail parts.
     - There are 500 related service parts (e.g. replacement strings, electronics, installment services, etc.). Of these, 400 are directly related to a single instrument while 100 are shared among instrument families.
     - There are 4000 production parts (e.g. wood, screws, tools, unshaped metals, etc.). 3000 of these are directly related to a single instrument while 1000 are shared among instrument families.
  3. Counts for each of these item types were arbitrarily assigned roughly to match their complexity/number of models. Formulas were defined in columns H, I, and J to give indices to the start of these item groups in the item master. **_Ex_:** "=$J6 + $G6 + IF($C5 <> $C6, VLOOKUP("Shared " & $C6, $E$35:$G$40, 3, FALSE))"
  4. The "Item Master" worksheet uses these indces to assign an item number to each of the 5000 items systematically as follows:
     - First digit represents whether the item is a production, service, or retail item.
     - Next 2 digits represent the related instrument where the first digit corresponds with the instrument family.
     - Next 2 digits represent the instrument/accessory model using XMATCH and VLOOKUP functions. **_Formula_:** "=TEXT(IF($B6 = "Retail Item", IF((ROW($A6) - 4504 - XMATCH($C6, $C$4506:$C$5005, 0, 1)) <= VLOOKUP($C6, 'Instrument Table'!$B:$D, 3, FALSE), ROW($A6) - 4504 - XMATCH($C6, $C$4506:$C$5005, 0, 1), 0), 0), "00")"
     - Item suffix of 3 digits counts up from 000 if a subsequent line contains a new model number.
     - "Item Description" summarizes this numbering logic to describe each item. **_Formula_:** "LEFT($C6, 1) & LOWER(RIGHT($C6, LEN($C6) - 1)) & IF(AND($G6 <> "00", $B6 = "Retail Item"), " model #" & $G6, " " & LOWER($B6) & " #" & $H6)"
     - "Unique" checks that each item number is a unique primary key using SUM(COUNTIFS()).
  5. Item Master then uses partially random methods to assign values for iventory, standard cost, and annual consumption for each item. Parameters were tweaked until the "Pivot Table" worksheet produced inventory/consumption totals that made sense for the business:
     - Inventory was assigned random uniformly for service/retail items and according to a randomized power function for production. **_Formula_:** "=ROUND(IF($B6 = "Production Item",POWER(RANDBETWEEN(0, 2), RANDBETWEEN(1, 5)), IF($B6 = "Service Item", RANDBETWEEN(0, 10), RANDBETWEEN(0, 100))), 2)"
     - Standard Cost was assigned uniformly for all items. Retail items cost between $1,000 and $10,000 (this is not the same as price). **_Formula_:** "=ROUND(IF($B6 = "Production Item", RANDBETWEEN(1, 100), IF($B6 = "Service Item", RANDBETWEEN(1, 1000), RANDBETWEEN(1000, 10000))), 2)"
     - Annual Consumption (how many of each item that are used in production/sold each year) was defined with a random power function for production/service items and random uniformly for retail items. Between 5 and 100 of each retail item are sold each year. **_Formula_:** "=ROUND(IF($B6 = "Production Item", POWER(RANDBETWEEN(0, 20), 2), IF($B6 = "Service Item", POWER(RANDBETWEEN(0, 5), 3), RANDBETWEEN(5, 100))), 2)"
  6. Five calculated columns were added to the "Item Master" worksheet:
     - Inventory value = Inventory x Standard Cost
     - Annual Consumption Value = Consumption x Standard Cost
     - Consumption Rank = where an item ranks with respect to its annual consumption value = "COUNTIFS($P:$P, ">" & $P6) + 1"
     - Percent Rank = consumption rank as a percentage = "(COUNTIFS($P:$P, ">" & $P6) + 1) / (COUNTA($I:$I) - 1)"
     - Cumulative Consumption = total consumption of all items that have a lower annual consumption than this item = "SUMIFS($P:$P, $Q:$Q, "<" & $Q6)"
