# Portfolio-Projects
Theresa DiMascio's portfolio projects for professional resume.

The purpose of completing these projects is to demonstrate my proficiency in the skills required for a successful career in corporate research or business process analysis.

### **Project List:**

  - [**Project #1:** Multivariate Analysis of SAT Data](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%231:%20Multivariate%20Analysis%20of%20SAT%20Data)
  - [**Project #2:** ABC Analysis and Cycle Counts](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%232:%20ABC%20Analysis%20and%20Cycle%20Counts)
  - [**Project #3:** Health Insurance Dashboard](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%233:%20Health%20Insurance%20Dashboard)
  - [**Project #4:** Video Game Sales and Ratings **(WORK IN PROGRESS)**](https://github.com/TheresaDiMascio/Portfolio-Projects/tree/main/Project%20%234:%20Video%20Game%20Sales%20and%20Ratings)

### **Project Summaries:**

  **Project #1: Multivariate Analysis of SAT Data**
  
  1. Found public dataset of New York City public high school SAT data (2014-2015) on Kaggle (Source: [Average SAT Scores for NYC Public Schools](https://www.kaggle.com/datasets/nycopendata/high-schools)).
  2. Uploaded CSV file (435 rows, 22 columns) to GitHub to use as primary data source.
  3. Used Power Query's Web.Contents and CSV.Document data connectors to load the data from GitHub into Excel (worksheet "NYC SAT Data"):
  
  ![Power Query Load](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Power%20Query%20Load.png)
  
  5. Used Power Query's Web.Contents and Pdf.Tables data connectors to read SAT score percentiles from College Board's official report on SAT scores (Source: [Understanding SAT Scores](https://satsuite.collegeboard.org/media/pdf/understanding-sat-scores.pdf)).
     - Original table import needed to be cleaned in Power Query by promoting headers, breaking double-column format into two tables, and appending:
     
     ![SAT Percentiles Initial Load](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/SAT%20Percentiles%20Initial%20Load.png)
     
     - Removed null rows and changed data types to percentiles.
     - Left with two tables (Total SAT Percentiles and SAT Section Percentiles) for looking up percentiles for SAT subsection and total scores:
     
     ![SAT Percentiles Final](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/SAT%20Percentiles%20Final.png)
     
  6. Used Power Query's Web.Content and Excel.Workbook data connectors to import IRS's tax return data by ZIP code for New York (Source: [IRS New York Tax Returns by ZIP](https://www.irs.gov/pub/irs-soi/20zp33ny.xlsx)).
     - Initial load had many extraneous fields and needed to be transformed before use:
     
     ![IRS Tax Initial Load](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/IRS%20Tax%20Initial%20Load.png)
     
     - Removed all fields except ZIP Code, Income Bracket, Count of Returns Filed, and Reported Income.
     - Aggregated rows by ZIP code to calculate Average Total Income for each New York ZIP code:
     
     ![IRS Returns by ZIP](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Returns%20by%20ZIP.png)
     
  7. Loaded all tables into Excel then added 18 calculated columns (orange headers) to "NYC SAT Data" worksheet:
     - If a school name contained certain words: "=IF(ISNUMBER(SEARCH("SCIENCE", UPPER(\[@\[School Name]]))), TRUE(), FALSE())"
     - Test duration in "hrs" and "hrs+min" given start/end times.
     - Total SAT 1600 and 2400 scores given three subsection scores.
     - School's percentile among the NYC high schools subpop. in math, reading, and total SAT 1600 score: "=PERCENTRANK.INC(Z:Z, \[@\[Average Score (SAT Math)]])"
     - School's percentile among College Board's national SAT scores report: "=XLOOKUP(10 * ROUND(\[@\[Average Score (SAT Math)]] / 10, 0), 'SAT Section Percentiles'!$A:$A, 'SAT Section Percentiles'!$D:$D, 0)"
     - True/false column for whether a school is in the top 50 for NYC public schools: "=IF(RANK(\[@\[SAT 1600]], AD:AD, 0) <= 50, TRUE, FALSE)"
     - True/false column for if a school is above College Board's reported national average: "=IF(\[@\[National Sample LOOKUP Total]] > 0.5, TRUE, FALSE)"
     - Relative strong subject: "=IF(\[@\[National Sample LOOKUP Math]] > \[@\[National Sample LOOKUP Reading]], "Math", IF(\[@\[National Sample LOOKUP Reading]] > \[@\[National Sample LOOKUP Math]], "Reading", "Balanced Math/Reading"))"
  8. Created new worksheet ("Exploratory Graphs") to visualize and perform multivariate analysis on the data:
     - Calculated inter-correlations between subsection scores. Reading/writing scores were highly correlated with each other. Math scores were strongly correlated with reading/writing, but not as much as reading with writing:
     
     ![Subsection Inter-Correlation](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Subsection%20Inter-Correlations.png)
     
     - Used scatterplot clustering to visualize a weak yet detectable correlation between school racial breakdown and subsection/total SAT scores:
     
     ![Race Clustering](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Race%20Clustering.png)
     
     - Proved a non-correlation between school name attributes (e.g. branding as a "science" school) and total SAT 2400 score.
     - Used Excel map visual to show a moderate geographical clustering of total SAT 2400 scores by ZIP code and provided side-by-side comparison of average reported income by ZIP code. Suburbs showed a similar clustering, implying correlation; however, Manhattan's business districts seem to skew data away from correlation in those particular ZIP codes:
     
     ![Geographic Clustering](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Geographic%20Clustering.png)
     
     - Visualized strong relationship between average SAT 2400 score and racial breakdown when data is statified by burough. Schools predominated by whites are more present in Staten Island, which performed higher on the SAT. Schools predominated by Hispanic/black students were more present in the Bronx/Brooklyn, which performed worse on the SAT:
     
     ![Racial Breakdown by Burough](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Racial%20Breakdown%20by%20Burough.png)
     
     - Used linear regression to model a moderate positive correlation between average SAT 2400 scores and percentage of student body that took the SAT:
     
     ![Percent Tested](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%231%3A%20Multivariate%20Analysis%20of%20SAT%20Data/Percent%20Tested.png)

  **Project #2: ABC Analysis and Cycle Counts**
  
  1. The first goal of this project was to systematically generate an item master of 5,000 parts for a fictitious musical instrument company then assign inventory, cost, and consumption values to each item. The second goal was to define 3 valid methodologies for giving each item an ABC value (see [Wikipedia](https://en.wikipedia.org/wiki/ABC_analysis) for more information) so that they could be efficiently cycle counted according to auditor standards.
  2. The general categorizations for items was sketched out in the "Instrument Table" worksheet. This corporation sells 28 musical instruments/accessories within 6  instrument families (pianos, strings, woodwinds, brass, percussion, and other). The features of the item master summarized on this page are:
     - Each instrument has between 1 and 25 sellable variations (models). Software, instructional materials, and accessories have between 40 and 80 variations. These comprise 300 retail items in the item master.
     - Instruments have between 5 and 20 related retail parts (e.g. cases, bows, reeds, stands, etc.) that are directly related to a sellable instrument. Including the 200 of these related retail parts, there are 500 total retail parts.
     - There are 500 related service parts (e.g. replacement strings, electronics, installment services, etc.). Of these, 400 are directly related to a single instrument while 100 are shared among instrument families.
     - There are 4000 production parts (e.g. wood, screws, tools, unshaped metals, etc.). 3000 of these are directly related to a single instrument while 1000 are shared among instrument families.
  3. Quantities for each of these item types were arbitrarily assigned roughly to match their complexity/number of models. Formulas were defined in columns H, I, and J to give indices to the start of these item groups in the item master. **_Formula:_** "=$J6 + $G6 + IF($C5 <> $C6, VLOOKUP("Shared " & $C6, $E$35:$G$40, 3, FALSE))"
  
  ![Instrument Table](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Instrument%20Table.png)
  
  4. The "Item Master" worksheet uses these indces to assign an item number (e.g. 10001-01) to each of the 5,000 items systematically as follows:
     - First digit represents whether the item is a production, service, or retail item.
     - Next 2 digits represent the related instrument where the first digit corresponds with the instrument family.
     - Next 2 digits represent the instrument/accessory model using XMATCH and VLOOKUP functions. **_Formula_:** "=TEXT(IF($B6 = "Retail Item", IF((ROW($A6) - 4504 - XMATCH($C6, $C$4506:$C$5005, 0, 1)) <= VLOOKUP($C6, 'Instrument Table'!$B:$D, 3, FALSE), ROW($A6) - 4504 - XMATCH($C6, $C$4506:$C$5005, 0, 1), 0), 0), "00")"
     - Item suffix of 3 digits counts up from 000 if a subsequent line contains a new model number.
     - "Item Description" summarizes this numbering logic to describe each item. **_Formula_:** "LEFT($C6, 1) & LOWER(RIGHT($C6, LEN($C6) - 1)) & IF(AND($G6 <> "00", $B6 = "Retail Item"), " model #" & $G6, " " & LOWER($B6) & " #" & $H6)"
     - "Unique" checks that each item number is a unique primary key using SUM(COUNTIFS()).
     
  ![Item Master 1](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Item%20Master%201.png)
  
  5. Item Master then uses partially random methods to assign values for iventory, standard cost, and annual consumption for each item. Parameters were tweaked until the "Pivot Table" worksheet produced inventory/consumption totals that made sense for the business:
     - Inventory was assigned random uniformly for service/retail items and according to a randomized power function for production. **_Formula_:** "=ROUND(IF($B6 = "Production Item",POWER(RANDBETWEEN(0, 2), RANDBETWEEN(1, 5)), IF($B6 = "Service Item", RANDBETWEEN(0, 10), RANDBETWEEN(0, 100))), 2)"
     - Standard Cost was assigned uniformly for all items. Retail items cost between $1,000 and $10,000 (this is not the same as price). **_Formula_:** "=ROUND(IF($B6 = "Production Item", RANDBETWEEN(1, 100), IF($B6 = "Service Item", RANDBETWEEN(1, 1000), RANDBETWEEN(1000, 10000))), 2)"
     - Annual Consumption (how many of each item that are used in production/sold each year) was defined with a random power function for production/service items and random uniformly for retail items. Between 5 and 100 of each retail item are sold each year. **_Formula_:** "=ROUND(IF($B6 = "Production Item", POWER(RANDBETWEEN(0, 20), 2), IF($B6 = "Service Item", POWER(RANDBETWEEN(0, 5), 3), RANDBETWEEN(5, 100))), 2)"
  6. Five calculated columns were added to the "Item Master" worksheet (one calculation per item):
     - Inventory value = Inventory x Standard Cost
     - Annual Consumption Value = Consumption x Standard Cost
     - Consumption Rank = where an item ranks with respect to its annual consumption value = "COUNTIFS($P:$P, ">" & $P6) + 1"
     - Percent Rank = consumption rank as a percentage = "(COUNTIFS($P:$P, ">" & $P6) + 1) / (COUNTA($I:$I) - 1)"
     - Cumulative Consumption = total consumption of all items that have a lower annual consumption than this item = "SUMIFS($P:$P, $Q:$Q, "<" & $Q6)"
  
  ![Item Master 2](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Item%20Master%202.png)
  
  7. The "ABC Analysis" worksheet was created with the following features:
     - User-defined counts per year for A, B, and C items. These parameters are usually set by auditing standards. A items need to be cycle counted more often to maintain inventory accuracy because they are more valuable and have more turnover (consumption). B items are either valuable or have high turnover. C items are either low in value or low in turnover.
     
     ![Auditor Standards](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Auditor%20Standards.png)
     
     - User-defined counting days per year. A typical business would only count on business days (around 250 days per year), but larger companies may cycle count 365 days a year.
     
     ![Counting Days](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Counting%20Days.png)
     
     - Calculator #1 (item-based) lets the user set a percentage of items that are A, B, or C. The model then suggests a certain number of daily counts of each item and calculates the amount of consumption that is verified by cycle counting according to the auditing standards set by the user. If the total percentage is below 100%, the items that are not assigned A, B, or C will not be counted, and the "Consumption per Count" measure will increase (more efficient counting). However, this inventory will not be verified at any point during the year.
     - Calculator #2 (consumption-based) lets the user set a percentage of consumption that is assigned to A, B, and C items. The model then assigns a percentage of items as A, B, or C.
     - Calculator #3 (counting resources-based) lets the user choose how many daily cycle counts of A and B items they desire then the model assigns a percentage of items as A, B, or C and gives a suggested daily count of C items to meet auditor standards.
     
     ![Calculators](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Calculators.png)
     
  8. Two measures of inventory accuracy ("Cycle Counted Consumption" and "Cycle Counted Inventory") were constructed to allow comparison of user inputs across calculators.
  9. Two measures of counting efficiency ("Consumption per Count" and "Inventory per Count") were constructed to allow comparison of user inputs across calculators.
  10. Two graphs ("Cumulative Consumption Distribution by Item" and "Annual Consumption by Item") were constructed to give the user to make more information when selecting item/consumption percentages. A items would be on the left-hand side of each graph, B items would be in the middle, and C items are on the far right:
  
  ![Two Graphs](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%232%3A%20ABC%20Analysis%20and%20Cycle%20Counts/Two%20Graphs.png)

  **Project #3: Health Insurance Dashboard**
  1. Found public dataset of health insurance information on Kaggle (Source: [Medical Cost Personal Datasets](https://www.kaggle.com/datasets/mirichoi0218/insurance)).
  2. Uploaded CSV file (1338 rows, 7 columns) to GitHub to use as primary data source.
  3. Used Power Query's Web.Contents and Csv.Document data connectors to import the health insurance data into Power Query Editor.
  4. Added index column to be used as policyholder's unique ID number.
  5. Wrote the custom "gaussianHeight" function in Power Query to assign a random (Gaussian) height to male and female policyholders given the national averages and standard deviations:
  
  ![gaussianHeight](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%233%3A%20Health%20Insurance%20Dashboard/Gaussian%20Height%20Function.png)
  
  6. Added calculated column "Weight (lbs)" using BMI and height. **_Formula:_** "Number.Round(\[bmi] * Number.Power(\[#"Height (in)"], 2) / 703, 0)"
  7. Added two columns with random numbers between 0 and 1 for generating a probable first and last name for each policyholder.
  8. Used Power Query's Web.Contents and Web.Page data connectors to pull BMI ranges of underweight, healthy, overweight, and obese Americans from the CDC's official website (Source: [About Adult BMI](https://www.cdc.gov/healthyweight/assessing/bmi/adult_bmi/index.html)).
  9. Used Power Query's Web.Contents and Web.Page data connectors to pull the top 200 baby names of each decade from the Social Security Administration's official website (Source: [Popular Baby Names by Decade]([https://www.ssa.gov/OACT/babynames/decades/names1950s.html](https://www.ssa.gov/OACT/babynames/decades/)). Cleaned the data and added 4 calculated columns (orange headers) to assist with random name generation:
     - Two columns for male and female probability density functions (i.e. the probability of a certain name being chosen within a certain decade). **_Formula_:** "= \[@\[Males Number]] / SUMIFS($C:$C, $F:$F, \[@Decade])"
     - Two columns for male and female cumulative density functions (i.e. the probability of a certain name or a more common name being chosen within a certain decade). **_Formula_:** "=SUMIFS($G:$G, $A:$A, "<=" & \[@Rank], $F:$F, \[@Decade])"
  10. Used Power Query's Web.Contents and Excel.Workbook data connectors to pull the 1000 most common last names from the 2010 U.S. Census official website (Source: [2010 Surnames](https://www2.census.gov/topics/genealogy/2010surnames/)). Added 1 calculated column to assist with random last name generation:
     - Cumulative density function to represent the probability of having a certain name or a more common name in 2010. **_Formula_:** "=\[@\[CUMULATIVE PROPORTION]] / SUM($D:$D)"
  11. Created "User Dashboard" worksheet to act as main interface between policyholder and their confidential health information. Check the box in cell A4 (Excel for desktop only) to view the dashboard in explanation mode:
  
  ![Explanation Mode](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%233%3A%20Health%20Insurance%20Dashboard/Explanation%20Mode.png)
  
  12. The policyholder enters their unique ID into cell D7 to view their confidential health information. Features of the dashboard include:
     - Age, sex, and BMI, which were pulled from the original data source. BMI is conditionally formatted to show how overweight (red) or underweight (yellow) a policyholder is.
     - Condition, which is assigned using XLOOKUP. Based upon the CDC guidelines for BMI.
     - Height and weight, which have slicers (yellow), which uses Excel's data validation feature so the user can change from imperial (in, ft'in", and lbs) to metric (kg and cm) units.
     - Subpopulation averages, which use AVERAGEIFS to show the user where they stand with respect to the subpopulation of policyholders of the same gender.
     - Charges and charges percentile, which show the user their medical costs and where they stand within the subpopulation.
     - If the policyholder is a smoker, the dashboard reminds them how much they would save on average if they quit smoking.
  13. The probable name generator uses the Social Security Association and 2010 U.S. Census datasets to assign a first and last name to each unique ID given their two random numbers between 0 and 1. The methodology for doing so is as follows:
     - The formulas use XLOOKUP, VLOOKUP, and FILTER to see where their random number lies within the cumulative density function (CDF) of the "First Name Rankings Appended" and "2010 Census Top 1000 Last Names" tables.
     - If the random number doesn't match a CDF exactly, XLOOKUP will find the name that is next greatest.
     - FILTER only allows the lookup to search within the birth decade of the policyholder.
  14. The dashboard tells the user how popular their first and last names are as a rank (with correct ordinal suffix) within their gender/birth decade:
  
  ![Probable Name Generator](https://raw.githubusercontent.com/TheresaDiMascio/Portfolio-Projects/main/Project%20%233%3A%20Health%20Insurance%20Dashboard/Probable%20Name%20Generator.png)
  
  **Project #4: Video Game Sales and Ratings (WORK IN PROGRESS)**
