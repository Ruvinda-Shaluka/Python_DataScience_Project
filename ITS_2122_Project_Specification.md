# Group Project | ITS 2122 Python for Data Science & AI

**ITS 2122: Python for Data Science & AI**
**Group Project Specification**
**Semester 3, 2026**

## PROJECT TITLE
**Credit Risk & Customer Financial Health Analytics for a Digital Lending Provider**
**HDSE 73**

| Domain | Finance / Digital Lending / Credit Risk Analytics |
| :--- | :--- |
| **Module Component** | Final Group Project & Presentation (40% of module grade) |
| **Deliverables** | Business Report (PDF) + Technical Appendix (Jupyter Notebook) + Presentation |

*IJSE Institute of Software Engineering*

---

## Part 1: The Business Mandate & Project Overview

### 1.1 Introduction: Your Role as Data Science Consultants
For this capstone project, your group will operate as a team of data science consultants engaged by the executive board of "FinTrust Digital Finance," a fast-growing financial technology lender that issues credit cards and short-term personal credit to salaried customers.

The company has acquired a large base of borrowers and accumulated a substantial volume of account and repayment data. However, its credit-monitoring and collections decisions remain largely manual and intuitive rather than data-driven.

The leadership team recognises that to manage risk responsibly, reduce defaults, and protect both the business and its customers, it must treat its data as a strategic asset. Your team's mandate is to analyse historical credit-card account data, uncover the behavioural drivers of repayment and default, and deliver a clear, evidence-based risk-monitoring framework. You are expected to move beyond simple reporting and produce high-value intelligence that can directly inform credit policy, early-warning monitoring, and customer engagement.

### 1.2 The Core Challenge: From Data to Decisions
The management team has provided you with a portfolio of customer accounts covering demographics, granted credit limits, six months of bill statements, six months of repayments, repayment-status history, and a flag indicating whether each customer defaulted in the following month.

They have identified several critical blind spots and have tasked your team with addressing the following fundamental questions:

1. **Default Risk Profiling:** What is the overall default rate, and how does it vary across age bands, education levels, marital status, and credit-limit tiers? Which customer groups carry the greatest risk?
2. **Repayment Behaviour:** How do repayment delays, bill amounts, and payment amounts over the recent months relate to the likelihood of default? Which behaviours are the strongest early-warning signals?
3. **Credit Exposure vs. Capacity:** Where is the company over-exposed — that is, where are high credit limits combined with weak repayment behaviour, and what is the financial significance of these accounts?
4. **Customer Segmentation:** Can we move beyond a one-size-fits-all approach and group customers into meaningful financial-health segments (for example Healthy, Watchlist, At-Risk, Critical) based on their behaviour?
5. **Actionable Strategy:** How should each segment be managed differently through reminders, credit reviews, restructuring offers, or financial-wellness support, while remaining fair and transparent?

### 1.3 Project Objectives
This project is a comprehensive application of the skills and concepts covered in ITS 2122. Successful completion will require your team to demonstrate proficiency across the full data-analysis lifecycle. Specifically, you will be expected to:

* **Data Ingestion & Cleaning:** Apply robust loading, validation, and preprocessing techniques with Pandas to handle real-world imperfections such as invalid category codes, duplicates, and inconsistent values. This assesses Module 5 (File Handling) and Module 6 (Data Manipulation).
* **Exploratory Data Analysis (EDA):** Conduct a thorough EDA with Pandas and NumPy to dissect repayment behaviour, surface patterns, and answer the board's core questions. This assesses Module 6 (Data Manipulation).
* **Data Visualization:** Produce a portfolio of clear, professionally formatted visualizations with Matplotlib and Seaborn to communicate findings to a non-technical audience. This assesses Module 7 (Data Visualization).
* **Advanced Analytics & Modelling:** Engineer behavioural features and implement an interpretable credit-risk scoring and segmentation framework using Python data structures, user-defined functions, and advanced Pandas operations. This covers Modules 2, 3, and 6.
* **Data Enrichment:** Acquire and integrate external finance data through a public web API using the requests library. This assesses Module 8 (APIs, Data Collection & Debugging).
* **Professional Communication:** Synthesise all findings into a persuasive, professionally structured business report that delivers actionable, evidence-based recommendations.

---

## Part 2: The Primary Asset - Data & Tools

### 2.1 The Dataset: Default of Credit Card Clients
The foundation of your analysis will be the public "Default of Credit Card Clients" dataset from the UCI Machine Learning Repository [1].

The dataset contains 30,000 customer records from a credit-card issuer, including demographic attributes, granted credit, six months of billing and payment history, and a target variable indicating default in the next month. It is well documented and highly relevant to the business scenario.

Your first task, which directly tests your file-handling capabilities (Module 5), will be to load the provided data file (.csv or .xls) into a Pandas DataFrame to begin your analysis.

### 2.2 Data Dictionary
A precise understanding of the data is critical for meaningful analysis. The following table summarises the key fields and the nuances your team must account for during cleaning and feature engineering.

| Variable | Description | Key Considerations & Notes |
| :--- | :--- | :--- |
| **ID** | Unique identifier for each customer account. | An identifier only; exclude from any statistical or correlation analysis. |
| **LIMIT_BAL** | Amount of granted credit (the credit limit), in currency units. | Use for exposure analysis and credit-limit tiers; check for extreme outliers. |
| **SEX, EDUCATION, MARRIAGE** | Demographic attributes, stored as numeric codes. | Must be decoded into readable labels. Some codes are undocumented/unknown and must be handled and justified. |
| **AGE** | Customer age in years. | Group into age bands for EDA; validate for impossible values. |
| **PAY_1 ... PAY_6** | Repayment status for the six most recent months. | Core delinquency signal. Positive values indicate months of delay; document the encoding you adopt. |
| **BILL_AMT1 ... BILL_AMT6** | Bill statement amount for each of the six months. | Used for balance trends and utilisation-style features; values can be zero or negative. |
| **PAY_AMT1 ... PAY_AMT6** | Amount paid in each of the six months. | Used for repayment consistency and payment-to-bill ratios; guard against divide-by-zero. |
| **default payment next month** | Target: 1 if the customer defaulted next month, else 0. | Used to compute default rates and validate the risk segments you build. |

*Note: In the given dataset, all currency-related data are expressed in New Taiwan Dollars (NTD/TWD).*

### 2.3 Required Python Libraries
To complete this project, you will use the core data-science stack taught in this module. Your Jupyter Notebook environment should import the following essential libraries:

* `pandas`: The primary tool for loading, cleaning, transforming, grouping, and analysing the data.
* `numpy`: For numerical operations and custom feature calculations.
* `matplotlib` and `seaborn`: The standard libraries for clear, annotated, and aesthetically pleasing visualizations.
* `requests`: Required for the API integration task in Phase 5.

---

## Part 3: The Analytical Blueprint - A Phased Approach
Follow this structured, phased approach to guide your analysis from raw data to strategic recommendations. Each phase builds upon the last and is designed to ensure a comprehensive, methodologically sound project.

### 3.1 Phase 1: Data Sanitation and Preprocessing
The quality of your insights depends directly on the quality of your data. This phase transforms the raw dataset into a clean, reliable, analysis-ready format.

1. **Load Data:** Ingest the dataset into a Pandas DataFrame and confirm the row/column counts against the documentation.
2. **Initial Assessment:** Audit the data using `.info()`, `.describe(include='all')`, `.isnull().sum()`, and `.value_counts()` on categorical fields.
3. **Rename Columns:** Convert columns to clear, Python-friendly names (for example `limit_balance`, `pay_status_1`, `bill_amount_1`, `default_next_month`).
4. **Decode Categories:** Map the numeric `SEX`, `EDUCATION`, and `MARRIAGE` codes to readable labels, explicitly handling undocumented/unknown codes.
5. **Handle Duplicates & Invalid Values:** Remove duplicate rows and validate ranges for age, repayment status, bills, and payments; state and justify every decision.
6. **Feature Engineering:** Create behavioural features such as average bill amount, average payment amount, payment-to-bill ratio, number of delayed months, maximum delay, and a recent-balance trend indicator.
7. **Persist the Result:** Export the cleaned dataset (for example `cleaned_credit_data.csv`) to support reproducibility.

### 3.2 Phase 2: Exploratory Data Analysis & Insight Generation
With a clean dataset, dive into exploration to uncover patterns and answer the board's questions, moving from observation to interpretation.

1. **Default-Rate Analysis:** Compute the overall default rate and compare it across age bands, education, marital status, and credit-limit tiers using grouped summaries and bar charts.
2. **Repayment-Behaviour Analysis:** Examine repayment-status patterns across the six months and identify which delays most strongly relate to default.
3. **Financial Position Analysis:** Compare bill amounts, payment amounts, and payment-to-bill ratios for defaulted versus non-defaulted customers.
4. **Relationship Analysis:** Use correlation analysis and/or grouped aggregations to investigate how credit limit, bills, payments, and delays relate to default; present a correlation heatmap.
5. **Visual Portfolio:** Produce at least eight meaningful visualizations, each with a clear title, labelled axes, an appropriate chart type, and a short written interpretation.

### 3.3 Phase 3: Advanced Analytics: Credit-Risk Segmentation
This phase moves from descriptive to diagnostic analytics. You will build an interpretable credit-risk scoring framework — analogous in rigour to an RFM model — to quantitatively rank and segment customers by financial health.

1. **Define Risk Dimensions:** Select clear behavioural dimensions, for example Delinquency (repayment delay), Repayment Capacity (payment-to-bill ratio), and Exposure (credit limit / outstanding balance).
2. **Score Customers:** For each dimension, use `pandas.qcut()` or documented rules to assign scores (e.g. 1-5), ensuring the direction is correct (worse behaviour = higher risk).
3. **Combine Scores:** Concatenate or aggregate the dimension scores into an overall risk score implemented through a reusable user-defined function.
4. **Map to Segments:** Use a Python dictionary or function to translate scores into descriptive, business-friendly segments. Validate each segment by checking its actual default rate.

Your segmentation should produce business-interpretable groups similar to the following framework:

| Segment | Characteristics |
| :--- | :--- |
| **Healthy** | Pays on time, strong payment-to-bill ratio, low exposure relative to limit. |
| **Watchlist** | Occasional short delays or a weakening payment ratio; early signs of stress. |
| **At-Risk** | Repeated delays, high outstanding bills, or low repayment consistency. |
| **Critical/Intervention** | Severe and sustained delays with high exposure; highest default rate. |

### 3.4 Phase 4: Strategic Recommendations
A technical risk model is only useful when its output is translated into action. This phase requires you to think like a credit-risk and customer strategist.

* Investigate the Credit Utilisation and Exposure Hypothesis: Some customers may be using a large percentage of their available credit limit and showing weak repayment behaviour. Create a histogram or box plot of credit utilisation, outstanding balance, and credit limit across customer segments.
* Examine whether customers with high utilisation and poor repayment behaviour contribute disproportionately to default risk. Comment on this finding, because the strategy for a customer using most of their available credit may differ from the strategy for a customer with low utilisation.
* Translate each segment into specific, fair, and transparent actions such as reminders, credit-limit reviews, payment restructuring, or financial-wellness support.

### 3.5 Phase 5: Data Enrichment via API Integration
This task lets your team demonstrate advanced skills in data acquisition and integration, as covered in Module 8.

* Select a free, public finance API (for example a currency-exchange API such as ExchangeRate-API, or the World Bank indicators API).
* Using the `requests` library, fetch relevant external data — for instance exchange rates to convert credit limits and balances into a reporting currency (USD/EUR) or a macroeconomic indicator such as inflation or lending interest rate for context.
* Apply the result to your dataset (for example creating `LIMIT_BAL_USD`) or summarise it alongside your analysis.
* In a dedicated notebook section, document the API used, show a sample JSON response, handle errors gracefully, and explain the business value of the enrichment (e.g. reporting to international stakeholders or relating risk to macroeconomic conditions).
* Do not embed any private API keys in the notebook.

---

## Part 4: Final Deliverables
Your submission will consist of a professional business report, and a technical appendix as summarised below. You should also submit the cleaned dataset file.

| Deliverable | Format | Requirement |
| :--- | :--- | :--- |
| **Strategic Insights Report** | PDF | **2,500-3,000 words (strict)**, written for the executive board. Insight-led, not a code dump; visualizations support the narrative. Excludes title page, contents, and appendix from the word count.<br><br>**Your report should begin with an Executive Summary.** It should consist of a concise, one-page overview of the project's purpose, your most critical findings, and your top three strategic recommendations. This is the most important page of the report.<br><br>After that, you should describe all the processes and methodologies you followed, the results you obtained, and the conclusions you reached for each phase from Phase 1 to Phase 5 (i.e., Sections 3.1 to 3.5). |
| **Technical Appendix** | Jupyter Notebook (.ipynb) | A single, well-organised, reproducible notebook following the phased structure, with Markdown explaining the rationale behind each step. Must run top-to-bottom without errors.<br><br>After each cell, the corresponding output should be clearly visible, including any plots, tables, and other generated results. |
| **Cleaned Dataset** | CSV/Excel | The cleaned dataset as a CSV or Excel file. |

### 4.1 The Strategic Insights Report (PDF)
This is the primary deliverable for the executive board. It must be polished and written in clear business language — not a code report. The focus must be on insights and recommendations, with visualizations used to support your narrative. Required structure:

1. **Executive Summary** - a concise one-page overview of purpose, most critical findings, and top three recommendations.
2. **Introduction** - the business problem and project objectives as defined in the mandate.
3. **Data & Methodology** - the dataset and the key steps of your cleaning and feature-engineering approach (Phase 1).
4. **Credit Risk & Repayment Analysis** - your EDA findings (Phase 2), illustrated with your best visualizations.
5. **Customer Segmentation & Strategy** - the core analytical section: segment sizes, characteristics, default rates, and the high-exposure findings (Phases 3-4).
6. **Data Enrichment via API** - the results and business value of your API integration (Phase 5).
7. **Recommendations, Ethics & Limitations** - actionable strategy, responsible-analytics considerations, and limitations.

### 4.2 The Technical Appendix (Jupyter Notebook)
Submit a single, well-organised notebook containing all the Python code used for your analysis. It serves as the technical documentation for your project and must be reproducible.

* **Clean & Logical Flow:** Follow the phased structure of the project (Phase 1 to Phase 5).
* **Well-Documented Code:** Code cells must be accompanied by Markdown explaining why each step is performed and how outputs are interpreted.
* **Reproducibility:** The notebook must run from top to bottom without errors, assuming the data file is in the correct location.

---

## Part 5: Assessment & Evaluation Rubric

### 5.1 Grading Breakdown
The project is graded out of 100 points. Note that marks are not awarded purely on a group-wise basis: the report, notebook, and teamwork are all assessed in light of individual performance in the final presentation and viva, so members of the same group may receive different marks.

| Component | Weight |
| :--- | :--- |
| Strategic Insights Report (PDF) | 50% |
| Technical Appendix (Jupyter Notebook) | 40% |
| Teamwork & Collaboration (Peer Evaluation) | 10% |

### 5.2 Detailed Rubric
Your submission will be evaluated against the following criteria to ensure a fair and transparent assessment process.

| Criteria | Wt. | Unsatisfactory (0-49%) | Satisfactory (50-69%) | Good (70-89%) | Excellent (90-100%) |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Code Quality & Correctness** | 20% | Non-functional, major logical errors, disorganised, or signs of plagiarism. | Runs but is Inefficient, poorly structured, and lightly commented. | Functional, mostly correct, with documentation explaining its purpose. | Efficient, well-structured, extensively documented, follows PEP 8 best practices. |
| **Data Cleaning & Preprocessing** | 10% | Fails to handle nulls, duplicates, invalid codes, or categories. | Basic cleaning done but with little justification or analysis. | All required steps completed correctly with adequate justification. | Thorough, expertly justified, and mindful of biases introduced by cleaning. |
| **Data Analysis & Depth** | 20% | Superficial or contains significant calculation/interpretation errors. | Basic totals/averages only; lacks depth or business connection. | Correct EDA and risk analysis with valid, logical conclusions. | Deep and insightful; surfaces non-obvious patterns and strong, data-backed hypotheses. |
| **Data Visualization** | 10% | Missing, misleading, or unreadable plots. | Plots generated but poorly labelled and cluttered. | Clear, correctly labelled, titled, and appropriate plots. | Compelling, polished visuals that tell a story for a non-technical audience. |
| **Report & Communication** | 30% | Poorly written, unstructured; reads like a code dump. | Summarises notebook output with little business context or strategy. | Well-structured; communicates findings with linked recommendations. | Persuasive, professional narrative with a strong summary and specific, actionable recommendations. |
| **Teamwork & Collaboration** | 10% | No evidence of collaboration; work clearly siloed. | Minimal collaboration; contributions highly unbalanced. | Clear shared work and meaningful contributions from all members. | Excellent, integrated collaboration with balanced, high-quality contributions. |

---

## Part 6: Submission Guidelines
* **Group size:** 4-5 students unless otherwise approved by the lecturer.
* **Submission package:** One compressed folder named `ITS2122_GroupName_FinanceProject.zip`. Contents: Report PDF, Jupyter Notebook (.ipynb), and the cleaned dataset file.
* **Academic integrity:** All datasets, APIs, external code, and AI-assisted work must be acknowledged. Plagiarism or fabricated outputs may result in zero marks for the affected component.
* **Late submission:** Follows the institute / module late-submission policy announced by the lecturer.
* **Viva readiness:** Each student must be prepared to explain any part of the notebook during the final viva.

### References
[1] UCI Machine Learning Repository
Default of Credit Card Clients:
https://archive.ics.uci.edu/dataset/350/default+of+credit+card+clients

[2] pandas Documentation: https://pandas.pydata.org/docs/
[3] NumPy Documentation: https://numpy.org/doc/
[4] Matplotlib Documentation: https://matplotlib.org/stable/
[5] seaborn Documentation: https://seaborn.pydata.org/
[6] Requests Documentation: https://requests.readthedocs.io/

*IJSE Institute of Software Engineering*
