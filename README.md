# Employee Attendance Dashboard

The **Employee Attendance Dashboard** is an Excel-based analytical solution designed to automate attendance tracking, working hour calculation, and payroll estimation using employee clock-in and clock-out data. The system converts raw attendance records into an interactive dashboard that helps HR and management monitor workforce productivity, payout distribution, and attendance irregularities.

Many organizations rely on manual Excel sheets to manage attendance, which often leads to calculation errors, inconsistent records, and difficulty in analyzing employee behavior. This project addresses those issues by providing an automated and visually driven dashboard built entirely in Microsoft Excel.


## Project Purpose

The dashboard was developed to:

-   Automatically calculate total working hours from punch data
-   Identify late check-ins, early check-outs, and missed punches
-   Compare actual payout vs expected payout
-   Analyze attendance by employee, gender, and day
-   Provide an interactive two-page reporting interface

## The system uses attendance data with the following fields:

Sr no | Name | Gender | Date | Timetable | On duty | Off duty | Clock In | Clock Out

From these columns, the dashboard derives:
-   Working hours per day
-   Full day / half day classification
-   Extra hours worked
-   Attendance status (Present/Absent)
-   Payroll and deduction values

## Dashboard Overview

####  Page 1 – Organizational Summary

The first page presents high-level KPIs such as:
-   Total Employees
-   Total Working Hours
-   Total Payout vs Expected Payout
-   Deductions
-   Gender-wise attendance
-   Day-wise presence trends
-   Employee ranking by attendance
   
####  Page 2 – Employee Detail Analysis

The second page allows drill-down using:
-   Employee and Month filters
-   Extra hours worked
-   Late check-ins / early check-outs
-   Missed punches
-   Day-wise attendance distribution
-   Leave type analysis
----------

## Outcome

This dashboard enables:
-   Faster payroll processing
-   Better visibility of employee behavior
-   Reduction in manual effort
-   Data-driven HR decisions 

The project demonstrates how Excel can be used as a powerful analytics tool without relying on external BI software.

## Methodology & Calculations

The dashboard was developed by transforming raw attendance logs into meaningful analytical measures using Excel formulas and pivot-based modeling. Since the dataset only contains punch records, several custom columns were created to compute working behaviour and payroll metrics.

#### 1)  Data Cleaning & Preparation
The raw attendance dataset was first processed using Power Query in Excel to ensure data accuracy and consistency before building the dashboard. Column health and data quality were reviewed to detect blank values, inconsistent formats, and invalid records. Duplicate entries were identified and removed to prevent double counting of attendance.

Time-related columns such as On Duty, Off Duty, Clock In, and Clock Out were converted and corrected to the custom format **[h]:mm** to enable proper calculation of working hours exceeding 24 hours. Logical validation was applied to ensure that Clock Out was always later than Clock In, and missing punches were flagged for further analysis.

Different Excel formulas were applied during transformation and calculation, including:

-   **IF** – to classify attendance and irregularities
    
-   **AND / OR** – to validate multiple conditions
    
-   **COUNTIFS** – to count attendance types
    
-   **SUMIFS** – to total working hours and payouts
    

This process ensured that only clean and structured data was loaded into the dashboard model.

#### 2) Custom Columns Creation
Several derived columns were added to convert raw punches into meaningful measures:

#### a) Working Hours
Calculated as the difference between clock times:
-   **Working Hours = Clock Out – Clock In**

This represents actual time spent at work for each day.

#### b) Attendance Status

-   Marked as **Present** when both punches exist
-   Marked as **Absent** when no valid punch   
    

#### c) Extra Hours

-   If Working Hours exceed standard shift → counted as Extra Hours   

#### d) Payout & Deduction

-   Actual Payout = Working Hours × Hourly Rate
    
-   Expected Payout = Standard Hours × Rate
    
-   Deduction = Expected – Actual
    
These custom columns drive all KPIs shown in the dashboard.

#### 3) Calculations & Formula Logic

- Working Hours =IFS(AND(ISBLANK(I2)=FALSE,ISBLANK(J2)=FALSE),J2-I2,AND(ISBLANK(I2)=TRUE,ISBLANK(J2)=TRUE),"",AND(ISBLANK(I2)=FALSE,ISBLANK(J2)=TRUE),I2+H2)

- Extra time worked =IF(AND(ISNUMBER([@[Working_Hours]])=TRUE,[@[Working_Hours]]>[@[Days_Hours]]),[@[Working_Hours]]-[@[Days_Hours]],"")

- Punch =IF(AND(ISNUMBER([@[Clock In]])=TRUE,ISNUMBER([@[Clock Out]])=FALSE),"Missed Punch",IF(AND(ISNUMBER([@[Clock In]])=FALSE,ISNUMBER([@[Clock Out]])=FALSE),"",IF([@[Clock In]]>=[@[On duty]]+TIME(0,5,0),"Late Check-In",IF([@[Clock Out]]<=[@[Off duty]]-TIME(0,5,0),"Early Check-Out",""))))

- Payout =IFS(ISNUMBER(K2)=TRUE,K2*$W$17*24,ISNUMBER(K2)=FALSE,0*$W$17)

- Expected Payout =H2*24*$W$17

- **Column Design Note** : To improve usability and transparency, all custom columns created using Excel formulas are highlighted in ORANGE color in the worksheet. This visual distinction helps users:

			a)  Easily identify which fields contain formulas 
			b) Avoid accidental manual editing
			c) Understand that these columns are automatically calculated
			d) Safely update only the raw input fields



#### 4) Workflow Summary

1.  Import raw attendance data
    
2.  Perform cleaning and validation
    
3.  Generate custom calculated columns
    
4.  Step by step calculations using formulas
    
5.  Design two dashboard pages
    
This methodology enables automatic updates any new attendance entry immediately reflects in all KPIs and visuals.

## **Assumptions & Business Rules**

The following rules were considered while calculating attendance status and working hours:

#### 1) Grace Time Rule (5 Minutes)

-   If an employee checks in within 5 minutes of the On Duty time, it is treated as **On Time** and no late punch is recorded.
    
-   If an employee checks out within 5 minutes before Off Duty time, it is considered a valid checkout and no early exit is marked.

#### 2) Late Check-In 
If an employee checks in more than 5 minutes after the On Duty time, the record is marked as Late Punch In.

#### 3) Early Check-Out
If an employee leaves more than 5 minutes before the Off Duty time, the record is marked as Early Check Out.

#### 4) Missed Punch
If an employee checks in but forgets to check out, the record is marked as Missed Punch.

#### 5) Hour Allocation for Missed Punch

-  In case of a missed punch, the system assumes the employee completed the standard shift.
-   The defined shift duration (e.g., 8 hours) is added to the Clock In time to generate an estimated Clock Out time.
-   Payment for that day is calculated based on this allotted shift duration.

## Dashboard Overview

The Employee Attendance Dashboard consists of **two interactive pages** designed to provide both organizational-level insights and employee-level drill-down analysis. All visuals are dynamically linked to the processed attendance dataset and update automatically based on slicer selections.

### Page 1 – Organizational Summary Dashboard

The first page presents a high-level overview of the entire workforce performance and payroll status.

<img 
  src= "https://github.com/krutik-dhabaliya/Employee_Attendance_Dashboard_Excel/blob/main/Dashboard/P1.png"
  width="80%"
/>

A button is provided to move to the Detail Analysis Page (Page 2) for deeper investigation.

### Page 2 – Employee Detail Analysis

This page focuses on individual employee performance with interactive filters.

<img 
  src= "https://github.com/krutik-dhabaliya/Employee_Attendance_Dashboard_Excel/blob/main/Dashboard/P2.png"
  width="80%"
/>

### Dashboard Functionality
-   Automatic recalculation when new data is added
-   Visual alerts for irregular attendance
-   Clear comparison between payout and expected cost
   
The two-page design ensures both strategic overview for management and detailed operational analysis for HR.

## Results & Insights

The Employee Attendance Dashboard provides clear visibility into workforce performance and payroll impact. Based on the processed data and dashboard metrics, the following key insights were observed:

#### 1) Workforce Attendance Performance
-   **Full-day attendance accounts for 88.26%** of total working hours, while **11.74%** corresponds to half-day records. 

-   Certain employees show significantly higher attendance consistency, which is reflected in the employee ranking chart.

#### 2) Payroll & Cost Analysis

-   **Actual Payout:** $24,973.30
-   **Expected Payout:** $20,628.40
-   **Total Deduction:** $4,344.89

The gap between actual and expected payout is mainly influenced by:

-   Early check-outs
-   Half-day attendance
-   Missed punches
-   Variations in working hours

This comparison helps management understand payroll deviations and control labor costs.

#### 3) Attendance Irregularities

From the employee detail page:

-   Instances of Early Check-Outs and Missed Punches were identified
-   Extra hours worked by certain employees were recorded (e.g., 83:36 hours)
-   Late check-ins were minimal due to the 5-minute grace rule

These indicators help HR take corrective actions and improve discipline.

#### 4) Operational Benefits

The dashboard enabled:
-   Quick identification of low attendance employees
-   Transparent payroll calculation
-   Reduction in manual reconciliation effort
-   Data-driven decision making for HR and management

## Limitations & Future Enhancements

###  1. Limitations
Although the dashboard provides automated attendance analysis, there are certain constraints:

-   **Manual Data Dependency**  
    The system relies on manually imported Excel attendance data and is not directly connected to biometric or HR systems.
    
-   **No Real-Time Integration**  
    Attendance updates are reflected only after the data file is refreshed, not in real time.
    
-   **Fixed Hourly Rate**  
    The payout calculation assumes a single hourly rate and does not support role-based or overtime rate variations.
    
-   **Assumption-Based Missed Punch Handling**  
    In case of missed punch, the system allocates standard shift hours, which may not always reflect actual working time.
    
-   **Scalability**  
    Excel performance may reduce with very large datasets or multi-year records.

### **2. Future Enhancements**

The dashboard can be improved with the following additions:

-   **Integration with Biometric/HR System** for automatic data capture
    
-   **Role-Based Pay Structure** with overtime and weekend rates
    
-   **VBA Automation** to refresh data and generate monthly reports
    
-   **Email Alerts** for late check-in, early exit, and missed punch
    
-   **Power BI Migration** for large-scale and real-time analytics
      
These enhancements would make the system more robust and enterprise ready.

## Conclusion

The Employee Attendance Dashboard successfully transforms raw punch records into a structured and interactive analytical solution using Microsoft Excel. By automating the calculation of working hours, attendance status, and payroll estimation, the system reduces manual effort and minimizes errors commonly found in traditional attendance sheets.

The dashboard provides both organizational and employee-level insights, enabling HR and management to monitor presence trends, identify irregular behaviors such as late check-ins, early check-outs, and missed punches, and compare **actual payout versus expected payout**. The inclusion of business rules such as the **5-minute grace period** and standardized handling of **missed punches** ensures fair and consistent payroll processing.

Custom columns built through Excel formulas form the core of the model, while visual highlighting of these columns helps users clearly distinguish calculated fields from raw data. The two-page interactive design allows quick decision-making without the need for external BI tools.

Overall, this project demonstrates how Excel can function as a powerful analytics platform for attendance management, providing:

-   Accurate and automated hour computation
-   Transparent payroll calculation
-   Better workforce visibility
-   Data-driven HR decision support

With further enhancements such as system integration and automation, the dashboard can evolve into a complete enterprise attendance solution.
