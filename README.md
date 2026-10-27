🎓 Students Performance Dashboard (Power BI)

An interactive Power BI dashboard that analyzes the academic performance of 500 university students across four departments. It shows how attendance, study habits, and assignments relate to final scores, and it flags students who are at risk of failing so they can be supported early.

Dashboard preview

<!-- Add a screenshot of the dashboard page to /images/dashboard.png -->
📌 Project Overview

Universities collect a lot of student data but rarely turn it into something that advisors and instructors can act on. This project cleans a raw student-performance CSV, models it as a star schema, and presents the key indicators in a one-page dashboard plus a drill-down page for at-risk students.

Questions the dashboard answers

How many students are there, and what are the average score, pass rate, and attendance?
Which departments, sections, and academic years perform best?
Does higher attendance lead to higher final scores?
How many students are at risk (final score below 50), and who are they?
📂 Dataset
Item	Details
Source file	Student_Performance_Raw.csv
Rows	501 records (500 unique students)
Departments	Computer Science, Artificial Intelligence, Information Systems, Cybersecurity
Academic years	1 – 4
Sections	A, B, C

Main columns: StudentID, StudentName, Gender, Department, AcademicYear, Section, Instructor, AttendancePct, StudyHoursPerWeek, AssignmentAvg, PreviousScore, InternetAccess, Scholarship, FinalScore, PerformanceBand, PassStatus.

🧹 Data Cleaning (Power Query)
Issue	Treatment
Wrong data types	Set proper types (text, whole number, decimal) for every column
Inconsistent text (extra spaces, mixed case)	Trim + Capitalize Each Word on Gender, Department, StudentName; Trim on PassStatus and PerformanceBand
Blank InternetAccess	Replaced with "Unknown"
Missing StudyHoursPerWeek	Replaced with 9.2 (the dataset median)
Invalid attendance (> 100%)	New column Custom_attendence: invalid values are set to null
Invalid assignment average (< 0 or > 100)	New column Custom_assigment: invalid values are set to null
🧱 Data Model

A star schema with one fact table and two dimension tables.

 dim_department ───┐
   (Department)    │ 1
                   ▼ *
         FactStudentPerformance
                   ▲ *
                   │ 1
 dim_acadimic_year ┘
   (AcademicYear)
Table	Type	Description
FactStudentPerformance	Fact	One row per student record, with all metrics and the calculated bands
dim_department	Dimension	Distinct departments (created with Remove Duplicates)
dim_acadimic_year	Dimension	Distinct academic years (created with Remove Duplicates)

Relationships: Many-to-one, single direction, from the fact table to each dimension.

🧮 DAX
Calculated columns
Column	Logic
AttendanceBand	Low (< 60), Moderate (60–79), High (≥ 80), Unknown if blank
ScoreBand	At Risk (< 50), Satisfactory (50–69), Good (70–84), Excellent (≥ 85)
Measures
Measure	Purpose
total_students	Distinct count of StudentID
final_score / ava_final	Average final score
passed_students	Number of students who passed
pass_rate	Passed students ÷ total students
at-rist_students	Students with a final score below 50
ava_attendance	Average attendance %
ava_study_houres	Average study hours per week
HTML_KPI_Cards	Four KPI cards (students, average score, pass rate, attendance)
HTML_Department_Chart	Average score by department as progress bars
HTML_Attendance_Bands	Student count per attendance band
HTML_Pass_Fail	Passed vs at-risk comparison bars

The HTML_* measures generate HTML and CSS, and they are displayed with the HTML Content custom visual, which gives the dashboard its custom card design.

📊 Dashboard Pages
1. Dashboard
KPI cards: total students, average score, pass rate, average attendance
Bar chart: average final score by department
Donut chart: average score by academic year
Column chart: average score by section and pass status
Scatter plot: attendance % vs final score, per student
Cards: attendance distribution and pass vs at-risk
Slicer: gender (the pass vs fail card also responds to department and academic year selections)
2. AT_risk_students
Detail table with student ID, department, academic year, attendance, study hours, assignment average, final score, and score band
Final score slicer to focus on students below the passing threshold
🔍 Key Insights
Overall average final score is about 66 / 100, and about 92% of students passed.
38 students (7.6%) are at risk, with a final score below 50.
Average scores are close across departments, from about 65 (Computer Science) to about 68 (Cybersecurity).
Students with high attendance have the highest average score (about 67.5), compared with about 64 for the other two bands.

Figures come from the current dataset and update with the slicers.

🛠️ Tools & Skills
Power BI Desktop
Power Query (M) for cleaning and transformation
DAX for measures and calculated columns
Data modeling (star schema)
HTML / CSS inside DAX for custom visuals
