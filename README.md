
# HR Employee Attrition Analysis 📊

A comprehensive data analytics project examining employee attrition patterns across 1,470 employees, providing strategic insights and actionable recommendations to reduce turnover costs.

![Project Status](https://img.shields.io/badge/Status-Complete-success)
![SQL](https://img.shields.io/badge/SQL-MySQL-blue)
![Analysis](https://img.shields.io/badge/Analysis-HR%20Analytics-orange)

## 📋 Table of Contents
- [Project Overview](#project-overview)
- [Key Findings](#key-findings)
- [Technologies Used](#technologies-used)
- [Dataset](#dataset)
- [Analysis Process](#analysis-process)
- [Dashboard Highlights](#dashboard-highlights)
- [Strategic Recommendations](#strategic-recommendations)
- [Project Files](#project-files)
- [Key Insights](#key-insights)
- [Business Impact](#business-impact)
- [Contact](#contact)

## 🎯 Project Overview

This project analyzes employee attrition data to identify patterns, drivers, and risk factors contributing to employee turnover. The analysis provides data-driven insights to help HR and leadership make informed decisions on retention strategies.

**Project Goals:**
- Understand the scale and rate of employee attrition
- Identify demographic and role-specific patterns
- Analyze work environment and compensation factors
- Quantify the financial impact of attrition
- Provide actionable strategic recommendations

## 🔍 Key Findings

### Overall Metrics
- **Total Employees:** 1,470
- **Attrition Rate:** 16%
- **Employees Lost:** 237
- **Retention Rate:** 83%
- **Estimated Annual Cost:** $16.5 million

### Critical Insights

#### 1. **Role-Specific Attrition (Highest Risk)**
- Sales Representatives: **39.8%** attrition
- Laboratory Technicians: **23.9%** attrition
- Human Resources: **23.1%** attrition
- Sales Executives: **17.5%** attrition

#### 2. **Overtime Impact (Most Significant Factor)**
- Employees working overtime: **30.5%** attrition
- Employees without overtime: **10.4%** attrition
- **Risk Factor:** Nearly 3x higher attrition with overtime

#### 3. **Tenure Analysis**
- 0-4 years (Early Career): **31.8%** attrition
- 5-9 years: **15.6%** attrition
- 10-14 years: **11.8%** attrition
- 15+ years: **<10%** attrition

#### 4. **Compensation Impact**
- Low Income (<$3,000/month): **23.5%** attrition
- Mid Income ($3,000-$7,000): **14.8%** attrition
- High Income (>$7,000): **8.6%** attrition

#### 5. **Career Stagnation**
- Never Promoted (>1 year): **25.0%** attrition
- Long Overdue (9+ years): **23.5%** attrition
- Recently Promoted (1-3 years): **10.0%** attrition

#### 6. **Department Breakdown**
- Sales: **20.6%** attrition (92 employees lost)
- Human Resources: **19.0%** attrition (12 employees lost)
- Research & Development: **16.1%** attrition (133 employees lost)

## 🛠 Technologies Used

- **SQL (MySQL):** Data cleaning, transformation, and analysis
- **Data Visualization:** Power BI / Tableau (for dashboard creation)
- **Microsoft PowerPoint:** Executive presentation
- **Microsoft Word:** Detailed recommendations report
- **GitHub:** Version control and project documentation

## 📊 Dataset

The dataset includes 1,470 employee records with the following attributes:

### Demographic Information
- Age, Gender, Marital Status
- Education Level, Education Field
- Distance from Home

### Employment Details
- Department, Job Role, Job Level
- Monthly Income, Hourly Rate, Daily Rate
- Years at Company, Years in Current Role
- Years Since Last Promotion
- Years with Current Manager

### Work Environment Metrics
- Overtime Status
- Job Satisfaction (1-4 scale)
- Environment Satisfaction (1-4 scale)
- Work-Life Balance (1-4 scale)
- Relationship Satisfaction (1-4 scale)
- Job Involvement (1-4 scale)

### Performance Indicators
- Performance Rating
- Percent Salary Hike
- Training Times Last Year
- Stock Option Level

## 📈 Analysis Process

### 1. Data Cleaning & Preparation
```sql
-- Created working table and cleaned column names
CREATE TABLE attrition2 LIKE attrition;
INSERT attrition2 SELECT * FROM attrition;

-- Standardized column naming conventions
ALTER TABLE attrition2 RENAME COLUMN MonthlyIncome TO Monthly_Income;
-- (Applied to all 30+ columns for consistency)

-- Removed redundant columns
ALTER TABLE attrition2
DROP COLUMN EmployeeCount,
DROP COLUMN Over18,
DROP COLUMN Standard_Hours;
```

### 2. Exploratory Data Analysis
- Calculated overall attrition rate
- Analyzed attrition by department, role, and demographics
- Examined work environment factors
- Investigated compensation and career progression patterns

### 3. Key Analysis Queries

**Attrition by Department:**
```sql
SELECT Department,
    COUNT(*) AS Total,
    SUM(CASE WHEN Attrition='Yes' THEN 1 ELSE 0 END) Employees_Left,
    ROUND(SUM(CASE WHEN Attrition='Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) Attrition_Rate
FROM attrition2
GROUP BY Department
ORDER BY Attrition_Rate DESC;
```

**Overtime Impact:**
```sql
SELECT Over_Time,
    ROUND(SUM(CASE WHEN Attrition='Yes' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) Attrition_Rate
FROM attrition2
GROUP BY Over_Time
ORDER BY Over_Time DESC;
```

**Promotion Analysis:**
```sql
SELECT Promotion_Status,
    COUNT(*) Total_Employees,
    SUM(CASE WHEN Attrition='Yes' THEN 1 ELSE 0 END) Employee_Left,
    ROUND(100.0 * SUM(CASE WHEN Attrition='Yes' THEN 1 ELSE 0 END) / COUNT(*), 2) Attrition_Rate
FROM (
    SELECT 
        CASE 
            WHEN Years_At_Company <= 1 THEN 'New Hire'
            WHEN Years_Since_Last_Promotion = 0 AND Years_At_Company > 1 THEN 'Never Promoted (1 yr)'
            WHEN Years_Since_Last_Promotion BETWEEN 1 AND 3 THEN 'Recently Promoted (1-3 yrs)'
            WHEN Years_Since_Last_Promotion BETWEEN 4 AND 8 THEN 'Promotion Gap (4-8 yrs)'
            WHEN Years_Since_Last_Promotion >= 9 THEN 'Long Overdue (9+ yrs)'
            ELSE 'Unknown'
        END AS Promotion_Status,
        Attrition 
    FROM attrition2
) AS GROUPED
GROUP BY Promotion_Status
ORDER BY Attrition_Rate DESC;
```

## 📊 Dashboard Highlights

The interactive dashboard includes:

### Page 1: Overview & Demographics
- Key metrics cards (Total Employees, Attrition Rate, Retention Rate)
- Attrition by Age Group & Gender
- Attrition by Department
- Employee Count by Job Role
- Work-Life Balance distribution
- Job Involvement levels
- Attrition by Job Level & Monthly Income
- Education Level distribution

### Page 2: Deep Dive Analysis
- Attrition by Years at Company
- Attrition by Overtime Status
- Years Since Last Promotion analysis
- Relationship Satisfaction breakdown
- Environment Satisfaction levels
- Attrition by Distance from Home
- Job Satisfaction distribution

**Filtering Capabilities:**
- Marital Status
- Education Level
- Job Level

## 💡 Strategic Recommendations

### 1. Redesign Sales Compensation & Career Path (CRITICAL)
**Problem:** Sales Representatives face 39.8% attrition
**Solutions:**
- Implement tiered commission structure with performance bonuses
- Create clear promotion pathway: Sales Rep → Senior Rep → Team Lead → Manager
- Establish 6-month performance reviews with development plans
- **Target:** Reduce Sales Rep attrition from 39.8% to <25%
- **Investment:** $850,000 annually
- **ROI:** 140-180% in first year

### 2. Mandate Overtime Management Program (CRITICAL)
**Problem:** Overtime workers show 30.5% attrition vs 10.4% without overtime
**Solutions:**
- Cap overtime hours at 10 hours/week with manager approval
- Hire temporary staff during peak periods
- Implement workload balancing across teams
- **Target:** Reduce overtime-driven attrition from 30.5% to <15%
- **Investment:** $500,000 annually
- **ROI:** 240-300% in first year

### 3. Launch Enhanced Onboarding & Early Career Support (HIGH PRIORITY)
**Problem:** Early career employees (0-4 years) show 31.8% attrition
**Solutions:**
- Extend onboarding from 2 weeks to 90-day structured program
- Assign mentors for all new hires in first year
- Monthly check-ins during first 12 months
- **Target:** Reduce 0-4 year tenure attrition from 31.8% to <20%
- **Investment:** $350,000 annually
- **ROI:** 470-550% in first year

### 4. Establish Promotion Timeline Standards (HIGH PRIORITY)
**Problem:** Employees not promoted in >1 year show 25% attrition
**Solutions:**
- Set clear promotion timelines by role (18-36 months based on performance)
- Conduct annual talent reviews to identify promotion-ready employees
- Create transparent criteria for advancement
- **Target:** Reduce never-promoted attrition from 25% to <15%
- **Investment:** $250,000 annually
- **ROI:** 420-580% in first year

## 📁 Project Files


hr-attrition-analysis/


 [README.md](https://github.com/user-attachments/files/24980419/README.md)    
        
        # Project documentation

[hr_employee_attrition.sql](https://github.com/user-attachments/files/24980429/hr_employee_attrition.sql)     
        
        # Complete SQL analysis queries


dashboards

[HR_Attrition_Dashboard_Page1][View the dashboard](https://github.com/user-attachments/assets/77e60979-1b96-4e3b-9cad-65e4d5de0823)

      # Overview dashboard
      
[HR_Attrition_Dashboard_Page2][View the dashboard](https://github.com/user-attachments/assets/fbb174fa-86c5-4fe9-b06b-c305c02972c3)

      # Deep dive dashboard


 reports
 
[HR_Attrition_Executive_Review.pptx](https://github.com/user-attachments/files/24980425/HR_Attrition_Executive_Review.pptx)     

    # 12-slide executive presentation

[HR_Attrition_Detailed_Recommendations.docx](https://github.com/user-attachments/files/24980424/HR_Attrition_Detailed_Recommendations.docx) 
    
    # Comprehensive report (20+ pages)


data

 [data_dictionary.md](https://github.com/user-attachments/files/24980432/data_dictionary.md)                     
       
        # Dataset documentation
```

## 💼 Key Insights

### What Drives Attrition?

1. **Excessive Overtime** is the single biggest risk factor (3x attrition increase)
2. **Career Stagnation** drives 25% attrition among non-promoted employees
3. **Low Compensation** (<$3,000/month) shows 2.7x higher attrition than high earners
4. **Early Career Period** (0-4 years) is the highest risk window
5. **Sales Roles** experience structural challenges with nearly 40% turnover

### Who is Most at Risk?

- Sales Representatives and Laboratory Technicians
- Employees working overtime regularly
- New hires in first 4 years
- Employees earning <$3,000/month
- Employees not promoted in >1 year
- Single employees (25.5% attrition)
- Younger workforce (18-35 age range)

## 📊 Business Impact

### Current State
- **Annual Attrition Cost:** $16.5 million
- **Employees Lost Annually:** 237
- **Average Replacement Cost:** $57,444 per employee
- **Critical Knowledge Loss:** 133 R&D employees, 92 Sales employees

### Projected Impact (with Recommendations)
- **Attrition Rate Reduction:** 16% → 13% (3% decrease)
- **Annual Cost Savings:** $4.5M - $6.5M
- **Employees Retained:** 90-110 additional per year
- **Total Investment Required:** $2M annually
- **Overall ROI:** 225-325% in first year

### Implementation Timeline
- **Phase 1 (Weeks 1-4):** Immediate actions and approvals
- **Phase 2 (Weeks 5-12):** Foundation building
- **Phase 3 (Weeks 13-24):** Full deployment
- **Phase 4 (Weeks 25-52):** Optimization and scale

## 📧 Contact

**Project Author:** 
Favour Chegwe
Financial Data Analyst

For questions, feedback, or collaboration opportunities:
- GitHub: [favouritefavil](https://github.com/favouritefavil)
- LinkedIn:[Chegwe Favour](http://www.linkedin.com/in/favour-chegwe)
- Email: favourchegwec@gmail.com


**⭐ If you found this project helpful, please give it a star!**

*Last Updated: January 2026*
