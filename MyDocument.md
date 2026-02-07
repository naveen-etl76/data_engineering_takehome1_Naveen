# NYC Jobs Dataset ETL Pipeline Documentation

## Learnings
- **Data Profiling**: Exploring the schema revealed mixed data types (numeric, categorical, free-text, and dates). Profiling was critical to decide which columns needed casting and which required text parsing.
- **Salary Normalization**: Converting hourly, weekly, and daily salaries into annual equivalents was essential for fair comparisons across postings.
- **Regex for Education Requirements**: Degree levels were embedded in free-text fields. Regex-based extraction provided a workable solution but highlighted the complexity of unstructured data.
- **Skills Extraction**: Splitting skills from free-text into individual tokens allowed analysis of demand and salary correlation by skill.

## Challenges
- **Inconsistent Text Formats**: Minimum qualification requirements and preferred skills varied widely in phrasing, making parsing error-prone.
- **Multiple Pathways**: Some postings listed degree OR experience requirements, complicating categorization.
- **Data Quality Issues**: Missing values, inconsistent salary frequencies, and non-standard date formats required careful handling.
- **Exploding Skills**: Skills extraction created duplicate rows per job posting, which needed aggregation for KPI analysis.

## Considerations
- **Normalization Assumptions**: 
  - 2080 hours/year for hourly salaries
  - 52 weeks/year for weekly salaries
  - 260 working days/year for daily salaries
- **Upper Bound Salary**: Used `Salary Range To` for normalization, assuming it reflects the maximum potential pay.
- **Recent Data Filtering**: For KPIs like average salary per agency (last 2 years), relied on `Posting_Year` derived from `Posting Date`.

## Assumptions
- **Posting Date Format**: Assumed ISO 8601 (`yyyy-MM-dd'T'HH:mm:ss.SSS`) for parsing.
- **Degree Categorization**: Regex captures common degree keywords; postings without clear matches default to "Other".
- **Skills Delimiters**: Assumed skills are separated by bullets, commas, or semicolons.

## Deployment Notes
- **Pipeline**: Implemented in PySpark with modular functions for preprocessing, feature engineering, and KPI analysis.
- **Storage**: Processed data saved in Parquet for efficient querying.
- **Scheduling**: Airflow DAG orchestrates daily runs.
- **Testing**: PyTest cases validate salary normalization, degree categorization, and skill extraction.

## Insights
- Job postings are concentrated in a handful of categories.
- Salary distributions vary significantly across categories.
- Higher degrees (Master/Doctorate) generally correlate with higher salaries.
- Certain agencies consistently post the highest-paying jobs.
- Skills like data analysis, project management, and specialized technical expertise are linked to higher salaries.

## Future Improvements
- **NLP for Skills**: Use natural language processing to better extract and standardize skills.
- **Salary Prediction Models**: Train ML models to predict salary ranges based on job features.
- **Interactive Dashboards**: Build dashboards (e.g., with Plotly or Power BI) for dynamic KPI visualization.
