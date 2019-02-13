
# PyCity Schools Analysis

+ Average math and reading scores stay consistent across grade level when grouped by school.  There is no major improvement in scores from any school.
+ Math passing rates are always consistently lower across every metric, but the difference between math and reading passing rates is greater amoung lower performing schools, large schools, and higher spending per student which all seem to correlate.  
+ The top 5 schools are all charter schools while the bottom 5 all district schools. 
+ In general (one exception), per student spending is higher in bottom performing schools than top performing.  
+ Schools under 2000 students have much higher passing rates than those with student populations above 2000.  A comparision of 95 to 75%.  The same phenomenon is seen with high and low per student spending brackets and district versus charter schools.  






```python
#Dependencies
import pandas as pd
import numpy as np
import os

# define file path
schools_file = os.path.join('Resources','schools_complete.csv')
students_file = os.path.join('Resources', 'students_complete.csv')

# read schools file
schools_df = pd.read_csv(schools_file)

#read student file
students_df = pd.read_csv(students_file)

#renames for merge
schools_df.rename(columns = {'name': 'school'}, inplace = True)

merged_df = students_df.merge(schools_df, how = 'left', on = 'school')


```

## District Summary


```python
#create array of unique school names
unique_school_names = schools_df['school'].unique()

#gives the length of unique school names to give us how many schools
school_count = len(unique_school_names)

#district student count
dist_student_count = schools_df['size'].sum()

#student count from student file (to verify with district student count)
total_student_rec = students_df['name'].count()

#total budget
total_budget = schools_df['budget'].sum()

#calculations for number and % passing reading
num_passing_reading = students_df.loc[students_df['reading_score'] >= 70]['reading_score'].count()
perc_pass_reading = num_passing_reading/total_student_rec

#calculations for number and % passing math
num_passing_math = students_df.loc[students_df['math_score'] >= 70]['math_score'].count()
perc_pass_math = num_passing_math/total_student_rec

#average math score calculation
avg_math_score = students_df['math_score'].mean()

#average reading score calculation
avg_reading_score = students_df['reading_score'].mean()

#Overall Passing Rate Calculations
overall_pass = students_df[(students_df['math_score'] >= 70) & (students_df['reading_score'] >= 70)]['name'].count()/total_student_rec

# district dataframe from dictionary

district_summary = pd.DataFrame({
    
    "Total Schools": [school_count],
    "Total Students": [dist_student_count],
    "Total Budget": [total_budget],
    "Average Reading Score": [avg_reading_score],
    "Average Math Score": [avg_math_score],
    "% Passing Reading":[perc_pass_reading],
    "% Passing Math": [perc_pass_math],
    "Overall Passing Rate": [overall_pass]

})

#store as different df to change order
dist_sum = district_summary[["Total Schools", 
                             "Total Students", 
                             "Total Budget", 
                             "Average Reading Score", 
                             "Average Math Score", 
                             '% Passing Reading', 
                             '% Passing Math', 
                             'Overall Passing Rate']]

#format cells
dist_sum.style.format({"Total Budget": "${:,.2f}", 
                       "Average Reading Score": "{:.1f}", 
                       "Average Math Score": "{:.1f}", 
                       "% Passing Math": "{:.1%}", 
                       "% Passing Reading": "{:.1%}", 
                       "Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Schools</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Reading Score</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >% Passing Reading</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691" class="row_heading level0 row0" >0</th> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col0" class="data row0 col0" >15</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col1" class="data row0 col1" >39170</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col2" class="data row0 col2" >$24,649,428.00</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col3" class="data row0 col3" >81.9</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col4" class="data row0 col4" >79.0</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col5" class="data row0 col5" >85.8%</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col6" class="data row0 col6" >75.0%</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col7" class="data row0 col7" >65.2%</td> 
    </tr></tbody> 
</table> 



## School Summary


```python
#groups by school
by_school = merged_df.set_index('school').groupby(['school'])

#school types
sch_types = schools_df.set_index('school')['type']

# total students by school
stu_per_sch = by_school['Student ID'].count()

# school budget
sch_budget = schools_df.set_index('school')['budget']

#per student budget
stu_budget = schools_df.set_index('school')['budget']/schools_df.set_index('school')['size']

#avg scores by school
avg_math = by_school['math_score'].mean()
avg_read = by_school['reading_score'].mean()

# % passing scores
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('school')['Student ID'].count()/stu_per_sch 
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('school')['Student ID'].count()/stu_per_sch 
overall = merged_df[(merged_df['reading_score'] >= 70) & (merged_df['math_score'] >= 70)].groupby('school')['Student ID'].count()/stu_per_sch 

sch_summary = pd.DataFrame({
    "School Type": sch_types,
    "Total Students": stu_per_sch,
    "Per Student Budget": stu_budget,
    "Total School Budget": sch_budget,
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_read,
    "Overall Passing Rate": overall
})




#munging
sch_summary = sch_summary[['School Type', 
                          'Total Students', 
                          'Total School Budget', 
                          'Per Student Budget', 
                          'Average Math Score', 
                          'Average Reading Score',
                          '% Passing Math',
                          '% Passing Reading',
                          'Overall Passing Rate']]

#formatting
sch_summary.style.format({'Total Students': '{:,}', 
                          "Total School Budget": "${:,}", 
                          "Per Student Budget": "${:.0f}",
                          'Average Math Score': "{:.1f}", 
                          'Average Reading Score': "{:.1f}", 
                          "% Passing Math": "{:.1%}", 
                          "% Passing Reading": "{:.1%}", 
                          "Overall Passing Rate": "{:.1%}"})

```




<style  type="text/css" >
</style>  
<table id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col0" class="data row0 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col1" class="data row0 col1" >4,976</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col2" class="data row0 col2" >$3,124,928</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col3" class="data row0 col3" >$628</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col4" class="data row0 col4" >77.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col5" class="data row0 col5" >81.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col6" class="data row0 col6" >66.7%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col7" class="data row0 col7" >81.9%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row0_col8" class="data row0 col8" >54.6%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col1" class="data row1 col1" >1,858</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col2" class="data row1 col2" >$1,081,356</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col3" class="data row1 col3" >$582</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col4" class="data row1 col4" >83.1</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col5" class="data row1 col5" >84.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col6" class="data row1 col6" >94.1%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col7" class="data row1 col7" >97.0%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row1_col8" class="data row1 col8" >91.3%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col0" class="data row2 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col1" class="data row2 col1" >2,949</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col2" class="data row2 col2" >$1,884,411</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col3" class="data row2 col3" >$639</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col4" class="data row2 col4" >76.7</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col5" class="data row2 col5" >81.2</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col6" class="data row2 col6" >66.0%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col7" class="data row2 col7" >80.7%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row2_col8" class="data row2 col8" >53.2%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col0" class="data row3 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col1" class="data row3 col1" >2,739</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col2" class="data row3 col2" >$1,763,916</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col3" class="data row3 col3" >$644</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col4" class="data row3 col4" >77.1</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col5" class="data row3 col5" >80.7</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col6" class="data row3 col6" >68.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col7" class="data row3 col7" >79.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row3_col8" class="data row3 col8" >54.3%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col1" class="data row4 col1" >1,468</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col2" class="data row4 col2" >$917,500</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col3" class="data row4 col3" >$625</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col4" class="data row4 col4" >83.4</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col5" class="data row4 col5" >83.8</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col6" class="data row4 col6" >93.4%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col7" class="data row4 col7" >97.1%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row4_col8" class="data row4 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col0" class="data row5 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col1" class="data row5 col1" >4,635</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col2" class="data row5 col2" >$3,022,020</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col3" class="data row5 col3" >$652</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col4" class="data row5 col4" >77.3</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col5" class="data row5 col5" >80.9</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col6" class="data row5 col6" >66.8%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col7" class="data row5 col7" >80.9%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row5_col8" class="data row5 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col0" class="data row6 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col1" class="data row6 col1" >427</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col2" class="data row6 col2" >$248,087</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col3" class="data row6 col3" >$581</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col4" class="data row6 col4" >83.8</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col5" class="data row6 col5" >83.8</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col6" class="data row6 col6" >92.5%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col7" class="data row6 col7" >96.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row6_col8" class="data row6 col8" >89.2%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col0" class="data row7 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col1" class="data row7 col1" >2,917</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col2" class="data row7 col2" >$1,910,635</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col3" class="data row7 col3" >$655</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col4" class="data row7 col4" >76.6</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col5" class="data row7 col5" >81.2</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col6" class="data row7 col6" >65.7%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col7" class="data row7 col7" >81.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row7_col8" class="data row7 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col0" class="data row8 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col1" class="data row8 col1" >4,761</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col2" class="data row8 col2" >$3,094,650</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col3" class="data row8 col3" >$650</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col4" class="data row8 col4" >77.1</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col5" class="data row8 col5" >81.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col6" class="data row8 col6" >66.1%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col7" class="data row8 col7" >81.2%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row8_col8" class="data row8 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col0" class="data row9 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col1" class="data row9 col1" >962</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col2" class="data row9 col2" >$585,858</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col3" class="data row9 col3" >$609</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col4" class="data row9 col4" >83.8</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col5" class="data row9 col5" >84.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col6" class="data row9 col6" >94.6%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col7" class="data row9 col7" >95.9%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row9_col8" class="data row9 col8" >90.5%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col0" class="data row10 col0" >District</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col1" class="data row10 col1" >3,999</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col2" class="data row10 col2" >$2,547,363</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col3" class="data row10 col3" >$637</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col4" class="data row10 col4" >76.8</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col5" class="data row10 col5" >80.7</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col6" class="data row10 col6" >66.4%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col7" class="data row10 col7" >80.2%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row10_col8" class="data row10 col8" >53.0%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col0" class="data row11 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col1" class="data row11 col1" >1,761</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col2" class="data row11 col2" >$1,056,600</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col3" class="data row11 col3" >$600</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col4" class="data row11 col4" >83.4</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col5" class="data row11 col5" >83.7</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col6" class="data row11 col6" >93.9%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col7" class="data row11 col7" >95.9%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row11_col8" class="data row11 col8" >89.9%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col0" class="data row12 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col1" class="data row12 col1" >1,635</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col2" class="data row12 col2" >$1,043,130</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col3" class="data row12 col3" >$638</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col4" class="data row12 col4" >83.4</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col5" class="data row12 col5" >83.8</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col6" class="data row12 col6" >93.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col7" class="data row12 col7" >97.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row12_col8" class="data row12 col8" >90.9%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col0" class="data row13 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col1" class="data row13 col1" >2,283</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col2" class="data row13 col2" >$1,319,574</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col3" class="data row13 col3" >$578</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col4" class="data row13 col4" >83.3</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col5" class="data row13 col5" >84.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col6" class="data row13 col6" >93.9%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col7" class="data row13 col7" >96.5%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row13_col8" class="data row13 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col0" class="data row14 col0" >Charter</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col1" class="data row14 col1" >1,800</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col2" class="data row14 col2" >$1,049,400</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col3" class="data row14 col3" >$583</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col4" class="data row14 col4" >83.7</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col5" class="data row14 col5" >84.0</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col6" class="data row14 col6" >93.3%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col7" class="data row14 col7" >96.6%</td> 
        <td id="T_1beb1fc6_9a44_11e7_9648_0c4de9c48691row14_col8" class="data row14 col8" >90.3%</td> 
    </tr></tbody> 
</table> 



## Top Performing Schools by Passing Rate


```python
# sort values by passing rate and then only print top 5 
top_5 = sch_summary.sort_values("Overall Passing Rate", ascending = False)
top_5.head().style.format({'Total Students': '{:,}',
                           "Total School Budget": "${:,}", 
                           "Per Student Budget": "${:.0f}", 
                           "% Passing Math": "{:.1%}", 
                           "% Passing Reading": "{:.1%}", 
                           "Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691" class="row_heading level0 row0" >Cabrera High School</th> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col0" class="data row0 col0" >Charter</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col1" class="data row0 col1" >1,858</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col2" class="data row0 col2" >$1,081,356</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col3" class="data row0 col3" >$582</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col4" class="data row0 col4" >83.0619</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col5" class="data row0 col5" >83.9758</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col6" class="data row0 col6" >94.1%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col7" class="data row0 col7" >97.0%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row0_col8" class="data row0 col8" >91.3%</td> 
    </tr>    <tr> 
        <th id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691" class="row_heading level0 row1" >Thomas High School</th> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col0" class="data row1 col0" >Charter</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col1" class="data row1 col1" >1,635</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col2" class="data row1 col2" >$1,043,130</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col3" class="data row1 col3" >$638</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col4" class="data row1 col4" >83.4183</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col5" class="data row1 col5" >83.8489</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col6" class="data row1 col6" >93.3%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col7" class="data row1 col7" >97.3%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row1_col8" class="data row1 col8" >90.9%</td> 
    </tr>    <tr> 
        <th id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691" class="row_heading level0 row2" >Griffin High School</th> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col0" class="data row2 col0" >Charter</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col1" class="data row2 col1" >1,468</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col2" class="data row2 col2" >$917,500</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col3" class="data row2 col3" >$625</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col4" class="data row2 col4" >83.3515</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col5" class="data row2 col5" >83.8168</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col6" class="data row2 col6" >93.4%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col7" class="data row2 col7" >97.1%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row2_col8" class="data row2 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691" class="row_heading level0 row3" >Wilson High School</th> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col0" class="data row3 col0" >Charter</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col1" class="data row3 col1" >2,283</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col2" class="data row3 col2" >$1,319,574</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col3" class="data row3 col3" >$578</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col4" class="data row3 col4" >83.2742</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col5" class="data row3 col5" >83.9895</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col6" class="data row3 col6" >93.9%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col7" class="data row3 col7" >96.5%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row3_col8" class="data row3 col8" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691" class="row_heading level0 row4" >Pena High School</th> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col0" class="data row4 col0" >Charter</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col1" class="data row4 col1" >962</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col2" class="data row4 col2" >$585,858</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col3" class="data row4 col3" >$609</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col4" class="data row4 col4" >83.8399</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col5" class="data row4 col5" >84.0447</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col6" class="data row4 col6" >94.6%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col7" class="data row4 col7" >95.9%</td> 
        <td id="T_1de965ee_9a44_11e7_a6d9_0c4de9c48691row4_col8" class="data row4 col8" >90.5%</td> 
    </tr></tbody> 
</table> 



## Bottom Performing Schools by Passing Rate


```python
#bottom 5 schools from worse to best
#take tail of top5 sort and re-sort from worst to best
bottom_5 = top_5.tail()
bottom_5 = bottom_5.sort_values('Overall Passing Rate')
bottom_5.style.format({'Total Students': '{:,}', 
                       "Total School Budget": "${:,}", 
                       "Per Student Budget": "${:.0f}", 
                       "% Passing Math": "{:.1%}", 
                       "% Passing Reading": "{:.1%}", 
                       "Overall Passing Rate": "{:.1%}"})
```




<style  type="text/css" >
</style>  
<table id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School Type</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total School Budget</th> 
        <th class="col_heading level0 col3" >Per Student Budget</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >Average Reading Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691" class="row_heading level0 row0" >Rodriguez High School</th> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col0" class="data row0 col0" >District</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col1" class="data row0 col1" >3,999</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col2" class="data row0 col2" >$2,547,363</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col3" class="data row0 col3" >$637</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col4" class="data row0 col4" >76.8427</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col5" class="data row0 col5" >80.7447</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col6" class="data row0 col6" >66.4%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col7" class="data row0 col7" >80.2%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row0_col8" class="data row0 col8" >53.0%</td> 
    </tr>    <tr> 
        <th id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691" class="row_heading level0 row1" >Figueroa High School</th> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col0" class="data row1 col0" >District</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col1" class="data row1 col1" >2,949</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col2" class="data row1 col2" >$1,884,411</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col3" class="data row1 col3" >$639</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col4" class="data row1 col4" >76.7118</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col5" class="data row1 col5" >81.158</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col6" class="data row1 col6" >66.0%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col7" class="data row1 col7" >80.7%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row1_col8" class="data row1 col8" >53.2%</td> 
    </tr>    <tr> 
        <th id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691" class="row_heading level0 row2" >Huang High School</th> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col0" class="data row2 col0" >District</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col1" class="data row2 col1" >2,917</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col2" class="data row2 col2" >$1,910,635</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col3" class="data row2 col3" >$655</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col4" class="data row2 col4" >76.6294</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col5" class="data row2 col5" >81.1827</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col6" class="data row2 col6" >65.7%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col7" class="data row2 col7" >81.3%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row2_col8" class="data row2 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691" class="row_heading level0 row3" >Hernandez High School</th> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col0" class="data row3 col0" >District</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col1" class="data row3 col1" >4,635</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col2" class="data row3 col2" >$3,022,020</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col3" class="data row3 col3" >$652</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col4" class="data row3 col4" >77.2898</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col5" class="data row3 col5" >80.9344</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col6" class="data row3 col6" >66.8%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col7" class="data row3 col7" >80.9%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row3_col8" class="data row3 col8" >53.5%</td> 
    </tr>    <tr> 
        <th id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691" class="row_heading level0 row4" >Johnson High School</th> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col0" class="data row4 col0" >District</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col1" class="data row4 col1" >4,761</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col2" class="data row4 col2" >$3,094,650</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col3" class="data row4 col3" >$650</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col4" class="data row4 col4" >77.0725</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col5" class="data row4 col5" >80.9664</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col6" class="data row4 col6" >66.1%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col7" class="data row4 col7" >81.2%</td> 
        <td id="T_1ed7f1a8_9a44_11e7_847a_0c4de9c48691row4_col8" class="data row4 col8" >53.5%</td> 
    </tr></tbody> 
</table> 



## Math Scores by Grade


```python
#creates grade level average math scores for each school 
ninth_math = students_df.loc[students_df['grade'] == '9th'].groupby('school')["math_score"].mean()
tenth_math = students_df.loc[students_df['grade'] == '10th'].groupby('school')["math_score"].mean()
eleventh_math = students_df.loc[students_df['grade'] == '11th'].groupby('school')["math_score"].mean()
twelfth_math = students_df.loc[students_df['grade'] == '12th'].groupby('school')["math_score"].mean()

math_scores = pd.DataFrame({
        "9th": ninth_math,
        "10th": tenth_math,
        "11th": eleventh_math,
        "12th": twelfth_math
})
math_scores = math_scores[['9th', '10th', '11th', '12th']]
math_scores.index.name = "School"

#show and format
math_scores.style.format({'9th': '{:.1f}', 
                          "10th": '{:.1f}', 
                          "11th": "{:.1f}", 
                          "12th": "{:.1f}"})
```




<style  type="text/css" >
</style>  
<table id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row0_col0" class="data row0 col0" >77.1</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row0_col1" class="data row0 col1" >77.0</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row0_col2" class="data row0 col2" >77.5</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row0_col3" class="data row0 col3" >76.5</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row1_col0" class="data row1 col0" >83.1</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row1_col1" class="data row1 col1" >83.2</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row1_col2" class="data row1 col2" >82.8</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row1_col3" class="data row1 col3" >83.3</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row2_col0" class="data row2 col0" >76.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row2_col1" class="data row2 col1" >76.5</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row2_col2" class="data row2 col2" >76.9</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row2_col3" class="data row2 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row3_col0" class="data row3 col0" >77.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row3_col1" class="data row3 col1" >77.7</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row3_col2" class="data row3 col2" >76.9</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row3_col3" class="data row3 col3" >76.2</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row4_col0" class="data row4 col0" >82.0</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row4_col1" class="data row4 col1" >84.2</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row4_col2" class="data row4 col2" >83.8</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row4_col3" class="data row4 col3" >83.4</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row5_col0" class="data row5 col0" >77.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row5_col1" class="data row5 col1" >77.3</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row5_col2" class="data row5 col2" >77.1</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row5_col3" class="data row5 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row6_col0" class="data row6 col0" >83.8</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row6_col1" class="data row6 col1" >83.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row6_col2" class="data row6 col2" >85.0</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row6_col3" class="data row6 col3" >82.9</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row7_col0" class="data row7 col0" >77.0</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row7_col1" class="data row7 col1" >75.9</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row7_col2" class="data row7 col2" >76.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row7_col3" class="data row7 col3" >77.2</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row8_col0" class="data row8 col0" >77.2</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row8_col1" class="data row8 col1" >76.7</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row8_col2" class="data row8 col2" >77.5</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row8_col3" class="data row8 col3" >76.9</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row9_col0" class="data row9 col0" >83.6</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row9_col1" class="data row9 col1" >83.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row9_col3" class="data row9 col3" >84.1</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row10_col0" class="data row10 col0" >76.9</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row10_col1" class="data row10 col1" >76.6</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row10_col2" class="data row10 col2" >76.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row10_col3" class="data row10 col3" >77.7</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row11_col0" class="data row11 col0" >83.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row11_col1" class="data row11 col1" >82.9</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row11_col2" class="data row11 col2" >83.4</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row11_col3" class="data row11 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row12_col0" class="data row12 col0" >83.6</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row12_col1" class="data row12 col1" >83.1</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row12_col2" class="data row12 col2" >83.5</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row12_col3" class="data row12 col3" >83.5</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row13_col0" class="data row13 col0" >83.1</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row13_col1" class="data row13 col1" >83.7</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row13_col2" class="data row13 col2" >83.2</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row13_col3" class="data row13 col3" >83.0</td> 
    </tr>    <tr> 
        <th id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row14_col0" class="data row14 col0" >83.3</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row14_col1" class="data row14 col1" >84.0</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row14_col2" class="data row14 col2" >83.8</td> 
        <td id="T_1ff67b80_9a47_11e7_bee4_0c4de9c48691row14_col3" class="data row14 col3" >83.6</td> 
    </tr></tbody> 
</table> 



## Reading Scores by Grade


```python
#creates grade level average reading scores for each school
ninth_reading = students_df.loc[students_df['grade'] == '9th'].groupby('school')["reading_score"].mean()
tenth_reading = students_df.loc[students_df['grade'] == '10th'].groupby('school')["reading_score"].mean()
eleventh_reading = students_df.loc[students_df['grade'] == '11th'].groupby('school')["reading_score"].mean()
twelfth_reading = students_df.loc[students_df['grade'] == '12th'].groupby('school')["reading_score"].mean()

#merges the reading score averages by school and grade together
reading_scores = pd.DataFrame({
        "9th": ninth_reading,
        "10th": tenth_reading,
        "11th": eleventh_reading,
        "12th": twelfth_reading
})
reading_scores = reading_scores[['9th', '10th', '11th', '12th']]
reading_scores.index.name = "School"

#format
reading_scores.style.format({'9th': '{:.1f}', 
                             "10th": '{:.1f}', 
                             "11th": "{:.1f}", 
                             "12th": "{:.1f}"})

```




<style  type="text/css" >
</style>  
<table id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >9th</th> 
        <th class="col_heading level0 col1" >10th</th> 
        <th class="col_heading level0 col2" >11th</th> 
        <th class="col_heading level0 col3" >12th</th> 
    </tr>    <tr> 
        <th class="index_name level0" >School</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row0" >Bailey High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row0_col0" class="data row0 col0" >81.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row0_col1" class="data row0 col1" >80.9</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row0_col2" class="data row0 col2" >80.9</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row0_col3" class="data row0 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row1" >Cabrera High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row1_col0" class="data row1 col0" >83.7</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row1_col1" class="data row1 col1" >84.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row1_col2" class="data row1 col2" >83.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row1_col3" class="data row1 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row2" >Figueroa High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row2_col0" class="data row2 col0" >81.2</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row2_col2" class="data row2 col2" >80.6</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row2_col3" class="data row2 col3" >81.4</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row3" >Ford High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row3_col0" class="data row3 col0" >80.6</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row3_col1" class="data row3 col1" >81.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row3_col2" class="data row3 col2" >80.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row3_col3" class="data row3 col3" >80.7</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row4" >Griffin High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row4_col0" class="data row4 col0" >83.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row4_col1" class="data row4 col1" >83.7</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row4_col2" class="data row4 col2" >84.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row4_col3" class="data row4 col3" >84.0</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row5" >Hernandez High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row5_col0" class="data row5 col0" >80.9</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row5_col1" class="data row5 col1" >80.7</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row5_col2" class="data row5 col2" >81.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row5_col3" class="data row5 col3" >80.9</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row6" >Holden High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row6_col0" class="data row6 col0" >83.7</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row6_col1" class="data row6 col1" >83.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row6_col2" class="data row6 col2" >83.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row6_col3" class="data row6 col3" >84.7</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row7" >Huang High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row7_col0" class="data row7 col0" >81.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row7_col1" class="data row7 col1" >81.5</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row7_col2" class="data row7 col2" >81.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row7_col3" class="data row7 col3" >80.3</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row8" >Johnson High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row8_col0" class="data row8 col0" >81.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row8_col1" class="data row8 col1" >80.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row8_col2" class="data row8 col2" >80.6</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row8_col3" class="data row8 col3" >81.2</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row9" >Pena High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row9_col0" class="data row9 col0" >83.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row9_col1" class="data row9 col1" >83.6</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row9_col2" class="data row9 col2" >84.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row9_col3" class="data row9 col3" >84.6</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row10" >Rodriguez High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row10_col0" class="data row10 col0" >81.0</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row10_col1" class="data row10 col1" >80.6</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row10_col2" class="data row10 col2" >80.9</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row10_col3" class="data row10 col3" >80.4</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row11" >Shelton High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row11_col0" class="data row11 col0" >84.1</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row11_col1" class="data row11 col1" >83.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row11_col2" class="data row11 col2" >84.4</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row11_col3" class="data row11 col3" >82.8</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row12" >Thomas High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row12_col0" class="data row12 col0" >83.7</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row12_col1" class="data row12 col1" >84.3</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row12_col2" class="data row12 col2" >83.6</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row12_col3" class="data row12 col3" >83.8</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row13" >Wilson High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row13_col0" class="data row13 col0" >83.9</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row13_col1" class="data row13 col1" >84.0</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row13_col2" class="data row13 col2" >83.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row13_col3" class="data row13 col3" >84.3</td> 
    </tr>    <tr> 
        <th id="T_2989b59a_9a47_11e7_b434_0c4de9c48691" class="row_heading level0 row14" >Wright High School</th> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row14_col0" class="data row14 col0" >83.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row14_col1" class="data row14 col1" >83.8</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row14_col2" class="data row14 col2" >84.2</td> 
        <td id="T_2989b59a_9a47_11e7_b434_0c4de9c48691row14_col3" class="data row14 col3" >84.1</td> 
    </tr></tbody> 
</table> 



## Scores by School Spending


```python
# create spending bins
bins = [0, 584.999, 614.999, 644.999, 999999]
group_name = ['< $585', "$585 - 614", "$615 - 644", "> $644"]
merged_df['spending_bins'] = pd.cut(merged_df['budget']/merged_df['size'], bins, labels = group_name)

#group by spending
by_spending = merged_df.groupby('spending_bins')

#calculations
avg_math = by_spending['math_score'].mean()
avg_read = by_spending['reading_score'].mean()
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()
overall = merged_df[(merged_df['reading_score'] >= 70) & (merged_df['math_score'] >= 70)].groupby('spending_bins')['Student ID'].count()/by_spending['Student ID'].count()

            
# df build            
scores_by_spend = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_read,
    "Overall Passing Rate": overall
            
})
            
#reorder columns
scores_by_spend = scores_by_spend[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]

scores_by_spend.index.name = "Per Student Budget"
scores_by_spend = scores_by_spend.reindex(group_name)

#formating
scores_by_spend.style.format({'Average Math Score': '{:.1f}', 
                              'Average Reading Score': '{:.1f}', 
                              '% Passing Math': '{:.1%}', 
                              '% Passing Reading':'{:.1%}', 
                              'Overall Passing Rate': '{:.1%}'})

```




<style  type="text/css" >
</style>  
<table id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Per Student Budget</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691" class="row_heading level0 row0" >< $585</th> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row0_col1" class="data row0 col1" >84.0</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row0_col2" class="data row0 col2" >93.7%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row0_col3" class="data row0 col3" >96.7%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row0_col4" class="data row0 col4" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691" class="row_heading level0 row1" >$585 - 614</th> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row1_col0" class="data row1 col0" >83.5</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row1_col1" class="data row1 col1" >83.8</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row1_col2" class="data row1 col2" >94.1%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row1_col3" class="data row1 col3" >95.9%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row1_col4" class="data row1 col4" >90.1%</td> 
    </tr>    <tr> 
        <th id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691" class="row_heading level0 row2" >$615 - 644</th> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row2_col0" class="data row2 col0" >78.1</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row2_col1" class="data row2 col1" >81.4</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row2_col2" class="data row2 col2" >71.4%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row2_col3" class="data row2 col3" >83.6%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row2_col4" class="data row2 col4" >60.3%</td> 
    </tr>    <tr> 
        <th id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691" class="row_heading level0 row3" >> $644</th> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row3_col0" class="data row3 col0" >77.0</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row3_col1" class="data row3 col1" >81.0</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row3_col2" class="data row3 col2" >66.2%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row3_col3" class="data row3 col3" >81.1%</td> 
        <td id="T_ee01b6a8_9a46_11e7_bfce_0c4de9c48691row3_col4" class="data row3 col4" >53.5%</td> 
    </tr></tbody> 
</table> 



## Scores by School Size


```python
# create size bins
bins = [0, 999, 1999, 99999999999]
group_name = ["Small (<1000)", "Medium (1000-2000)" , "Large (>2000)"]
merged_df['size_bins'] = pd.cut(merged_df['size'], bins, labels = group_name)

#group by spending
by_size = merged_df.groupby('size_bins')

#calculations 
avg_math = by_size['math_score'].mean()
avg_read = by_size['math_score'].mean()
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()
overall = merged_df[(merged_df['reading_score'] >= 70) & (merged_df['math_score'] >= 70)].groupby('size_bins')['Student ID'].count()/by_size['Student ID'].count()

            
# df build            
scores_by_size = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_read,
    "Overall Passing Rate": overall
            
})
            
#reorder columns
scores_by_size = scores_by_size[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]

scores_by_size.index.name = "Total Students"
scores_by_size = scores_by_size.reindex(group_name)

#formating
scores_by_size.style.format({'Average Math Score': '{:.1f}', 
                              'Average Reading Score': '{:.1f}', 
                              '% Passing Math': '{:.1%}', 
                              '% Passing Reading':'{:.1%}', 
                              'Overall Passing Rate': '{:.1%}'})
```




<style  type="text/css" >
</style>  
<table id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Total Students</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691" class="row_heading level0 row0" >Small (<1000)</th> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row0_col0" class="data row0 col0" >83.8</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row0_col1" class="data row0 col1" >83.8</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row0_col2" class="data row0 col2" >94.0%</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row0_col3" class="data row0 col3" >96.0%</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row0_col4" class="data row0 col4" >90.1%</td> 
    </tr>    <tr> 
        <th id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691" class="row_heading level0 row1" >Medium (1000-2000)</th> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row1_col0" class="data row1 col0" >83.4</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row1_col1" class="data row1 col1" >83.4</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row1_col2" class="data row1 col2" >93.6%</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row1_col3" class="data row1 col3" >96.8%</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row1_col4" class="data row1 col4" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691" class="row_heading level0 row2" >Large (>2000)</th> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row2_col0" class="data row2 col0" >77.5</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row2_col1" class="data row2 col1" >77.5</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row2_col2" class="data row2 col2" >68.7%</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row2_col3" class="data row2 col3" >82.1%</td> 
        <td id="T_ec4503ec_9a46_11e7_902f_0c4de9c48691row2_col4" class="data row2 col4" >56.6%</td> 
    </tr></tbody> 
</table> 



## Scores by School Type


```python
# group by type of school
by_type = merged_df.groupby("type")

#calculations 
avg_math = by_type['math_score'].mean()
avg_read = by_type['math_score'].mean()
pass_math = merged_df[merged_df['math_score'] >= 70].groupby('type')['Student ID'].count()/by_type['Student ID'].count()
pass_read = merged_df[merged_df['reading_score'] >= 70].groupby('type')['Student ID'].count()/by_type['Student ID'].count()
overall = merged_df[(merged_df['reading_score'] >= 70) & (merged_df['math_score'] >= 70)].groupby('type')['Student ID'].count()/by_type['Student ID'].count()

# df build            
scores_by_type = pd.DataFrame({
    "Average Math Score": avg_math,
    "Average Reading Score": avg_read,
    '% Passing Math': pass_math,
    '% Passing Reading': pass_read,
    "Overall Passing Rate": overall})
    
#reorder columns
scores_by_type = scores_by_type[[
    "Average Math Score",
    "Average Reading Score",
    '% Passing Math',
    '% Passing Reading',
    "Overall Passing Rate"
]]
scores_by_type.index.name = "Type of School"


#formating
scores_by_type.style.format({'Average Math Score': '{:.1f}', 
                              'Average Reading Score': '{:.1f}', 
                              '% Passing Math': '{:.1%}', 
                              '% Passing Reading':'{:.1%}', 
                              'Overall Passing Rate': '{:.1%}'})
```




<style  type="text/css" >
</style>  
<table id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Type of School</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691" class="row_heading level0 row0" >Charter</th> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row0_col0" class="data row0 col0" >83.4</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row0_col1" class="data row0 col1" >83.4</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row0_col2" class="data row0 col2" >93.7%</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row0_col3" class="data row0 col3" >96.6%</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row0_col4" class="data row0 col4" >90.6%</td> 
    </tr>    <tr> 
        <th id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691" class="row_heading level0 row1" >District</th> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row1_col0" class="data row1 col0" >77.0</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row1_col1" class="data row1 col1" >77.0</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row1_col2" class="data row1 col2" >66.5%</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row1_col3" class="data row1 col3" >80.9%</td> 
        <td id="T_eaa9a600_9a46_11e7_a39f_0c4de9c48691row1_col4" class="data row1 col4" >53.7%</td> 
    </tr></tbody> 
</table> 




```python

```
