# Data Dictionary - HR Employee Attrition Dataset

## Overview
This dataset contains information about 1,470 employees, including demographic details, employment information, compensation data, and attrition status.

---

## Dataset Structure

**Total Records:** 1,470 employees  
**Target Variable:** Attrition (Yes/No)  
**Features:** 35+ attributes across demographics, employment, compensation, and satisfaction metrics

---

## Data Fields

### Demographics

| Field Name | Data Type | Description | Values/Range |
|------------|-----------|-------------|--------------|
| **Age** | Integer | Employee's age in years | 18-60 |
| **Gender** | Categorical | Employee's gender | Male, Female |
| **Marital_Status** | Categorical | Marital status of employee | Single, Married, Divorced |
| **Distance_From_Home** | Integer | Distance from home to workplace (miles) | 1-29 |

### Education

| Field Name | Data Type | Description | Values/Range |
|------------|-----------|-------------|--------------|
| **Education** | Ordinal | Education level | 1 = Below College<br>2 = College<br>3 = Bachelor<br>4 = Master<br>5 = Doctor |
| **Education_Field** | Categorical | Field of education | Life Sciences, Medical, Marketing, Technical Degree, Human Resources, Other |

### Employment Details

| Field Name | Data Type | Description | Values/Range |
|------------|-----------|-------------|--------------|
| **Department** | Categorical | Department where employee works | Sales, Research & Development, Human Resources |
| **Job_Role** | Categorical | Specific job role | Sales Executive, Research Scientist, Laboratory Technician, Manufacturing Director, Healthcare Representative, Manager, Sales Representative, Research Director, Human Resources |
| **Job_Level** | Ordinal | Hierarchical job level | 1-5 (1 = Entry, 5 = Executive) |
| **Years_At_Company** | Integer | Total years with the company | 0-40 |
| **Years_In_Current_Role** | Integer | Years in current position | 0-18 |
| **Years_Since_Last_Promotion** | Integer | Years since last promotion | 0-15 |
| **Years_With_Curr_Manager** | Integer | Years working with current manager | 0-17 |
| **Total_Working_Years** | Integer | Total professional work experience | 0-40 |
| **Num_Companies_Worked** | Integer | Number of previous employers | 0-9 |
| **Training_Times_Last_Year** | Integer | Training sessions attended last year | 0-6 |

### Compensation

| Field Name | Data Type | Description | Values/Range |
|------------|-----------|-------------|--------------|
| **Monthly_Income** | Integer | Monthly salary in dollars | $1,009 - $19,999 |
| **Hourly_Rate** | Integer | Hourly pay rate | $30 - $100 |
| **Daily_Rate** | Integer | Daily pay rate | $102 - $1,499 |
| **Monthly_Rate** | Integer | Monthly rate (alternative measure) | $2,094 - $26,999 |
| **Percent_Salary_Hike** | Integer | Last salary increase percentage | 11% - 25% |
| **Stock_Option_Level** | Ordinal | Stock option tier | 0-3 (0 = None, 3 = Highest) |

### Work Environment

| Field Name | Data Type | Description | Values/Range |
|------------|-----------|-------------|--------------|
| **Over_Time** | Binary | Works overtime regularly | Yes, No |
| **Business_Travel** | Categorical | Travel frequency | Non-Travel, Travel_Rarely, Travel_Frequently |

### Satisfaction Metrics
*(All rated on 1-4 scale: 1 = Low, 4 = Very High)*

| Field Name | Data Type | Description | Scale |
|------------|-----------|-------------|-------|
| **Environment_Satisfaction** | Ordinal | Satisfaction with work environment | 1-4 |
| **Job_Satisfaction** | Ordinal | Overall job satisfaction | 1-4 |
| **Relationship_Satisfaction** | Ordinal | Satisfaction with workplace relationships | 1-4 |
| **Work_Life_Balance** | Ordinal | Work-life balance rating | 1-4 |
| **Job_Involvement** | Ordinal | Level of job engagement | 1-4 |

### Performance

| Field Name | Data Type | Description | Values/Range |
|------------|-----------|-------------|--------------|
| **Performance_Rating** | Ordinal | Performance evaluation score | 1-4 (3 = Excellent, 4 = Outstanding) |

### Target Variable

| Field Name | Data Type | Description | Values |
|------------|-----------|-------------|--------|
| **Attrition** | Binary | Employee left the company | Yes, No |

---

## Data Cleaning Notes

### Columns Removed
The following columns were removed during analysis as they contained no variance or redundant information:

- **EmployeeCount:** All values = 1 (redundant)
- **Over18:** All values = 'Y' (no variance)
- **Standard_Hours:** All values = 80 (no variance)
- **Employee_Number:** Unique identifier (not needed for analysis)

### Column Transformations
- All column names standardized to snake_case format for SQL consistency
- Special character encoding issues resolved (e.g., `Ã¯Â»Â¿Age` → `Age`)
- Over18 values standardized (Y/Yes → Yes, N/No → No) before removal

---

## Key Statistics

### Attrition Metrics
- **Overall Attrition Rate:** 16%
- **Employees Who Left:** 237
- **Employees Retained:** 1,233
- **Retention Rate:** 83%

### Department Distribution
- Research & Development: ~65% of workforce
- Sales: ~30% of workforce
- Human Resources: ~5% of workforce

### Salary Distribution
- **Median Monthly Income:** $4,787
- **Average (Attrited Employees):** $4,787
- **Salary Range:** $1,009 - $19,999

### Tenure Distribution
- **Average Years at Company:** 7 years
- **Average (Attrited Employees):** 5 years
- **Longest Tenure:** 40 years

---

## Data Quality

### Completeness
- **Missing Values:** None detected
- **Duplicate Records:** None (verified by Employee_Number)
- **Data Consistency:** All categorical values validated

### Data Validation
✅ No null values in any field  
✅ All numeric fields within expected ranges  
✅ All categorical fields contain valid categories  
✅ Date-related fields (years) are logical and consistent  

---

## Usage Notes

### For Analysis
- Use `Attrition` as the target variable for predictive modeling
- Satisfaction metrics (1-4 scale) can be grouped as Low (1), Medium (2-3), High (4)
- Income brackets: Low (<$3,000), Mid ($3,000-$7,000), High (>$7,000)
- Tenure groups: 0-4 years, 5-9 years, 10-14 years, 15+ years

### Calculated Fields Used in Analysis
```sql
-- Age Groups
CASE 
    WHEN Age BETWEEN 18 AND 25 THEN '18-25'
    WHEN Age BETWEEN 26 AND 35 THEN '26-35'
    WHEN Age BETWEEN 36 AND 45 THEN '36-45'
    WHEN Age BETWEEN 46 AND 55 THEN '46-55'
    ELSE '56+'
END AS Age_Group

-- Income Brackets
CASE 
    WHEN Monthly_Income < 3000 THEN 'Low Income'
    WHEN Monthly_Income BETWEEN 3000 AND 7000 THEN 'Mid Income'
    ELSE 'High Income'
END AS Income_Bracket

-- Promotion Status
CASE 
    WHEN Years_At_Company <= 1 THEN 'New Hire'
    WHEN Years_Since_Last_Promotion = 0 AND Years_At_Company > 1 THEN 'Never Promoted (>1 yr)'
    WHEN Years_Since_Last_Promotion BETWEEN 1 AND 3 THEN 'Recently Promoted (1-3 yrs)'
    WHEN Years_Since_Last_Promotion BETWEEN 4 AND 8 THEN 'Promotion Gap (4-8 yrs)'
    WHEN Years_Since_Last_Promotion >= 9 THEN 'Long Overdue (9+ yrs)'
END AS Promotion_Status
```

---

## Data Source

**Dataset Name:** HR Employee Attrition  
**Format:** CSV / SQL Database  
**Size:** 1,470 records × 35 attributes  
**Domain:** Human Resources Analytics  

---

## Related Files

- `hr_employee_attrition.sql` - Complete SQL analysis queries
- `README.md` - Project overview and findings
- Dashboard visualizations in `/dashboards` folder

---

*Last Updated: January 2026*
