
# School District Analysis

![Books Image](images/education.jpg)

In this demonstration, a dataset of high school students' math and reading scores is analyzed to help make budget decisions for a school district.
## Goals:

#### Will create a high level snapshot (in table form) of the district's key metrics, including:


- Total Schools
- Total Students
- Total Budget
- Average Math Score
- Average Reading Score
- % Passing Math
- % Passing Reading
- Overall Passing Rate (Average of the above two)




#### School Summary


Will create an overview table that summarizes key metrics about each school, including:


- School Name
- School Type
- Total Students
- Total School Budget
- Per School Budget
- Average Math Score
- Average Reading Score
- % Passing Math
- % Passing Reading
- Overall Passing Rate (Average of the above two)




#### Top Performing Schools (By Passing Rate)


Will create a table that highlights the top 5 performing schools based on Overall Passing Rate. Will include:


- School Name
- School Type
- Total Students
- Total School Budget
- Per School Budget
- Average Math Score
- Average Reading Score
- % Passing Math
- % Passing Reading
- Overall Passing Rate (Average of the above two)




#### Top Performing Schools (By Passing Rate)


Will create a table that highlights the bottom 5 performing schools based on Overall Passing Rate. Will include all of the same metrics as above.


#### Math Scores by Grade


Will create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.


#### Reading Scores by Grade


Will create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.


#### Scores by School Spending


Will create a table that breaks down school performances based on average Spending Ranges (Per Student). Will use 4 reasonable bins to group school spending. Will include in the table each of the following:


- Average Math Score
- Average Reading Score
- % Passing Math
- % Passing Reading
- Overall Passing Rate (Average of the above two)




#### Scores by School Size


- Will repeat the above breakdown, but this time will group schools based on a reasonable approximation of school size (Small, Medium, Large).


#### Scores by School Type


- Will repeat the above breakdown, but this time group schools based on school type (Charter vs. District).


#### As final considerations:


- The script will work for both datasets.
- Will comment on observable trends in the data.

#### Import dependencies


```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

#### Read in CSVs


```python
schools_csv = pd.read_csv('../pandas_homework/inputfiles/schools_complete.csv')
students_csv = pd.read_csv('../pandas_homework/inputfiles/students_complete.csv')
schools = pd.DataFrame(schools_csv)
students = pd.DataFrame(students_csv)
```

#### Explore data


```python
schools.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
students.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



### High Level Snapshot

#### Perform summary calculations and store


```python
schoolnum = len(schools['School ID'].unique())
```


```python
studentnum = len(students['Student ID'].unique())
```


```python
totbudget = schools.budget.sum()
```


```python
avmath = students.math_score.mean()
```


```python
avreading = students.reading_score.mean()
```


```python
passing_reading = students[students['reading_score'] >= 70]
passr = passing_reading['Student ID'].count()
```


```python
passing_math = students[students['math_score'] >= 70]
passm = passing_math['Student ID'].count()
```


```python
readingpassrate = (passr / studentnum) * 100
```


```python
mathpassrate = (passm / studentnum) * 100
```


```python
totpassrate = (readingpassrate + mathpassrate) / 2
```

#### Create snapshot and format
Make sure to save unformatted snapshot as a variable in case we want to plot


```python
snapshot = pd.DataFrame({'Total Schools': schoolnum,
                         'Total Students': studentnum,
                         'Total Budget': totbudget,
                         'Average Math Score': avmath,
                         'Average Reading Score': avreading,
                         '% Passing Math': mathpassrate,
                         '% Passing Reading': readingpassrate,
                         'Overall Passing Rate': totpassrate}, index=[0])

formatted_snapshot = snapshot.style.format({'Total Budget': '${:,.2f}',
                                            'Average Math Score': '{:.2f}%',
                                            'Average Reading Score': '{:.2f}%',
                                            '% Passing Math': '{:.2f}%',
                                            '% Passing Reading': '{:.2f}%',
                                            'Overall Passing Rate': '{:.2f}%'})
formatted_snapshot
```




<style  type="text/css" >
</style>  
<table id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >% Passing Math</th> 
        <th class="col_heading level0 col1" >% Passing Reading</th> 
        <th class="col_heading level0 col2" >Average Math Score</th> 
        <th class="col_heading level0 col3" >Average Reading Score</th> 
        <th class="col_heading level0 col4" >Overall Passing Rate</th> 
        <th class="col_heading level0 col5" >Total Budget</th> 
        <th class="col_heading level0 col6" >Total Schools</th> 
        <th class="col_heading level0 col7" >Total Students</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col0" class="data row0 col0" >74.98%</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col1" class="data row0 col1" >85.81%</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col2" class="data row0 col2" >78.99%</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col3" class="data row0 col3" >81.88%</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col4" class="data row0 col4" >80.39%</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col5" class="data row0 col5" >$24,649,428.00</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col6" class="data row0 col6" >15</td> 
        <td id="T_9d3974f8_dc91_11e7_8a8d_a0999b1e5b83row0_col7" class="data row0 col7" >39170</td> 
    </tr></tbody> 
</table> 



### School Summary


```python
schools.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
students.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>



Strategy:

1. First start with schools dataframe.
2. Then we need to merge different students groupby queries to schools, one at a time.

The first prepatory step will be to make sure that there is an identical column name between the two dataframes to serve as a key. This will allow us to avoid duplicate columns when merging.
- Lets rename the 'name' column in the schools dataframe to match the student's dataframe column name, 'schools'
- While we're at it, lets remove the superfluous 'School ID' column.


```python
schools = schools.rename(columns={'name':'school'})
```


```python
schools = schools.drop('School ID', axis=1)
```

Great, now we are ready to create a query from the groupby object and merge the result to the schools dataframe

First create a copy of schools:


```python
schools_copy = schools
```

Then create the groupby object out of the students dataframe
- groupby school to get summaries for each school


```python
schools_group = students.groupby('school')
```

Next, query the groupby object to return average reading score for each school
- This will create a series, with the schools as the index.
  - Therefore, because merge will not work with a series, we need to turn the result into a dataframe
    - We will also need to reset the index so we can call out the school column in the merge.


```python
reading_query = pd.DataFrame(schools_group['reading_score'].mean()).reset_index()
```

Now we will merge on the common column:


```python
step1 = schools_copy.merge(reading_query, on='school', how='inner')
```

And we will do this for math score:


```python
math_query = pd.DataFrame(schools_group['math_score'].mean()).reset_index()
step2 = step1.merge(math_query, on='school', how='inner')
```

Now we will do some calculated queries:
- the % students that passed math
- the % students that passed reading

#### Enter ```.apply``` and the ```lambda``` function:

```python
schools_group.apply(lambda x:np.mean(x['math_score']))
```

is the same as

```python
schools_group['math_score'].mean()
```

the benefit here is that you can write any function now and return the result inside a groupby.
- just replace ```np.mean()``` with a defined function


```python
def countpass(series):
    counter = 0
    for x in series:
        if x >= 70:
            counter = counter + 1
    return counter
        
```


```python
mathpass_query = pd.DataFrame(schools_group.apply(lambda x:countpass(x['math_score']))).reset_index()
readingpass_query = pd.DataFrame(schools_group.apply(lambda x:countpass(x['reading_score']))).reset_index()
```

Now we will merge these queries with the dataframe we are building on:


```python
step3 = step2.merge(mathpass_query, on='school', how='inner')
step4 = step3.merge(readingpass_query, on='school', how='inner')
step4
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>0_x</th>
      <th>0_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1916</td>
      <td>2372</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1946</td>
      <td>2381</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1653</td>
      <td>1688</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3094</td>
      <td>3748</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1371</td>
      <td>1426</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>2143</td>
      <td>2204</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1749</td>
      <td>1803</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>3318</td>
      <td>4077</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>395</td>
      <td>411</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>910</td>
      <td>923</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>1680</td>
      <td>1739</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>2654</td>
      <td>3208</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>3145</td>
      <td>3867</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>1871</td>
      <td>2172</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>1525</td>
      <td>1591</td>
    </tr>
  </tbody>
</table>
</div>



Checking math...:


```python
students[(students['school'] == 'Bailey High School') & (students['math_score'] >= 70)]['Student ID'].count()
```




    3318



Looks good, now that all of the groupby queries are done, we need to add some calculated columns to the dataframe.

Right now we have the number of students that passed math and reading respectively.
- We need to calculate percentages of students that passed for math and reading respectively
- we then need to average out those two numbers to give an overall pass rate for each school.


```python
step4['% Passing Math'] = step4['0_x'] / step4['size'] * 100
step4['% Passing Reading'] = step4['0_y'] / step4['size'] * 100
step4['% Passing Overall'] = (step4['% Passing Math'] + step4['% Passing Reading']) / 2
step4
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>0_x</th>
      <th>0_y</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1916</td>
      <td>2372</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1946</td>
      <td>2381</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1653</td>
      <td>1688</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3094</td>
      <td>3748</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1371</td>
      <td>1426</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>2143</td>
      <td>2204</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1749</td>
      <td>1803</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>3318</td>
      <td>4077</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>395</td>
      <td>411</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>910</td>
      <td>923</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>1680</td>
      <td>1739</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>2654</td>
      <td>3208</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>3145</td>
      <td>3867</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>1871</td>
      <td>2172</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>1525</td>
      <td>1591</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
  </tbody>
</table>
</div>



We also need to add total budget for all schools:


```python
step4['Total Budget'] = step4['budget'].sum()
step4
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
      <th>reading_score</th>
      <th>math_score</th>
      <th>0_x</th>
      <th>0_y</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
      <th>Total Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>1916</td>
      <td>2372</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>1946</td>
      <td>2381</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>1653</td>
      <td>1688</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>3094</td>
      <td>3748</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>1371</td>
      <td>1426</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>2143</td>
      <td>2204</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>1749</td>
      <td>1803</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>3318</td>
      <td>4077</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>395</td>
      <td>411</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>910</td>
      <td>923</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>1680</td>
      <td>1739</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>2654</td>
      <td>3208</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>3145</td>
      <td>3867</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>1871</td>
      <td>2172</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>1525</td>
      <td>1591</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>24649428</td>
    </tr>
  </tbody>
</table>
</div>



Now we need to:
- get rid of superfluous columns
- rename columns to be more intelligible
- Order the columns logically


```python
reduced = step4.drop('0_x', axis=1)
```


```python
reduced_more = reduced.drop('0_y', axis=1)
```


```python
renamed = reduced_more.rename(columns={'school':'School',
                                       'type':'Type',
                                       'size':'Total Students',
                                       'budget':'School Budget',
                                       'reading_score':'Average Reading Score',
                                       'math_score':'Average Math Score'})

formatted = renamed.style.format({'School Budget':'${:,.0f}',
                                 'Total Budget':'${:,.0f}',
                                 'Average Reading Score':'{:.1f}%',
                                 'Average Math Score':'{:.1f}%',
                                 '% Passing Math':'{:.1f}%',
                                 '% Passing Reading':'{:.1f}%',
                                 '% Passing Overall':'{:.1f}%'})
```


```python
formatted
```




<style  type="text/css" >
</style>  
<table id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School</th> 
        <th class="col_heading level0 col1" >Type</th> 
        <th class="col_heading level0 col2" >Total Students</th> 
        <th class="col_heading level0 col3" >School Budget</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >Average Math Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Passing Overall</th> 
        <th class="col_heading level0 col9" >Total Budget</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col0" class="data row0 col0" >Huang High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col1" class="data row0 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col2" class="data row0 col2" >2917</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col3" class="data row0 col3" >$1,910,635</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col4" class="data row0 col4" >81.2%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col5" class="data row0 col5" >76.6%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col6" class="data row0 col6" >65.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col7" class="data row0 col7" >81.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col8" class="data row0 col8" >73.5%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row0_col9" class="data row0 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row1" class="row_heading level0 row1" >1</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col0" class="data row1 col0" >Figueroa High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col1" class="data row1 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col2" class="data row1 col2" >2949</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col3" class="data row1 col3" >$1,884,411</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col4" class="data row1 col4" >81.2%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col5" class="data row1 col5" >76.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col6" class="data row1 col6" >66.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col7" class="data row1 col7" >80.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col8" class="data row1 col8" >73.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row1_col9" class="data row1 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row2" class="row_heading level0 row2" >2</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col0" class="data row2 col0" >Shelton High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col1" class="data row2 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col2" class="data row2 col2" >1761</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col3" class="data row2 col3" >$1,056,600</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col4" class="data row2 col4" >83.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col5" class="data row2 col5" >83.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col6" class="data row2 col6" >93.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col7" class="data row2 col7" >95.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col8" class="data row2 col8" >94.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row2_col9" class="data row2 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row3" class="row_heading level0 row3" >3</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col0" class="data row3 col0" >Hernandez High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col1" class="data row3 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col2" class="data row3 col2" >4635</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col3" class="data row3 col3" >$3,022,020</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col4" class="data row3 col4" >80.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col5" class="data row3 col5" >77.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col6" class="data row3 col6" >66.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col7" class="data row3 col7" >80.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col8" class="data row3 col8" >73.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row3_col9" class="data row3 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row4" class="row_heading level0 row4" >4</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col0" class="data row4 col0" >Griffin High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col1" class="data row4 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col2" class="data row4 col2" >1468</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col3" class="data row4 col3" >$917,500</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col4" class="data row4 col4" >83.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col5" class="data row4 col5" >83.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col6" class="data row4 col6" >93.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col7" class="data row4 col7" >97.1%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col8" class="data row4 col8" >95.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row4_col9" class="data row4 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row5" class="row_heading level0 row5" >5</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col0" class="data row5 col0" >Wilson High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col1" class="data row5 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col2" class="data row5 col2" >2283</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col3" class="data row5 col3" >$1,319,574</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col4" class="data row5 col4" >84.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col5" class="data row5 col5" >83.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col6" class="data row5 col6" >93.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col7" class="data row5 col7" >96.5%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col8" class="data row5 col8" >95.2%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row5_col9" class="data row5 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row6" class="row_heading level0 row6" >6</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col0" class="data row6 col0" >Cabrera High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col1" class="data row6 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col2" class="data row6 col2" >1858</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col3" class="data row6 col3" >$1,081,356</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col4" class="data row6 col4" >84.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col5" class="data row6 col5" >83.1%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col6" class="data row6 col6" >94.1%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col7" class="data row6 col7" >97.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col8" class="data row6 col8" >95.6%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row6_col9" class="data row6 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row7" class="row_heading level0 row7" >7</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col0" class="data row7 col0" >Bailey High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col1" class="data row7 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col2" class="data row7 col2" >4976</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col3" class="data row7 col3" >$3,124,928</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col4" class="data row7 col4" >81.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col5" class="data row7 col5" >77.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col6" class="data row7 col6" >66.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col7" class="data row7 col7" >81.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col8" class="data row7 col8" >74.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row7_col9" class="data row7 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row8" class="row_heading level0 row8" >8</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col0" class="data row8 col0" >Holden High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col1" class="data row8 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col2" class="data row8 col2" >427</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col3" class="data row8 col3" >$248,087</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col4" class="data row8 col4" >83.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col5" class="data row8 col5" >83.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col6" class="data row8 col6" >92.5%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col7" class="data row8 col7" >96.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col8" class="data row8 col8" >94.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row8_col9" class="data row8 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row9" class="row_heading level0 row9" >9</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col0" class="data row9 col0" >Pena High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col1" class="data row9 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col2" class="data row9 col2" >962</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col3" class="data row9 col3" >$585,858</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col4" class="data row9 col4" >84.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col5" class="data row9 col5" >83.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col6" class="data row9 col6" >94.6%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col7" class="data row9 col7" >95.9%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col8" class="data row9 col8" >95.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row9_col9" class="data row9 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row10" class="row_heading level0 row10" >10</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col0" class="data row10 col0" >Wright High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col1" class="data row10 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col2" class="data row10 col2" >1800</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col3" class="data row10 col3" >$1,049,400</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col4" class="data row10 col4" >84.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col5" class="data row10 col5" >83.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col6" class="data row10 col6" >93.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col7" class="data row10 col7" >96.6%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col8" class="data row10 col8" >95.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row10_col9" class="data row10 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row11" class="row_heading level0 row11" >11</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col0" class="data row11 col0" >Rodriguez High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col1" class="data row11 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col2" class="data row11 col2" >3999</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col3" class="data row11 col3" >$2,547,363</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col4" class="data row11 col4" >80.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col5" class="data row11 col5" >76.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col6" class="data row11 col6" >66.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col7" class="data row11 col7" >80.2%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col8" class="data row11 col8" >73.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row11_col9" class="data row11 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row12" class="row_heading level0 row12" >12</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col0" class="data row12 col0" >Johnson High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col1" class="data row12 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col2" class="data row12 col2" >4761</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col3" class="data row12 col3" >$3,094,650</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col4" class="data row12 col4" >81.0%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col5" class="data row12 col5" >77.1%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col6" class="data row12 col6" >66.1%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col7" class="data row12 col7" >81.2%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col8" class="data row12 col8" >73.6%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row12_col9" class="data row12 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row13" class="row_heading level0 row13" >13</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col0" class="data row13 col0" >Ford High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col1" class="data row13 col1" >District</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col2" class="data row13 col2" >2739</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col3" class="data row13 col3" >$1,763,916</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col4" class="data row13 col4" >80.7%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col5" class="data row13 col5" >77.1%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col6" class="data row13 col6" >68.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col7" class="data row13 col7" >79.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col8" class="data row13 col8" >73.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row13_col9" class="data row13 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83level0_row14" class="row_heading level0 row14" >14</th> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col0" class="data row14 col0" >Thomas High School</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col1" class="data row14 col1" >Charter</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col2" class="data row14 col2" >1635</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col3" class="data row14 col3" >$1,043,130</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col4" class="data row14 col4" >83.8%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col5" class="data row14 col5" >83.4%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col6" class="data row14 col6" >93.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col7" class="data row14 col7" >97.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col8" class="data row14 col8" >95.3%</td> 
        <td id="T_a141b5b0_dc91_11e7_9ee7_a0999b1e5b83row14_col9" class="data row14 col9" >$24,649,428</td> 
    </tr></tbody> 
</table> 



#### Top Performing Schools (By Passing Rate)

Create a table that highlights the top 5 performing schools based on Overall Passing Rate. Include:
- School Name
- School Type
- Total Students
- Total School Budget
- Per School Budget
- Average Math Score
- Average Reading Score
- % Passing Math
- % Passing Reading
- Overall Passing Rate (Average of the above two)

To accomplish all of this, all we need to do is sort the (unformatted) dataframe.


```python
bypassrate = renamed.sort_values(by='% Passing Overall', ascending=False)
```


```python
top_passing_schools = pd.DataFrame(bypassrate.head())
```


```python
top_passing_schools
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Type</th>
      <th>Total Students</th>
      <th>School Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
      <th>Total Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>24649428</td>
    </tr>
  </tbody>
</table>
</div>



We can format this using the above code:


```python
formatted_2 = top_passing_schools.style.format({'School Budget':'${:,.0f}',
                                                'Total Budget':'${:,.0f}',
                                                'Average Reading Score':'{:.1f}%',
                                                'Average Math Score':'{:.1f}%',
                                                '% Passing Math':'{:.1f}%',
                                                '% Passing Reading':'{:.1f}%',
                                                '% Passing Overall':'{:.1f}%'})

formatted_2
```




<style  type="text/css" >
</style>  
<table id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School</th> 
        <th class="col_heading level0 col1" >Type</th> 
        <th class="col_heading level0 col2" >Total Students</th> 
        <th class="col_heading level0 col3" >School Budget</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >Average Math Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Passing Overall</th> 
        <th class="col_heading level0 col9" >Total Budget</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83level0_row0" class="row_heading level0 row0" >6</th> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col0" class="data row0 col0" >Cabrera High School</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col1" class="data row0 col1" >Charter</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col2" class="data row0 col2" >1858</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col3" class="data row0 col3" >$1,081,356</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col4" class="data row0 col4" >84.0%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col5" class="data row0 col5" >83.1%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col6" class="data row0 col6" >94.1%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col7" class="data row0 col7" >97.0%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col8" class="data row0 col8" >95.6%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row0_col9" class="data row0 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83level0_row1" class="row_heading level0 row1" >14</th> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col0" class="data row1 col0" >Thomas High School</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col1" class="data row1 col1" >Charter</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col2" class="data row1 col2" >1635</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col3" class="data row1 col3" >$1,043,130</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col4" class="data row1 col4" >83.8%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col5" class="data row1 col5" >83.4%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col6" class="data row1 col6" >93.3%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col7" class="data row1 col7" >97.3%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col8" class="data row1 col8" >95.3%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row1_col9" class="data row1 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83level0_row2" class="row_heading level0 row2" >9</th> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col0" class="data row2 col0" >Pena High School</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col1" class="data row2 col1" >Charter</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col2" class="data row2 col2" >962</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col3" class="data row2 col3" >$585,858</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col4" class="data row2 col4" >84.0%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col5" class="data row2 col5" >83.8%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col6" class="data row2 col6" >94.6%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col7" class="data row2 col7" >95.9%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col8" class="data row2 col8" >95.3%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row2_col9" class="data row2 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83level0_row3" class="row_heading level0 row3" >4</th> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col0" class="data row3 col0" >Griffin High School</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col1" class="data row3 col1" >Charter</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col2" class="data row3 col2" >1468</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col3" class="data row3 col3" >$917,500</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col4" class="data row3 col4" >83.8%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col5" class="data row3 col5" >83.4%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col6" class="data row3 col6" >93.4%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col7" class="data row3 col7" >97.1%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col8" class="data row3 col8" >95.3%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row3_col9" class="data row3 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83level0_row4" class="row_heading level0 row4" >5</th> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col0" class="data row4 col0" >Wilson High School</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col1" class="data row4 col1" >Charter</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col2" class="data row4 col2" >2283</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col3" class="data row4 col3" >$1,319,574</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col4" class="data row4 col4" >84.0%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col5" class="data row4 col5" >83.3%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col6" class="data row4 col6" >93.9%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col7" class="data row4 col7" >96.5%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col8" class="data row4 col8" >95.2%</td> 
        <td id="T_a2f0fd6c_dc91_11e7_83f0_a0999b1e5b83row4_col9" class="data row4 col9" >$24,649,428</td> 
    </tr></tbody> 
</table> 



#### Bottom performing schools:

Will do the same for bottom five schools based on % passing overall


```python
bypassrate_asc = renamed.sort_values(by='% Passing Overall')
bottom_passing_schools = pd.DataFrame(bypassrate_asc.head())


formatted_3 = bottom_passing_schools.style.format({'School Budget':'${:,.0f}',
                                                   'Total Budget':'${:,.0f}',
                                                   'Average Reading Score':'{:.1f}%',
                                                   'Average Math Score':'{:.1f}%',
                                                   '% Passing Math':'{:.1f}%',
                                                   '% Passing Reading':'{:.1f}%',
                                                   '% Passing Overall':'{:.1f}%'})
formatted_3
```




<style  type="text/css" >
</style>  
<table id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School</th> 
        <th class="col_heading level0 col1" >Type</th> 
        <th class="col_heading level0 col2" >Total Students</th> 
        <th class="col_heading level0 col3" >School Budget</th> 
        <th class="col_heading level0 col4" >Average Reading Score</th> 
        <th class="col_heading level0 col5" >Average Math Score</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >% Passing Reading</th> 
        <th class="col_heading level0 col8" >% Passing Overall</th> 
        <th class="col_heading level0 col9" >Total Budget</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83level0_row0" class="row_heading level0 row0" >11</th> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col0" class="data row0 col0" >Rodriguez High School</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col1" class="data row0 col1" >District</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col2" class="data row0 col2" >3999</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col3" class="data row0 col3" >$2,547,363</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col4" class="data row0 col4" >80.7%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col5" class="data row0 col5" >76.8%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col6" class="data row0 col6" >66.4%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col7" class="data row0 col7" >80.2%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col8" class="data row0 col8" >73.3%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row0_col9" class="data row0 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83level0_row1" class="row_heading level0 row1" >1</th> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col0" class="data row1 col0" >Figueroa High School</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col1" class="data row1 col1" >District</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col2" class="data row1 col2" >2949</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col3" class="data row1 col3" >$1,884,411</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col4" class="data row1 col4" >81.2%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col5" class="data row1 col5" >76.7%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col6" class="data row1 col6" >66.0%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col7" class="data row1 col7" >80.7%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col8" class="data row1 col8" >73.4%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row1_col9" class="data row1 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83level0_row2" class="row_heading level0 row2" >0</th> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col0" class="data row2 col0" >Huang High School</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col1" class="data row2 col1" >District</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col2" class="data row2 col2" >2917</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col3" class="data row2 col3" >$1,910,635</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col4" class="data row2 col4" >81.2%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col5" class="data row2 col5" >76.6%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col6" class="data row2 col6" >65.7%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col7" class="data row2 col7" >81.3%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col8" class="data row2 col8" >73.5%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row2_col9" class="data row2 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83level0_row3" class="row_heading level0 row3" >12</th> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col0" class="data row3 col0" >Johnson High School</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col1" class="data row3 col1" >District</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col2" class="data row3 col2" >4761</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col3" class="data row3 col3" >$3,094,650</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col4" class="data row3 col4" >81.0%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col5" class="data row3 col5" >77.1%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col6" class="data row3 col6" >66.1%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col7" class="data row3 col7" >81.2%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col8" class="data row3 col8" >73.6%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row3_col9" class="data row3 col9" >$24,649,428</td> 
    </tr>    <tr> 
        <th id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83level0_row4" class="row_heading level0 row4" >13</th> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col0" class="data row4 col0" >Ford High School</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col1" class="data row4 col1" >District</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col2" class="data row4 col2" >2739</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col3" class="data row4 col3" >$1,763,916</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col4" class="data row4 col4" >80.7%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col5" class="data row4 col5" >77.1%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col6" class="data row4 col6" >68.3%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col7" class="data row4 col7" >79.3%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col8" class="data row4 col8" >73.8%</td> 
        <td id="T_a34f1a50_dc91_11e7_8bc7_a0999b1e5b83row4_col9" class="data row4 col9" >$24,649,428</td> 
    </tr></tbody> 
</table> 



#### Math Scores by Grade
 - Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.

This will be a bit easier because it is asking for only a scalar summary for 2 nested groups.


```python
answer1 = pd.DataFrame(students.groupby(['school', 'grade'])['math_score'].mean())
```

This is the same as:

```python
group = students.groupby(['school', 'grade'])
returned = group['math_score'].mean()
answer1 = pd.DataFrame(returned)
```


```python
answer1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>math_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th>grade</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Bailey High School</th>
      <th>10th</th>
      <td>76.996772</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.515588</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.083676</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Cabrera High School</th>
      <th>10th</th>
      <td>83.154506</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>82.765560</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.094697</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Figueroa High School</th>
      <th>10th</th>
      <td>76.539974</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.884344</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>76.403037</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Ford High School</th>
      <th>10th</th>
      <td>77.672316</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.918058</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.361345</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Griffin High School</th>
      <th>10th</th>
      <td>84.229064</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.842105</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>82.044010</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Hernandez High School</th>
      <th>10th</th>
      <td>77.337408</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.136029</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.438495</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Holden High School</th>
      <th>10th</th>
      <td>83.429825</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>85.000000</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.787402</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Huang High School</th>
      <th>10th</th>
      <td>75.908735</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.446602</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.027251</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Johnson High School</th>
      <th>10th</th>
      <td>76.691117</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.491653</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>77.187857</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Pena High School</th>
      <th>10th</th>
      <td>83.372000</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.328125</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.625455</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Rodriguez High School</th>
      <th>10th</th>
      <td>76.612500</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.395626</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>76.859966</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Shelton High School</th>
      <th>10th</th>
      <td>82.917411</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.383495</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.420755</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Thomas High School</th>
      <th>10th</th>
      <td>83.087886</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.498795</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.590022</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wilson High School</th>
      <th>10th</th>
      <td>83.724422</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.195326</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.085578</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wright High School</th>
      <th>10th</th>
      <td>84.010288</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.836782</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.644986</td>
    </tr>
    <tr>
      <th>9th</th>
      <td>83.264706</td>
    </tr>
  </tbody>
</table>
</div>



We can change the order of displayed grades by using the ```.reindex()``` function on the groupby result:


```python
answer1 = answer1.reindex(['9th', '10th', '11th', '12th'], level='grade')
answer1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>math_score</th>
    </tr>
    <tr>
      <th>school</th>
      <th>grade</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="4" valign="top">Bailey High School</th>
      <th>9th</th>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>76.996772</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.515588</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.492218</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Cabrera High School</th>
      <th>9th</th>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>83.154506</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>82.765560</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.277487</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Figueroa High School</th>
      <th>9th</th>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>76.539974</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.884344</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.151369</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Ford High School</th>
      <th>9th</th>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>77.672316</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.918058</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.179963</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Griffin High School</th>
      <th>9th</th>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>84.229064</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.842105</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.356164</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Hernandez High School</th>
      <th>9th</th>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>77.337408</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.136029</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.186567</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Holden High School</th>
      <th>9th</th>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>83.429825</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>85.000000</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>82.855422</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Huang High School</th>
      <th>9th</th>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>75.908735</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.446602</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.225641</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Johnson High School</th>
      <th>9th</th>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>76.691117</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>77.491653</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>76.863248</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Pena High School</th>
      <th>9th</th>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>83.372000</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>84.328125</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>84.121547</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Rodriguez High School</th>
      <th>9th</th>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>76.612500</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>76.395626</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>77.690748</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Shelton High School</th>
      <th>9th</th>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>82.917411</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.383495</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.778976</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Thomas High School</th>
      <th>9th</th>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>83.087886</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.498795</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.497041</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wilson High School</th>
      <th>9th</th>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>83.724422</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.195326</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.035794</td>
    </tr>
    <tr>
      <th rowspan="4" valign="top">Wright High School</th>
      <th>9th</th>
      <td>83.264706</td>
    </tr>
    <tr>
      <th>10th</th>
      <td>84.010288</td>
    </tr>
    <tr>
      <th>11th</th>
      <td>83.836782</td>
    </tr>
    <tr>
      <th>12th</th>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



Now we need to:
- rename columns
- format math_score


```python
answer2 = answer1.reset_index()
answer2
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>school</th>
      <th>grade</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>9th</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bailey High School</td>
      <td>10th</td>
      <td>76.996772</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bailey High School</td>
      <td>11th</td>
      <td>77.515588</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bailey High School</td>
      <td>12th</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cabrera High School</td>
      <td>9th</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Cabrera High School</td>
      <td>10th</td>
      <td>83.154506</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>11th</td>
      <td>82.765560</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Cabrera High School</td>
      <td>12th</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Figueroa High School</td>
      <td>9th</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Figueroa High School</td>
      <td>10th</td>
      <td>76.539974</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Figueroa High School</td>
      <td>11th</td>
      <td>76.884344</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Figueroa High School</td>
      <td>12th</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Ford High School</td>
      <td>9th</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>10th</td>
      <td>77.672316</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Ford High School</td>
      <td>11th</td>
      <td>76.918058</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ford High School</td>
      <td>12th</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Griffin High School</td>
      <td>9th</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Griffin High School</td>
      <td>10th</td>
      <td>84.229064</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Griffin High School</td>
      <td>11th</td>
      <td>83.842105</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Griffin High School</td>
      <td>12th</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Hernandez High School</td>
      <td>9th</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Hernandez High School</td>
      <td>10th</td>
      <td>77.337408</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Hernandez High School</td>
      <td>11th</td>
      <td>77.136029</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Hernandez High School</td>
      <td>12th</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Holden High School</td>
      <td>9th</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Holden High School</td>
      <td>10th</td>
      <td>83.429825</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Holden High School</td>
      <td>11th</td>
      <td>85.000000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Holden High School</td>
      <td>12th</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Huang High School</td>
      <td>9th</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Huang High School</td>
      <td>10th</td>
      <td>75.908735</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Huang High School</td>
      <td>11th</td>
      <td>76.446602</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Huang High School</td>
      <td>12th</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Johnson High School</td>
      <td>9th</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Johnson High School</td>
      <td>10th</td>
      <td>76.691117</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Johnson High School</td>
      <td>11th</td>
      <td>77.491653</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Johnson High School</td>
      <td>12th</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Pena High School</td>
      <td>9th</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pena High School</td>
      <td>10th</td>
      <td>83.372000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Pena High School</td>
      <td>11th</td>
      <td>84.328125</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Pena High School</td>
      <td>12th</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Rodriguez High School</td>
      <td>9th</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Rodriguez High School</td>
      <td>10th</td>
      <td>76.612500</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Rodriguez High School</td>
      <td>11th</td>
      <td>76.395626</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Rodriguez High School</td>
      <td>12th</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Shelton High School</td>
      <td>9th</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Shelton High School</td>
      <td>10th</td>
      <td>82.917411</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Shelton High School</td>
      <td>11th</td>
      <td>83.383495</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Shelton High School</td>
      <td>12th</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Thomas High School</td>
      <td>9th</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Thomas High School</td>
      <td>10th</td>
      <td>83.087886</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Thomas High School</td>
      <td>11th</td>
      <td>83.498795</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Thomas High School</td>
      <td>12th</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Wilson High School</td>
      <td>9th</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Wilson High School</td>
      <td>10th</td>
      <td>83.724422</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Wilson High School</td>
      <td>11th</td>
      <td>83.195326</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Wilson High School</td>
      <td>12th</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Wright High School</td>
      <td>9th</td>
      <td>83.264706</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Wright High School</td>
      <td>10th</td>
      <td>84.010288</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Wright High School</td>
      <td>11th</td>
      <td>83.836782</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Wright High School</td>
      <td>12th</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
answer3 = answer2.rename(columns={'school':'School',
                                  'grade':'Grade',
                                  'math_score':'Average Math Score'})
answer3
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Grade</th>
      <th>Average Math Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bailey High School</td>
      <td>9th</td>
      <td>77.083676</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Bailey High School</td>
      <td>10th</td>
      <td>76.996772</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Bailey High School</td>
      <td>11th</td>
      <td>77.515588</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bailey High School</td>
      <td>12th</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cabrera High School</td>
      <td>9th</td>
      <td>83.094697</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Cabrera High School</td>
      <td>10th</td>
      <td>83.154506</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>11th</td>
      <td>82.765560</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Cabrera High School</td>
      <td>12th</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Figueroa High School</td>
      <td>9th</td>
      <td>76.403037</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Figueroa High School</td>
      <td>10th</td>
      <td>76.539974</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Figueroa High School</td>
      <td>11th</td>
      <td>76.884344</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Figueroa High School</td>
      <td>12th</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Ford High School</td>
      <td>9th</td>
      <td>77.361345</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>10th</td>
      <td>77.672316</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Ford High School</td>
      <td>11th</td>
      <td>76.918058</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Ford High School</td>
      <td>12th</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Griffin High School</td>
      <td>9th</td>
      <td>82.044010</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Griffin High School</td>
      <td>10th</td>
      <td>84.229064</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Griffin High School</td>
      <td>11th</td>
      <td>83.842105</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Griffin High School</td>
      <td>12th</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Hernandez High School</td>
      <td>9th</td>
      <td>77.438495</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Hernandez High School</td>
      <td>10th</td>
      <td>77.337408</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Hernandez High School</td>
      <td>11th</td>
      <td>77.136029</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Hernandez High School</td>
      <td>12th</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Holden High School</td>
      <td>9th</td>
      <td>83.787402</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Holden High School</td>
      <td>10th</td>
      <td>83.429825</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Holden High School</td>
      <td>11th</td>
      <td>85.000000</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Holden High School</td>
      <td>12th</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Huang High School</td>
      <td>9th</td>
      <td>77.027251</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Huang High School</td>
      <td>10th</td>
      <td>75.908735</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Huang High School</td>
      <td>11th</td>
      <td>76.446602</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Huang High School</td>
      <td>12th</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Johnson High School</td>
      <td>9th</td>
      <td>77.187857</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Johnson High School</td>
      <td>10th</td>
      <td>76.691117</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Johnson High School</td>
      <td>11th</td>
      <td>77.491653</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Johnson High School</td>
      <td>12th</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Pena High School</td>
      <td>9th</td>
      <td>83.625455</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pena High School</td>
      <td>10th</td>
      <td>83.372000</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Pena High School</td>
      <td>11th</td>
      <td>84.328125</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Pena High School</td>
      <td>12th</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Rodriguez High School</td>
      <td>9th</td>
      <td>76.859966</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Rodriguez High School</td>
      <td>10th</td>
      <td>76.612500</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Rodriguez High School</td>
      <td>11th</td>
      <td>76.395626</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Rodriguez High School</td>
      <td>12th</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Shelton High School</td>
      <td>9th</td>
      <td>83.420755</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Shelton High School</td>
      <td>10th</td>
      <td>82.917411</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Shelton High School</td>
      <td>11th</td>
      <td>83.383495</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Shelton High School</td>
      <td>12th</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Thomas High School</td>
      <td>9th</td>
      <td>83.590022</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Thomas High School</td>
      <td>10th</td>
      <td>83.087886</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Thomas High School</td>
      <td>11th</td>
      <td>83.498795</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Thomas High School</td>
      <td>12th</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Wilson High School</td>
      <td>9th</td>
      <td>83.085578</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Wilson High School</td>
      <td>10th</td>
      <td>83.724422</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Wilson High School</td>
      <td>11th</td>
      <td>83.195326</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Wilson High School</td>
      <td>12th</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Wright High School</td>
      <td>9th</td>
      <td>83.264706</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Wright High School</td>
      <td>10th</td>
      <td>84.010288</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Wright High School</td>
      <td>11th</td>
      <td>83.836782</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Wright High School</td>
      <td>12th</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>




```python
answer4 = answer3.style.format({'Average Math Score':'{:.1f}%'})
answer4
```




<style  type="text/css" >
</style>  
<table id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School</th> 
        <th class="col_heading level0 col1" >Grade</th> 
        <th class="col_heading level0 col2" >Average Math Score</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row0_col0" class="data row0 col0" >Bailey High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row0_col1" class="data row0 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row0_col2" class="data row0 col2" >77.1%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row1" class="row_heading level0 row1" >1</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row1_col0" class="data row1 col0" >Bailey High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row1_col1" class="data row1 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row1_col2" class="data row1 col2" >77.0%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row2" class="row_heading level0 row2" >2</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row2_col0" class="data row2 col0" >Bailey High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row2_col1" class="data row2 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row2_col2" class="data row2 col2" >77.5%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row3" class="row_heading level0 row3" >3</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row3_col0" class="data row3 col0" >Bailey High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row3_col1" class="data row3 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row3_col2" class="data row3 col2" >76.5%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row4" class="row_heading level0 row4" >4</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row4_col0" class="data row4 col0" >Cabrera High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row4_col1" class="data row4 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row4_col2" class="data row4 col2" >83.1%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row5" class="row_heading level0 row5" >5</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row5_col0" class="data row5 col0" >Cabrera High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row5_col1" class="data row5 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row5_col2" class="data row5 col2" >83.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row6" class="row_heading level0 row6" >6</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row6_col0" class="data row6 col0" >Cabrera High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row6_col1" class="data row6 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row6_col2" class="data row6 col2" >82.8%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row7" class="row_heading level0 row7" >7</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row7_col0" class="data row7 col0" >Cabrera High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row7_col1" class="data row7 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row7_col2" class="data row7 col2" >83.3%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row8" class="row_heading level0 row8" >8</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row8_col0" class="data row8 col0" >Figueroa High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row8_col1" class="data row8 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row8_col2" class="data row8 col2" >76.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row9" class="row_heading level0 row9" >9</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row9_col0" class="data row9 col0" >Figueroa High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row9_col1" class="data row9 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row9_col2" class="data row9 col2" >76.5%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row10" class="row_heading level0 row10" >10</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row10_col0" class="data row10 col0" >Figueroa High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row10_col1" class="data row10 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row10_col2" class="data row10 col2" >76.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row11" class="row_heading level0 row11" >11</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row11_col0" class="data row11 col0" >Figueroa High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row11_col1" class="data row11 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row11_col2" class="data row11 col2" >77.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row12" class="row_heading level0 row12" >12</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row12_col0" class="data row12 col0" >Ford High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row12_col1" class="data row12 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row12_col2" class="data row12 col2" >77.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row13" class="row_heading level0 row13" >13</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row13_col0" class="data row13 col0" >Ford High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row13_col1" class="data row13 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row13_col2" class="data row13 col2" >77.7%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row14" class="row_heading level0 row14" >14</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row14_col0" class="data row14 col0" >Ford High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row14_col1" class="data row14 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row14_col2" class="data row14 col2" >76.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row15" class="row_heading level0 row15" >15</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row15_col0" class="data row15 col0" >Ford High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row15_col1" class="data row15 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row15_col2" class="data row15 col2" >76.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row16" class="row_heading level0 row16" >16</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row16_col0" class="data row16 col0" >Griffin High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row16_col1" class="data row16 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row16_col2" class="data row16 col2" >82.0%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row17" class="row_heading level0 row17" >17</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row17_col0" class="data row17 col0" >Griffin High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row17_col1" class="data row17 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row17_col2" class="data row17 col2" >84.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row18" class="row_heading level0 row18" >18</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row18_col0" class="data row18 col0" >Griffin High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row18_col1" class="data row18 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row18_col2" class="data row18 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row19" class="row_heading level0 row19" >19</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row19_col0" class="data row19 col0" >Griffin High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row19_col1" class="data row19 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row19_col2" class="data row19 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row20" class="row_heading level0 row20" >20</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row20_col0" class="data row20 col0" >Hernandez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row20_col1" class="data row20 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row20_col2" class="data row20 col2" >77.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row21" class="row_heading level0 row21" >21</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row21_col0" class="data row21 col0" >Hernandez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row21_col1" class="data row21 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row21_col2" class="data row21 col2" >77.3%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row22" class="row_heading level0 row22" >22</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row22_col0" class="data row22 col0" >Hernandez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row22_col1" class="data row22 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row22_col2" class="data row22 col2" >77.1%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row23" class="row_heading level0 row23" >23</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row23_col0" class="data row23 col0" >Hernandez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row23_col1" class="data row23 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row23_col2" class="data row23 col2" >77.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row24" class="row_heading level0 row24" >24</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row24_col0" class="data row24 col0" >Holden High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row24_col1" class="data row24 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row24_col2" class="data row24 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row25" class="row_heading level0 row25" >25</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row25_col0" class="data row25 col0" >Holden High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row25_col1" class="data row25 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row25_col2" class="data row25 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row26" class="row_heading level0 row26" >26</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row26_col0" class="data row26 col0" >Holden High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row26_col1" class="data row26 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row26_col2" class="data row26 col2" >85.0%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row27" class="row_heading level0 row27" >27</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row27_col0" class="data row27 col0" >Holden High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row27_col1" class="data row27 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row27_col2" class="data row27 col2" >82.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row28" class="row_heading level0 row28" >28</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row28_col0" class="data row28 col0" >Huang High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row28_col1" class="data row28 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row28_col2" class="data row28 col2" >77.0%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row29" class="row_heading level0 row29" >29</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row29_col0" class="data row29 col0" >Huang High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row29_col1" class="data row29 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row29_col2" class="data row29 col2" >75.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row30" class="row_heading level0 row30" >30</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row30_col0" class="data row30 col0" >Huang High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row30_col1" class="data row30 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row30_col2" class="data row30 col2" >76.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row31" class="row_heading level0 row31" >31</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row31_col0" class="data row31 col0" >Huang High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row31_col1" class="data row31 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row31_col2" class="data row31 col2" >77.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row32" class="row_heading level0 row32" >32</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row32_col0" class="data row32 col0" >Johnson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row32_col1" class="data row32 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row32_col2" class="data row32 col2" >77.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row33" class="row_heading level0 row33" >33</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row33_col0" class="data row33 col0" >Johnson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row33_col1" class="data row33 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row33_col2" class="data row33 col2" >76.7%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row34" class="row_heading level0 row34" >34</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row34_col0" class="data row34 col0" >Johnson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row34_col1" class="data row34 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row34_col2" class="data row34 col2" >77.5%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row35" class="row_heading level0 row35" >35</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row35_col0" class="data row35 col0" >Johnson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row35_col1" class="data row35 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row35_col2" class="data row35 col2" >76.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row36" class="row_heading level0 row36" >36</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row36_col0" class="data row36 col0" >Pena High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row36_col1" class="data row36 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row36_col2" class="data row36 col2" >83.6%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row37" class="row_heading level0 row37" >37</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row37_col0" class="data row37 col0" >Pena High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row37_col1" class="data row37 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row37_col2" class="data row37 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row38" class="row_heading level0 row38" >38</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row38_col0" class="data row38 col0" >Pena High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row38_col1" class="data row38 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row38_col2" class="data row38 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row39" class="row_heading level0 row39" >39</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row39_col0" class="data row39 col0" >Pena High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row39_col1" class="data row39 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row39_col2" class="data row39 col2" >84.1%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row40" class="row_heading level0 row40" >40</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row40_col0" class="data row40 col0" >Rodriguez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row40_col1" class="data row40 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row40_col2" class="data row40 col2" >76.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row41" class="row_heading level0 row41" >41</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row41_col0" class="data row41 col0" >Rodriguez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row41_col1" class="data row41 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row41_col2" class="data row41 col2" >76.6%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row42" class="row_heading level0 row42" >42</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row42_col0" class="data row42 col0" >Rodriguez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row42_col1" class="data row42 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row42_col2" class="data row42 col2" >76.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row43" class="row_heading level0 row43" >43</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row43_col0" class="data row43 col0" >Rodriguez High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row43_col1" class="data row43 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row43_col2" class="data row43 col2" >77.7%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row44" class="row_heading level0 row44" >44</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row44_col0" class="data row44 col0" >Shelton High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row44_col1" class="data row44 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row44_col2" class="data row44 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row45" class="row_heading level0 row45" >45</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row45_col0" class="data row45 col0" >Shelton High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row45_col1" class="data row45 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row45_col2" class="data row45 col2" >82.9%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row46" class="row_heading level0 row46" >46</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row46_col0" class="data row46 col0" >Shelton High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row46_col1" class="data row46 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row46_col2" class="data row46 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row47" class="row_heading level0 row47" >47</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row47_col0" class="data row47 col0" >Shelton High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row47_col1" class="data row47 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row47_col2" class="data row47 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row48" class="row_heading level0 row48" >48</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row48_col0" class="data row48 col0" >Thomas High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row48_col1" class="data row48 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row48_col2" class="data row48 col2" >83.6%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row49" class="row_heading level0 row49" >49</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row49_col0" class="data row49 col0" >Thomas High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row49_col1" class="data row49 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row49_col2" class="data row49 col2" >83.1%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row50" class="row_heading level0 row50" >50</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row50_col0" class="data row50 col0" >Thomas High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row50_col1" class="data row50 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row50_col2" class="data row50 col2" >83.5%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row51" class="row_heading level0 row51" >51</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row51_col0" class="data row51 col0" >Thomas High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row51_col1" class="data row51 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row51_col2" class="data row51 col2" >83.5%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row52" class="row_heading level0 row52" >52</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row52_col0" class="data row52 col0" >Wilson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row52_col1" class="data row52 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row52_col2" class="data row52 col2" >83.1%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row53" class="row_heading level0 row53" >53</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row53_col0" class="data row53 col0" >Wilson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row53_col1" class="data row53 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row53_col2" class="data row53 col2" >83.7%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row54" class="row_heading level0 row54" >54</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row54_col0" class="data row54 col0" >Wilson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row54_col1" class="data row54 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row54_col2" class="data row54 col2" >83.2%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row55" class="row_heading level0 row55" >55</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row55_col0" class="data row55 col0" >Wilson High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row55_col1" class="data row55 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row55_col2" class="data row55 col2" >83.0%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row56" class="row_heading level0 row56" >56</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row56_col0" class="data row56 col0" >Wright High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row56_col1" class="data row56 col1" >9th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row56_col2" class="data row56 col2" >83.3%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row57" class="row_heading level0 row57" >57</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row57_col0" class="data row57 col0" >Wright High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row57_col1" class="data row57 col1" >10th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row57_col2" class="data row57 col2" >84.0%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row58" class="row_heading level0 row58" >58</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row58_col0" class="data row58 col0" >Wright High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row58_col1" class="data row58 col1" >11th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row58_col2" class="data row58 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83level0_row59" class="row_heading level0 row59" >59</th> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row59_col0" class="data row59 col0" >Wright High School</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row59_col1" class="data row59 col1" >12th</td> 
        <td id="T_a5c11358_dc91_11e7_a8c9_a0999b1e5b83row59_col2" class="data row59 col2" >83.6%</td> 
    </tr></tbody> 
</table> 



#### Reading scores by grade:
We will do the same for reading scores as above:


```python
answer5 = pd.DataFrame(students.groupby(['school', 'grade'])['reading_score'].mean())
answer5 = answer5.reindex(['9th', '10th', '11th', '12th'], level='grade')
answer6 = answer5.reset_index()
answer7 = answer6.rename(columns={'school':'School',
                                  'grade':'Grade',
                                  'reading_score':'Average Reading Score'})
answer8 = answer7.style.format({'Average Reading Score':'{:.1f}%'})
answer8
```




<style  type="text/css" >
</style>  
<table id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >School</th> 
        <th class="col_heading level0 col1" >Grade</th> 
        <th class="col_heading level0 col2" >Average Reading Score</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row0" class="row_heading level0 row0" >0</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row0_col0" class="data row0 col0" >Bailey High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row0_col1" class="data row0 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row0_col2" class="data row0 col2" >81.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row1" class="row_heading level0 row1" >1</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row1_col0" class="data row1 col0" >Bailey High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row1_col1" class="data row1 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row1_col2" class="data row1 col2" >80.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row2" class="row_heading level0 row2" >2</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row2_col0" class="data row2 col0" >Bailey High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row2_col1" class="data row2 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row2_col2" class="data row2 col2" >80.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row3" class="row_heading level0 row3" >3</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row3_col0" class="data row3 col0" >Bailey High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row3_col1" class="data row3 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row3_col2" class="data row3 col2" >80.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row4" class="row_heading level0 row4" >4</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row4_col0" class="data row4 col0" >Cabrera High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row4_col1" class="data row4 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row4_col2" class="data row4 col2" >83.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row5" class="row_heading level0 row5" >5</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row5_col0" class="data row5 col0" >Cabrera High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row5_col1" class="data row5 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row5_col2" class="data row5 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row6" class="row_heading level0 row6" >6</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row6_col0" class="data row6 col0" >Cabrera High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row6_col1" class="data row6 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row6_col2" class="data row6 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row7" class="row_heading level0 row7" >7</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row7_col0" class="data row7 col0" >Cabrera High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row7_col1" class="data row7 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row7_col2" class="data row7 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row8" class="row_heading level0 row8" >8</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row8_col0" class="data row8 col0" >Figueroa High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row8_col1" class="data row8 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row8_col2" class="data row8 col2" >81.2%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row9" class="row_heading level0 row9" >9</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row9_col0" class="data row9 col0" >Figueroa High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row9_col1" class="data row9 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row9_col2" class="data row9 col2" >81.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row10" class="row_heading level0 row10" >10</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row10_col0" class="data row10 col0" >Figueroa High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row10_col1" class="data row10 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row10_col2" class="data row10 col2" >80.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row11" class="row_heading level0 row11" >11</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row11_col0" class="data row11 col0" >Figueroa High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row11_col1" class="data row11 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row11_col2" class="data row11 col2" >81.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row12" class="row_heading level0 row12" >12</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row12_col0" class="data row12 col0" >Ford High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row12_col1" class="data row12 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row12_col2" class="data row12 col2" >80.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row13" class="row_heading level0 row13" >13</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row13_col0" class="data row13 col0" >Ford High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row13_col1" class="data row13 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row13_col2" class="data row13 col2" >81.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row14" class="row_heading level0 row14" >14</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row14_col0" class="data row14 col0" >Ford High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row14_col1" class="data row14 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row14_col2" class="data row14 col2" >80.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row15" class="row_heading level0 row15" >15</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row15_col0" class="data row15 col0" >Ford High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row15_col1" class="data row15 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row15_col2" class="data row15 col2" >80.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row16" class="row_heading level0 row16" >16</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row16_col0" class="data row16 col0" >Griffin High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row16_col1" class="data row16 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row16_col2" class="data row16 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row17" class="row_heading level0 row17" >17</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row17_col0" class="data row17 col0" >Griffin High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row17_col1" class="data row17 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row17_col2" class="data row17 col2" >83.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row18" class="row_heading level0 row18" >18</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row18_col0" class="data row18 col0" >Griffin High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row18_col1" class="data row18 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row18_col2" class="data row18 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row19" class="row_heading level0 row19" >19</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row19_col0" class="data row19 col0" >Griffin High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row19_col1" class="data row19 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row19_col2" class="data row19 col2" >84.0%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row20" class="row_heading level0 row20" >20</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row20_col0" class="data row20 col0" >Hernandez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row20_col1" class="data row20 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row20_col2" class="data row20 col2" >80.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row21" class="row_heading level0 row21" >21</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row21_col0" class="data row21 col0" >Hernandez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row21_col1" class="data row21 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row21_col2" class="data row21 col2" >80.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row22" class="row_heading level0 row22" >22</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row22_col0" class="data row22 col0" >Hernandez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row22_col1" class="data row22 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row22_col2" class="data row22 col2" >81.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row23" class="row_heading level0 row23" >23</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row23_col0" class="data row23 col0" >Hernandez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row23_col1" class="data row23 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row23_col2" class="data row23 col2" >80.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row24" class="row_heading level0 row24" >24</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row24_col0" class="data row24 col0" >Holden High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row24_col1" class="data row24 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row24_col2" class="data row24 col2" >83.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row25" class="row_heading level0 row25" >25</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row25_col0" class="data row25 col0" >Holden High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row25_col1" class="data row25 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row25_col2" class="data row25 col2" >83.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row26" class="row_heading level0 row26" >26</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row26_col0" class="data row26 col0" >Holden High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row26_col1" class="data row26 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row26_col2" class="data row26 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row27" class="row_heading level0 row27" >27</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row27_col0" class="data row27 col0" >Holden High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row27_col1" class="data row27 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row27_col2" class="data row27 col2" >84.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row28" class="row_heading level0 row28" >28</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row28_col0" class="data row28 col0" >Huang High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row28_col1" class="data row28 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row28_col2" class="data row28 col2" >81.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row29" class="row_heading level0 row29" >29</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row29_col0" class="data row29 col0" >Huang High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row29_col1" class="data row29 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row29_col2" class="data row29 col2" >81.5%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row30" class="row_heading level0 row30" >30</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row30_col0" class="data row30 col0" >Huang High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row30_col1" class="data row30 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row30_col2" class="data row30 col2" >81.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row31" class="row_heading level0 row31" >31</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row31_col0" class="data row31 col0" >Huang High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row31_col1" class="data row31 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row31_col2" class="data row31 col2" >80.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row32" class="row_heading level0 row32" >32</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row32_col0" class="data row32 col0" >Johnson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row32_col1" class="data row32 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row32_col2" class="data row32 col2" >81.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row33" class="row_heading level0 row33" >33</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row33_col0" class="data row33 col0" >Johnson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row33_col1" class="data row33 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row33_col2" class="data row33 col2" >80.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row34" class="row_heading level0 row34" >34</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row34_col0" class="data row34 col0" >Johnson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row34_col1" class="data row34 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row34_col2" class="data row34 col2" >80.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row35" class="row_heading level0 row35" >35</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row35_col0" class="data row35 col0" >Johnson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row35_col1" class="data row35 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row35_col2" class="data row35 col2" >81.2%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row36" class="row_heading level0 row36" >36</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row36_col0" class="data row36 col0" >Pena High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row36_col1" class="data row36 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row36_col2" class="data row36 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row37" class="row_heading level0 row37" >37</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row37_col0" class="data row37 col0" >Pena High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row37_col1" class="data row37 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row37_col2" class="data row37 col2" >83.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row38" class="row_heading level0 row38" >38</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row38_col0" class="data row38 col0" >Pena High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row38_col1" class="data row38 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row38_col2" class="data row38 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row39" class="row_heading level0 row39" >39</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row39_col0" class="data row39 col0" >Pena High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row39_col1" class="data row39 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row39_col2" class="data row39 col2" >84.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row40" class="row_heading level0 row40" >40</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row40_col0" class="data row40 col0" >Rodriguez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row40_col1" class="data row40 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row40_col2" class="data row40 col2" >81.0%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row41" class="row_heading level0 row41" >41</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row41_col0" class="data row41 col0" >Rodriguez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row41_col1" class="data row41 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row41_col2" class="data row41 col2" >80.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row42" class="row_heading level0 row42" >42</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row42_col0" class="data row42 col0" >Rodriguez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row42_col1" class="data row42 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row42_col2" class="data row42 col2" >80.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row43" class="row_heading level0 row43" >43</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row43_col0" class="data row43 col0" >Rodriguez High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row43_col1" class="data row43 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row43_col2" class="data row43 col2" >80.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row44" class="row_heading level0 row44" >44</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row44_col0" class="data row44 col0" >Shelton High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row44_col1" class="data row44 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row44_col2" class="data row44 col2" >84.1%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row45" class="row_heading level0 row45" >45</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row45_col0" class="data row45 col0" >Shelton High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row45_col1" class="data row45 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row45_col2" class="data row45 col2" >83.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row46" class="row_heading level0 row46" >46</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row46_col0" class="data row46 col0" >Shelton High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row46_col1" class="data row46 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row46_col2" class="data row46 col2" >84.4%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row47" class="row_heading level0 row47" >47</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row47_col0" class="data row47 col0" >Shelton High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row47_col1" class="data row47 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row47_col2" class="data row47 col2" >82.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row48" class="row_heading level0 row48" >48</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row48_col0" class="data row48 col0" >Thomas High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row48_col1" class="data row48 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row48_col2" class="data row48 col2" >83.7%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row49" class="row_heading level0 row49" >49</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row49_col0" class="data row49 col0" >Thomas High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row49_col1" class="data row49 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row49_col2" class="data row49 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row50" class="row_heading level0 row50" >50</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row50_col0" class="data row50 col0" >Thomas High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row50_col1" class="data row50 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row50_col2" class="data row50 col2" >83.6%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row51" class="row_heading level0 row51" >51</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row51_col0" class="data row51 col0" >Thomas High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row51_col1" class="data row51 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row51_col2" class="data row51 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row52" class="row_heading level0 row52" >52</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row52_col0" class="data row52 col0" >Wilson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row52_col1" class="data row52 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row52_col2" class="data row52 col2" >83.9%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row53" class="row_heading level0 row53" >53</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row53_col0" class="data row53 col0" >Wilson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row53_col1" class="data row53 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row53_col2" class="data row53 col2" >84.0%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row54" class="row_heading level0 row54" >54</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row54_col0" class="data row54 col0" >Wilson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row54_col1" class="data row54 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row54_col2" class="data row54 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row55" class="row_heading level0 row55" >55</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row55_col0" class="data row55 col0" >Wilson High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row55_col1" class="data row55 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row55_col2" class="data row55 col2" >84.3%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row56" class="row_heading level0 row56" >56</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row56_col0" class="data row56 col0" >Wright High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row56_col1" class="data row56 col1" >9th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row56_col2" class="data row56 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row57" class="row_heading level0 row57" >57</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row57_col0" class="data row57 col0" >Wright High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row57_col1" class="data row57 col1" >10th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row57_col2" class="data row57 col2" >83.8%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row58" class="row_heading level0 row58" >58</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row58_col0" class="data row58 col0" >Wright High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row58_col1" class="data row58 col1" >11th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row58_col2" class="data row58 col2" >84.2%</td> 
    </tr>    <tr> 
        <th id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83level0_row59" class="row_heading level0 row59" >59</th> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row59_col0" class="data row59 col0" >Wright High School</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row59_col1" class="data row59 col1" >12th</td> 
        <td id="T_a6e29606_dc91_11e7_a201_a0999b1e5b83row59_col2" class="data row59 col2" >84.1%</td> 
    </tr></tbody> 
</table> 



#### Scores by School Spending

Will create a table that breaks down school performances based on average Spending Ranges (Per Student). Will use 4 reasonable bins to group school spending. Will include in the table each of the following:
- Average Math Score
- Average Reading Score
- % Passing Math
- % Passing Reading
- Overall Passing Rate (Average of the above two)


```python
per_student = pd.DataFrame({'School': schools.school,
                            'Per Student Spending': schools.budget / schools.size})
```


```python
per_student
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Per Student Spending</th>
      <th>School</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>31843.916667</td>
      <td>Huang High School</td>
    </tr>
    <tr>
      <th>1</th>
      <td>31406.850000</td>
      <td>Figueroa High School</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17610.000000</td>
      <td>Shelton High School</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50367.000000</td>
      <td>Hernandez High School</td>
    </tr>
    <tr>
      <th>4</th>
      <td>15291.666667</td>
      <td>Griffin High School</td>
    </tr>
    <tr>
      <th>5</th>
      <td>21992.900000</td>
      <td>Wilson High School</td>
    </tr>
    <tr>
      <th>6</th>
      <td>18022.600000</td>
      <td>Cabrera High School</td>
    </tr>
    <tr>
      <th>7</th>
      <td>52082.133333</td>
      <td>Bailey High School</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4134.783333</td>
      <td>Holden High School</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9764.300000</td>
      <td>Pena High School</td>
    </tr>
    <tr>
      <th>10</th>
      <td>17490.000000</td>
      <td>Wright High School</td>
    </tr>
    <tr>
      <th>11</th>
      <td>42456.050000</td>
      <td>Rodriguez High School</td>
    </tr>
    <tr>
      <th>12</th>
      <td>51577.500000</td>
      <td>Johnson High School</td>
    </tr>
    <tr>
      <th>13</th>
      <td>29398.600000</td>
      <td>Ford High School</td>
    </tr>
    <tr>
      <th>14</th>
      <td>17385.500000</td>
      <td>Thomas High School</td>
    </tr>
  </tbody>
</table>
</div>




```python
per_student_sorted = per_student.sort_values(by='Per Student Spending', ascending=False)
```


```python
pssmax = per_student_sorted['Per Student Spending'].max()
```


```python
pssmin = per_student_sorted['Per Student Spending'].min()
```


```python
pssrng = pssmax - pssmin
```


```python
# calculate bin width by dividing range into number of desired bins
binsiz = pssrng / 4
```


```python
binsiz
```




    11986.8375




```python
# create array containing bin borders
bins = np.arange(pssmin, pssmax + binsiz, binsiz)
```


```python
bins
```




    array([  4134.78333333,  16121.62083333,  28108.45833333,  40095.29583333,
            52082.13333333])




```python
labels = ['Low', 'Below Average', 'Above Average', 'High']
```


```python
# label spending array with bin array
binned = pd.cut(per_student_sorted['Per Student Spending'], bins, labels=labels, include_lowest=True)
```


```python
binned = pd.DataFrame(binned)

```


```python
# merge bin labels with dataframe
withbins = per_student_sorted.merge(binned, how='inner', left_index=True, right_index=True)
```


```python
withbins
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Per Student Spending_x</th>
      <th>School</th>
      <th>Per Student Spending_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>52082.133333</td>
      <td>Bailey High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>12</th>
      <td>51577.500000</td>
      <td>Johnson High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>3</th>
      <td>50367.000000</td>
      <td>Hernandez High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>11</th>
      <td>42456.050000</td>
      <td>Rodriguez High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>0</th>
      <td>31843.916667</td>
      <td>Huang High School</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>1</th>
      <td>31406.850000</td>
      <td>Figueroa High School</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>13</th>
      <td>29398.600000</td>
      <td>Ford High School</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>5</th>
      <td>21992.900000</td>
      <td>Wilson High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>6</th>
      <td>18022.600000</td>
      <td>Cabrera High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17610.000000</td>
      <td>Shelton High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>10</th>
      <td>17490.000000</td>
      <td>Wright High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>14</th>
      <td>17385.500000</td>
      <td>Thomas High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>4</th>
      <td>15291.666667</td>
      <td>Griffin High School</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9764.300000</td>
      <td>Pena High School</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>8</th>
      <td>4134.783333</td>
      <td>Holden High School</td>
      <td>Low</td>
    </tr>
  </tbody>
</table>
</div>



Ok, now that we have the spending bin for each school, we should:
- remove the per student spending x column
- join this on 'school' to the students dataset
- groupby the per_student_spending bin



```python
withbins = withbins.drop('Per Student Spending_x', axis=1)
```


```python
withbins
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Per Student Spending_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>High</td>
    </tr>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Low</td>
    </tr>
  </tbody>
</table>
</div>




```python
renamed.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Type</th>
      <th>Total Students</th>
      <th>School Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
      <th>Total Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>24649428</td>
    </tr>
  </tbody>
</table>
</div>




```python
# join bin labels to large dataframe
joined = renamed.merge(withbins, on='School', how='inner')
```


```python
joined.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Type</th>
      <th>Total Students</th>
      <th>School Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
      <th>Total Budget</th>
      <th>Per Student Spending_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>24649428</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>24649428</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>24649428</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>24649428</td>
      <td>High</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>24649428</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>24649428</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>24649428</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
      <td>24649428</td>
      <td>High</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
      <td>24649428</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>24649428</td>
      <td>Low</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
      <td>24649428</td>
      <td>Below Average</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
      <td>24649428</td>
      <td>High</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
      <td>24649428</td>
      <td>High</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>24649428</td>
      <td>Above Average</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>24649428</td>
      <td>Below Average</td>
    </tr>
  </tbody>
</table>
</div>




```python
# isolate pertinent columns
subset = joined[['Per Student Spending_y',
                 'Average Math Score', 
                 'Average Reading Score', 
                 '% Passing Math', 
                 '% Passing Reading', 
                 '% Passing Overall']]
```


```python
# groupby bins for summary
spendgrp = subset.groupby('Per Student Spending_y')
```


```python
# calculate averages and display
output1 = spendgrp.mean()
output1
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
    <tr>
      <th>Per Student Spending_y</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Low</th>
      <td>83.664898</td>
      <td>83.892148</td>
      <td>93.497607</td>
      <td>96.445946</td>
      <td>94.971776</td>
    </tr>
    <tr>
      <th>Below Average</th>
      <td>83.359224</td>
      <td>83.898984</td>
      <td>93.694764</td>
      <td>96.670815</td>
      <td>95.182790</td>
    </tr>
    <tr>
      <th>Above Average</th>
      <td>76.814591</td>
      <td>81.029000</td>
      <td>66.660665</td>
      <td>80.451556</td>
      <td>73.556111</td>
    </tr>
    <tr>
      <th>High</th>
      <td>77.063340</td>
      <td>80.919864</td>
      <td>66.464293</td>
      <td>81.059691</td>
      <td>73.761992</td>
    </tr>
  </tbody>
</table>
</div>




```python
# format for readability
outputformatted = output1.style.format({'Average Math Score': '{:.1f}',
                                        'Average Reading Score': '{:.1f}',
                                        '% Passing Math': '{:.1f}%',
                                        '% Passing Reading': '{:.1f}%',
                                        '% Passing Overall': '{:.1f}%'})

outputformatted
```




<style  type="text/css" >
</style>  
<table id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Passing Overall</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Per Student Spending_y</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83level0_row0" class="row_heading level0 row0" >Low</th> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row0_col0" class="data row0 col0" >83.7</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row0_col1" class="data row0 col1" >83.9</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row0_col2" class="data row0 col2" >93.5%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row0_col3" class="data row0 col3" >96.4%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row0_col4" class="data row0 col4" >95.0%</td> 
    </tr>    <tr> 
        <th id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83level0_row1" class="row_heading level0 row1" >Below Average</th> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row1_col0" class="data row1 col0" >83.4</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row1_col1" class="data row1 col1" >83.9</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row1_col2" class="data row1 col2" >93.7%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row1_col3" class="data row1 col3" >96.7%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row1_col4" class="data row1 col4" >95.2%</td> 
    </tr>    <tr> 
        <th id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83level0_row2" class="row_heading level0 row2" >Above Average</th> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row2_col0" class="data row2 col0" >76.8</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row2_col1" class="data row2 col1" >81.0</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row2_col2" class="data row2 col2" >66.7%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row2_col3" class="data row2 col3" >80.5%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row2_col4" class="data row2 col4" >73.6%</td> 
    </tr>    <tr> 
        <th id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83level0_row3" class="row_heading level0 row3" >High</th> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row3_col0" class="data row3 col0" >77.1</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row3_col1" class="data row3 col1" >80.9</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row3_col2" class="data row3 col2" >66.5%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row3_col3" class="data row3 col3" >81.1%</td> 
        <td id="T_ed6a7f80_dc91_11e7_94d2_a0999b1e5b83row3_col4" class="data row3 col4" >73.8%</td> 
    </tr></tbody> 
</table> 



#### Performance by school size


```python
sizemax = schools['size'].max()
sizemax
```




    4976




```python
sizemin = schools['size'].min()
sizemin
```




    427




```python
sizerang = sizemax - sizemin
sizerang
```




    4549




```python
# calculate bin width as range divided by number of desired bins
binsize = sizerang / 3
binsize
```




    1516.3333333333333




```python
# create bin border array
bins = np.arange(sizemin, sizemax + binsize, binsize)
```


```python
bins
```




    array([  427.        ,  1943.33333333,  3459.66666667,  4976.        ])




```python
labels = ['Small', 'Medium', 'Large']
```


```python
# label school size with bins
bysize = pd.cut(schools['size'], bins, labels=labels, include_lowest=True)
bysize = pd.DataFrame(bysize)
bysize
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Medium</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Medium</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Small</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Large</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Small</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Medium</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Small</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Large</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Small</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Small</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Small</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Large</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Large</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Medium</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Small</td>
    </tr>
  </tbody>
</table>
</div>




```python
# join bin labels back to large dataframe
withsize = renamed.merge(bysize, left_index=True, right_index=True)
```


```python
withsize
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Type</th>
      <th>Total Students</th>
      <th>School Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
      <th>Total Budget</th>
      <th>size</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>24649428</td>
      <td>Medium</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>24649428</td>
      <td>Medium</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>24649428</td>
      <td>Large</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Wilson High School</td>
      <td>Charter</td>
      <td>2283</td>
      <td>1319574</td>
      <td>83.989488</td>
      <td>83.274201</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
      <td>24649428</td>
      <td>Medium</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cabrera High School</td>
      <td>Charter</td>
      <td>1858</td>
      <td>1081356</td>
      <td>83.975780</td>
      <td>83.061895</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Bailey High School</td>
      <td>District</td>
      <td>4976</td>
      <td>3124928</td>
      <td>81.033963</td>
      <td>77.048432</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
      <td>24649428</td>
      <td>Large</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Holden High School</td>
      <td>Charter</td>
      <td>427</td>
      <td>248087</td>
      <td>83.814988</td>
      <td>83.803279</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Pena High School</td>
      <td>Charter</td>
      <td>962</td>
      <td>585858</td>
      <td>84.044699</td>
      <td>83.839917</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Wright High School</td>
      <td>Charter</td>
      <td>1800</td>
      <td>1049400</td>
      <td>83.955000</td>
      <td>83.682222</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Rodriguez High School</td>
      <td>District</td>
      <td>3999</td>
      <td>2547363</td>
      <td>80.744686</td>
      <td>76.842711</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
      <td>24649428</td>
      <td>Large</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Johnson High School</td>
      <td>District</td>
      <td>4761</td>
      <td>3094650</td>
      <td>80.966394</td>
      <td>77.072464</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
      <td>24649428</td>
      <td>Large</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Ford High School</td>
      <td>District</td>
      <td>2739</td>
      <td>1763916</td>
      <td>80.746258</td>
      <td>77.102592</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
      <td>24649428</td>
      <td>Medium</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Thomas High School</td>
      <td>Charter</td>
      <td>1635</td>
      <td>1043130</td>
      <td>83.848930</td>
      <td>83.418349</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
      <td>24649428</td>
      <td>Small</td>
    </tr>
  </tbody>
</table>
</div>




```python
# isolate pertinent columns
reduced = withsize[['size',
                    'Average Math Score', 
                    'Average Reading Score', 
                    '% Passing Math', 
                    '% Passing Reading', 
                    '% Passing Overall']]
reduced
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>size</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Medium</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Medium</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Small</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Large</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Small</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Medium</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Small</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Large</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Small</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Small</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Small</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Large</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Large</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Medium</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Small</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
  </tbody>
</table>
</div>




```python
# groupby bins to produce summary
grouped = reduced.groupby('size')
```


```python
# calculate averages by bin for each column
output3 = grouped.mean()
```


```python
# format for readability
output4 = output3.style.format({'Average Math Score':'{:.1f}', 
                                 'Average Reading Score':'{:.1f}', 
                                 '% Passing Math':'{:.1f}%', 
                                 '% Passing Reading':'{:.1f}%', 
                                 '% Passing Overall':'{:.1f}%'})
output4
```




<style  type="text/css" >
</style>  
<table id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Passing Overall</th> 
    </tr>    <tr> 
        <th class="index_name level0" >size</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83level0_row0" class="row_heading level0 row0" >Small</th> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row0_col0" class="data row0 col0" >83.5</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row0_col1" class="data row0 col1" >83.9</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row0_col2" class="data row0 col2" >93.6%</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row0_col3" class="data row0 col3" >96.6%</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row0_col4" class="data row0 col4" >95.1%</td> 
    </tr>    <tr> 
        <th id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83level0_row1" class="row_heading level0 row1" >Medium</th> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row1_col0" class="data row1 col0" >78.4</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row1_col1" class="data row1 col1" >81.8</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row1_col2" class="data row1 col2" >73.5%</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row1_col3" class="data row1 col3" >84.5%</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row1_col4" class="data row1 col4" >79.0%</td> 
    </tr>    <tr> 
        <th id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83level0_row2" class="row_heading level0 row2" >Large</th> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row2_col0" class="data row2 col0" >77.1</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row2_col1" class="data row2 col1" >80.9</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row2_col2" class="data row2 col2" >66.5%</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row2_col3" class="data row2 col3" >81.1%</td> 
        <td id="T_5a9ce438_dc94_11e7_8e17_a0999b1e5b83row2_col4" class="data row2 col4" >73.8%</td> 
    </tr></tbody> 
</table> 



#### School performance by type


```python
renamed.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>School</th>
      <th>Type</th>
      <th>Total Students</th>
      <th>School Budget</th>
      <th>Average Reading Score</th>
      <th>Average Math Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
      <th>Total Budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
      <td>81.182722</td>
      <td>76.629414</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
      <td>81.158020</td>
      <td>76.711767</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
      <td>83.725724</td>
      <td>83.359455</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
      <td>80.934412</td>
      <td>77.289752</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
      <td>24649428</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
      <td>83.816757</td>
      <td>83.351499</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
      <td>24649428</td>
    </tr>
  </tbody>
</table>
</div>




```python
# isolate pertinent columns
summarysubset = renamed[['Type',
                         'Average Math Score', 
                         'Average Reading Score', 
                         '% Passing Math', 
                         '% Passing Reading', 
                         '% Passing Overall']]
summarysubset
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Passing Overall</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>District</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>1</th>
      <td>District</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Charter</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>District</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Charter</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Charter</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Charter</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>7</th>
      <td>District</td>
      <td>77.048432</td>
      <td>81.033963</td>
      <td>66.680064</td>
      <td>81.933280</td>
      <td>74.306672</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Charter</td>
      <td>83.803279</td>
      <td>83.814988</td>
      <td>92.505855</td>
      <td>96.252927</td>
      <td>94.379391</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Charter</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Charter</td>
      <td>83.682222</td>
      <td>83.955000</td>
      <td>93.333333</td>
      <td>96.611111</td>
      <td>94.972222</td>
    </tr>
    <tr>
      <th>11</th>
      <td>District</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>12</th>
      <td>District</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>13</th>
      <td>District</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Charter</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
  </tbody>
</table>
</div>




```python
# groupby school type to calculate averages for each type
typegroup = summarysubset.groupby('Type')
output5 = typegroup.mean()
```


```python
# format for readability
output6 = output5.style.format({'Average Math Score':'{:.1f}', 
                                'Average Reading Score':'{:.1f}', 
                                '% Passing Math':'{:.1f}%', 
                                '% Passing Reading':'{:.1f}%', 
                                '% Passing Overall':'{:.1f}%'})
output6
```




<style  type="text/css" >
</style>  
<table id="T_767df822_dc94_11e7_914d_a0999b1e5b83" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Average Math Score</th> 
        <th class="col_heading level0 col1" >Average Reading Score</th> 
        <th class="col_heading level0 col2" >% Passing Math</th> 
        <th class="col_heading level0 col3" >% Passing Reading</th> 
        <th class="col_heading level0 col4" >% Passing Overall</th> 
    </tr>    <tr> 
        <th class="index_name level0" >Type</th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
        <th class="blank" ></th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_767df822_dc94_11e7_914d_a0999b1e5b83level0_row0" class="row_heading level0 row0" >Charter</th> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row0_col0" class="data row0 col0" >83.5</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row0_col1" class="data row0 col1" >83.9</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row0_col2" class="data row0 col2" >93.6%</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row0_col3" class="data row0 col3" >96.6%</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row0_col4" class="data row0 col4" >95.1%</td> 
    </tr>    <tr> 
        <th id="T_767df822_dc94_11e7_914d_a0999b1e5b83level0_row1" class="row_heading level0 row1" >District</th> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row1_col0" class="data row1 col0" >77.0</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row1_col1" class="data row1 col1" >81.0</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row1_col2" class="data row1 col2" >66.5%</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row1_col3" class="data row1 col3" >80.8%</td> 
        <td id="T_767df822_dc94_11e7_914d_a0999b1e5b83row1_col4" class="data row1 col4" >73.7%</td> 
    </tr></tbody> 
</table> 


