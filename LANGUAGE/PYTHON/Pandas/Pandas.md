**Table of Contents**

- [Opening](#opening)
- [Data loading manually and from CSV files to Pandas DataFrame](#data-loading-manually-and-from-csv-files-to-pandas-dataframe)
- [Loading, editing, and viewing data from Pandas DataFrame](#loading-editing-and-viewing-data-from-pandas-dataframe)
- [Renaming colmnns, exporting and saving Pandas DataFrames](#renaming-colmnns-exporting-and-saving-pandas-dataframes)
- [Summarising, grouping, and aggregating data in Pandas](#summarising-grouping-and-aggregating-data-in-pandas)
- [Merge and join DataFrames with Pandas](#merge-and-join-dataframes-with-pandas)
- [Basic Plotting Pandas DataFrames](#basic-plotting-pandas-dataframes)

# Opening

CSV(comma-separated value) files are a common file format of data. 
The ability to read, manipulate, and write date to and from CSV files using Python is a key skill to master for any data scientist or business analysis.

1) what CSV files are,  
2) how to read CSV files into "Pandas DataFrames",  
3) how to write DataFrames back to CSV files.  
What is "Pandas DataFrame"?
: Pandas is the most popular data manipulation package in Python, and DataFrames are the Pandas data type for storing tabular 2D data.
: Pandas development started in 2008 with main developer Wes McKinney and the library has become a standard for data analysis and management using Python.
: Pandas fluency is essential for any Python-based data professional, people interested in trying a Kaggle challenge, or anyone seeking to automate a data process. 
: The Pandas library documentation defines a DataFrame as a “two-dimensional, size-mutable, potentially heterogeneous tabular data structure with labeled axes (rows and columns)”.

- There can be multiple rows and columns in the data
- Each row represents a sample of data, 
- Each column contains a different variable that describes the samples (rows). 
- The data in every column is usually the same type of data – e.g. numbers, strings, dates
- Usually, unlike an excel data set, DataFrames avoid having missing values, and there are no gaps and empty values between rows or columns.

```python
# Manually generate data
import pandas as pd 
pd.options.display.max_columns = 20
pd.options.display.max_rows = 10
data = {'column1':[1,2,3,4,5],
        'anatoeh_column':['this', 'column', 'has', 'strings', 'indise!'],
        'float_column':[0.1, 0.5, 33, 48, 42.5555],
        'binary_column':[True, False, True, True, False]}
print(data)
print(data['column1'])
display(pd.DataFrame(data))
display(pd.DataFrame(data['column1']))
```

```json
    {'column1': [1, 2, 3, 4, 5], 'anatoeh_column': ['this', 'column', 'has', 'strings', 'indise!'], 'float_column': [0.1, 0.5, 33, 48, 42.5555], 'binary_column': [True, False, True, True, False]}
    [1, 2, 3, 4, 5]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>column1</th>
      <th>anatoeh_column</th>
      <th>float_column</th>
      <th>binary_column</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
      <td>this</td>
      <td>0.1000</td>
      <td>True</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
      <td>column</td>
      <td>0.5000</td>
      <td>False</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
      <td>has</td>
      <td>33.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
      <td>strings</td>
      <td>48.0000</td>
      <td>True</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
      <td>indise!</td>
      <td>42.5555</td>
      <td>False</td>
    </tr>
  </tbody>
</table>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>3</td>
      <td>4</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

# Data loading manually and from CSV files to Pandas DataFrame

(https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html)  
There are 3 fundamantal conceps to grasp and debug the operation of the data loading procedure.

1) Understanding file extensions and file types – what do the letters CSV actually mean? what’s the difference between a .csv file and a .txt file?  
2) Understanding how data is represented inside CSV files – if you open a CSV file, what does the data actually look like?  
3) Understanding the Python path and how to reference a file – what is the absolute and relative path to the file you are loading? What directory are you working in?  
4) CSV file loading errors
- FileNotFoundError: File b'filename.csv' does not exist  
=> A File Not Found error is typically an issue with path setup, current directory, or file name confusion (file extension can play a part here!)
- UnicodeDecodeError: 'utf-8' codec can't decode byte in position : invalid continuation byte  
=> A Unicode Decode Error is typically caused by not specifying the encoding of the file, and happens when you have a file with non-standard characters. For a quick fix, try opening the file in Sublime Text, and re-saving with encoding ‘UTF-8’.
- pandas.parser.CParserError: Error tokenizing data.  
=> Parse Errors can be caused in unusual circumstances to do with your data format – try to add the parameter “engine=’python'” to the read_csv function call; this changes the data reading function internally to a slower but more stable method.

```python
# Finding your Python path
# The "OS module" is for operating system dependent functionality into Python
import os
print(os.getcwd())
print(os.listdir())
# os.chdir("path")
```

```bash
    D:\Research\Analysis\Lecture\TimeSeriesAnalysis\Shared
    ['.ipynb_checkpoints', 'Data', 'Image', 'Lecture1_DataAnalysisCycle_DataStatistics_KK.ipynb', 'Lecture2_Learning_TimeSeries_KK.ipynb', 'Lecture3_Algorithms_ML_TS_Linear_KK.ipynb', 'Lecture4_Algorithms_TS_NonLinear_Multivariate_KK.ipynb', 'module.py', 'Practice0_Installation_Program_KK.ipynb', 'Practice1_Tutorial_Pandas_KK.ipynb', 'Practice2_Tutorial_Numpy.ipynb', 'Practice3_Setting_Analysis_KK.ipynb', 'Practice4_FE_Analysis_KK.ipynb', 'Practice5_Agile_Analysis_KK.ipynb', 'Practice6_TimeSeries_Analysis_KK.ipynb', 'week_contents.txt', '[FastCampus] 1주차_강의자료_김경원박사.pptx', '[FastCampus] 2주차_강의자료_김경원박사.pptx', '[FastCampus] 3주차_강의자료_김경원박사.pptx', '[FastCampus] 4주차_강의자료_김경원박사.pptx', '[FastCampus] 5주차_강의자료_김경원박사.pptx', '[FastCampus] 6주차_강의자료_김경원박사.pptx', '[FastCampus] 7주차_강의자료_김경원박사.pptx', '[FastCampus] 8주차_강의자료_김경원박사.pptx', '__pycache__']
```

```python
# File Loading from "Absolute" and "Relative" paths
# Relative paths are directions to the file starting at your current working directory, where absolute paths always start at the base of your file system.
# direct_path : 'https://s3-eu-west-1.amazonaws.com/shanebucket/downloads/FAO+database.csv' from 'https://www.kaggle.com/dorbicycle/world-foodfeed-production'
absolute_path = './Data/FoodAgricultureOrganization/Food_Agriculture_Organization_UN_Full.csv'
pd.read_csv(absolute_path, sep=',')
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>

```python
relative_path = 'D:/Research/Shared/TimeSeriesAnalysis/Data/FoodAgricultureOrganization/Food_Agriculture_Organization_UN_Full.csv'
pd.read_csv(relative_path, sep=',')
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>

```python
pd.options.display.max_columns = 20
relative_path = './Data/FoodAgricultureOrganization/Food_Agriculture_Organization_UN_Full.csv'
raw_data = pd.read_csv(relative_path, sep=',')
raw_data
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>
# Loading, editing, and viewing data from Pandas DataFrame
Pandas displays only 20 columns by default for wide data dataframes, and only 60 or so rows, truncating the middle section. 
If you’d like to change these limits, you can edit the defaults using some internal options for Pandas displays
(simple use pd.display.options.XX = value to set these) 
(https://pandas.pydata.org/pandas-docs/stable/options.html)
- pd.options.display.width – the width of the display in characters – use this if your display is wrapping rows over more than one line.
- pd.options.display.max_rows – maximum number of rows displayed.
- pd.options.display.max_columns – maximum number of columns displayed. 
Finally, to see some of the core statistics about a particular column, you can use the ‘describe‘ function.
- For numeric columns, describe() returns basic statistics: the value count, mean, standard deviation, minimum, maximum, and 25th, 50th, and 75th quantiles for the data in a column. 
- For string columns, describe() returns the value count, the number of unique entries, the most frequently occurring value (‘top’), and the number of times the top value occurs (‘freq’)
There’s two main options to achieve the selection and indexing activities in Pandas. 
When using .loc, or .iloc, you can control the output format by passing lists or single values to the selectors. 
(http://pandas.pydata.org/pandas-docs/stable/indexing.html#selection-by-label)
1. iloc
    - Note that .iloc returns a Pandas Series when one row is selected, and a Pandas DataFrame when multiple rows are selected, or if any column in full is selected. To counter this, pass a single-valued list if you require DataFrame output.
    - When selecting multiple columns or multiple rows in this manner, remember that in your selection e.g.[1:5], the rows/columns selected will run from the first number to one minus the second number. e.g. [1:5] will go 1,2,3,4., [x,y] goes from x to y-1.
2. loc
    - Label-based / Index-based indexing
    - Boolean / Logical indexing
        - You pass an array or Series of True/False values to the .loc indexer to select the rows where your Series has True values.
Selecting rows and columns
- using a dot notation, e.g. data.column_name,
- using square braces and the name of the column as a string, e.g. data['column_name']
- using numeric indexing and the iloc selector data.iloc[:, <column_number>] 
When a column is selected using any of these methodologies, a pandas.Series is the resulting datatype. A pandas series is a one-dimensional set of data.
- square-brace selection with a list of column names, e.g. data[['column_name_1', 'column_name_2']]
- using numeric indexing with the iloc selector and a list of column numbers, e.g. data.iloc[:, [0,1,20,22]]
Rows in a DataFrame are selected, typically, using the iloc/loc selection methods, or using logical selectors
- numeric row selection using the iloc selector, e.g. data.iloc[0:10, :] – select the first 10 rows.
- label-based row selection using the loc selector (this is only applicably if you have set an “index” on your dataframe. e.g. data.loc[44, :]
- logical-based row selection using evaluated statements, e.g. data[data["Area"] == "Ireland"] – select the rows where Area value is ‘Ireland’.
![](https://shanelynnwebsite-mid9n9g1q9y8tt.netdna-ssl.com/wp-content/uploads/2016/10/Pandas-selections-and-indexing-768x549.png)
To delete rows and columns from DataFrames, Pandas uses the “drop” function.
- To delete a column, or multiple columns, use the name of the column(s), and specify the “axis” as 1.
- Alternatively, as in the example below, the ‘columns’ parameter has been added in Pandas which cuts out the need for ‘axis’.
- The drop function returns a new DataFrame, with the columns removed. To actually edit the original DataFrame, the “inplace” parameter can be set to True, and there is no returned value.
- Rows can also be removed using the “drop” function, by specifying axis=0. Drop() removes rows based on “labels”, rather than numeric indexing. To delete rows based on their numeric position / index, use iloc to reassign the dataframe values

```python
# Examine data in a Pandas DataFrame
raw_data.shape
    (21477, 63)
```

```python
raw_data.ndim
    2
```

```python
raw_data.head(5)
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 63 columns</p>

```python
raw_data.tail(5)
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 63 columns</p>

```python
raw_data.dtypes
```

```bash
    Area Abbreviation     object
    Area Code              int64
    Area                  object
    Item Code              int64
    Item                  object
    Y2009                float64
    Y2010                float64
    Y2011                float64
    Y2012                  int64
    Y2013                  int64
    Length: 63, dtype: object
```

```python
raw_data['Item Code'] = raw_data['Item Code'].astype(str)
raw_data.dtypes
```

```bash
    Area Abbreviation     object
    Area Code              int64
    Area                  object
    Item Code             object
    Item                  object
    Y2009                float64
    Y2010                float64
    Y2011                float64
    Y2012                  int64
    Y2013                  int64
    Length: 63, dtype: object
```

```python
raw_data['Y2013'].describe()
```

```bash
    count     21477.000000
    mean        575.557480
    std        6218.379479
    min        -246.000000
    25%           0.000000
    50%           8.000000
    75%          90.000000
    max      489299.000000
    Name: Y2013, dtype: float64
```

```python
raw_data['Area'].describe()
```

```bash
    count     21477
    unique      174
    top       Spain
    freq        150
    Name: Area, dtype: object
```

```python
raw_data.describe()
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Code</th>
      <th>Element Code</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>Y1962</th>
      <th>Y1963</th>
      <th>Y1964</th>
      <th>Y1965</th>
      <th>Y1966</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>17938.000000</td>
      <td>...</td>
      <td>21128.000000</td>
      <td>21128.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21373.000000</td>
      <td>21477.000000</td>
      <td>21477.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>125.449411</td>
      <td>5211.687154</td>
      <td>20.450613</td>
      <td>15.794445</td>
      <td>195.262069</td>
      <td>200.782250</td>
      <td>205.464600</td>
      <td>209.925577</td>
      <td>217.556751</td>
      <td>225.988962</td>
      <td>...</td>
      <td>486.690742</td>
      <td>493.153256</td>
      <td>496.319328</td>
      <td>508.482104</td>
      <td>522.844898</td>
      <td>524.581996</td>
      <td>535.492069</td>
      <td>553.399242</td>
      <td>560.569214</td>
      <td>575.557480</td>
    </tr>
    <tr>
      <td>std</td>
      <td>72.868149</td>
      <td>146.820079</td>
      <td>24.628336</td>
      <td>66.012104</td>
      <td>1864.124336</td>
      <td>1884.265591</td>
      <td>1861.174739</td>
      <td>1862.000116</td>
      <td>2014.934333</td>
      <td>2100.228354</td>
      <td>...</td>
      <td>5001.782008</td>
      <td>5100.057036</td>
      <td>5134.819373</td>
      <td>5298.939807</td>
      <td>5496.697513</td>
      <td>5545.939303</td>
      <td>5721.089425</td>
      <td>5883.071604</td>
      <td>6047.950804</td>
      <td>6218.379479</td>
    </tr>
    <tr>
      <td>min</td>
      <td>1.000000</td>
      <td>5142.000000</td>
      <td>-40.900000</td>
      <td>-172.100000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>-169.000000</td>
      <td>-246.000000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>63.000000</td>
      <td>5142.000000</td>
      <td>6.430000</td>
      <td>-11.780000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>120.000000</td>
      <td>5142.000000</td>
      <td>20.590000</td>
      <td>19.150000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>...</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>8.000000</td>
      <td>8.000000</td>
      <td>8.000000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>188.000000</td>
      <td>5142.000000</td>
      <td>41.150000</td>
      <td>46.870000</td>
      <td>21.000000</td>
      <td>22.000000</td>
      <td>23.000000</td>
      <td>24.000000</td>
      <td>25.000000</td>
      <td>26.000000</td>
      <td>...</td>
      <td>75.000000</td>
      <td>77.000000</td>
      <td>78.000000</td>
      <td>80.000000</td>
      <td>82.000000</td>
      <td>83.000000</td>
      <td>83.000000</td>
      <td>86.000000</td>
      <td>88.000000</td>
      <td>90.000000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>276.000000</td>
      <td>5521.000000</td>
      <td>64.960000</td>
      <td>179.410000</td>
      <td>112227.000000</td>
      <td>109130.000000</td>
      <td>106356.000000</td>
      <td>104234.000000</td>
      <td>119378.000000</td>
      <td>118495.000000</td>
      <td>...</td>
      <td>360767.000000</td>
      <td>373694.000000</td>
      <td>388100.000000</td>
      <td>402975.000000</td>
      <td>425537.000000</td>
      <td>434724.000000</td>
      <td>451838.000000</td>
      <td>462696.000000</td>
      <td>479028.000000</td>
      <td>489299.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 57 columns</p>

```python
# Selecting and manipulating data
raw_data.iloc[0]
```

```bash
    Area Abbreviation                    AF
    Area Code                             2
    Area                        Afghanistan
    Item Code                          2511
    Item                 Wheat and products
    Y2009                              4538
    Y2010                              4605
    Y2011                              4711
    Y2012                              4810
    Y2013                              4895
    Name: 0, Length: 63, dtype: object
```

```python
raw_data.iloc[[1]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 63 columns</p>

```python
raw_data.iloc[[-1]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 63 columns</p>

```python
raw_data.iloc[:,0]
```

```bash
    0        AF
    1        AF
    2        AF
    3        AF
    4        AF
    ..
    21472    ZW
    21473    ZW
    21474    ZW
    21475    ZW
    21476    ZW
    Name: Area Abbreviation, Length: 21477, dtype: object
```

```python
raw_data.iloc[:,[1]]
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>181</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 1 columns</p>

```python
raw_data.iloc[:,[-1]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 1 columns</p>

```python
raw_data.iloc[0:5]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 63 columns</p>

```python
raw_data.iloc[:,0:2]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 2 columns</p>

```python
raw_data.iloc[[0,3,6,24],[0,5,6]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Element Code</th>
      <th>Element</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>5142</td>
      <td>Food</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>5142</td>
      <td>Food</td>
    </tr>
    <tr>
      <td>6</td>
      <td>AF</td>
      <td>5142</td>
      <td>Food</td>
    </tr>
    <tr>
      <td>24</td>
      <td>AF</td>
      <td>5142</td>
      <td>Food</td>
    </tr>
  </tbody>
</table>

```python
raw_data.iloc[0:5, 5:8]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
    </tr>
    <tr>
      <td>1</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
    </tr>
    <tr>
      <td>2</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
    </tr>
    <tr>
      <td>3</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
    </tr>
    <tr>
      <td>4</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
    </tr>
  </tbody>
</table>

```python
raw_data.loc[0]
```

```bash
    Area Abbreviation                    AF
    Area Code                             2
    Area                        Afghanistan
    Item Code                          2511
    Item                 Wheat and products
                                ...        
    Y2009                              4538
    Y2010                              4605
    Y2011                              4711
    Y2012                              4810
    Y2013                              4895
    Name: 0, Length: 63, dtype: object
```

```python
raw_data.loc[[1]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 63 columns</p>

```python
raw_data.loc[[1,3]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 63 columns</p>

```python
raw_data.loc[[1,3],['Item','Y2013']]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Rice (Milled Equivalent)</td>
      <td>422</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Barley and products</td>
      <td>89</td>
    </tr>
  </tbody>
</table>

```python
raw_data.loc[[1,3],'Item':'Y2013']
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>Y1962</th>
      <th>Y1963</th>
      <th>Y1964</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>183.0</td>
      <td>183.0</td>
      <td>182.0</td>
      <td>220.0</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>237.0</td>
      <td>237.0</td>
      <td>237.0</td>
      <td>238.0</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 59 columns</p>

```python
raw_data.loc[1:3,'Item':'Y2013']
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>Y1962</th>
      <th>Y1963</th>
      <th>Y1964</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>183.0</td>
      <td>183.0</td>
      <td>182.0</td>
      <td>220.0</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>76.0</td>
      <td>76.0</td>
      <td>76.0</td>
      <td>76.0</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>237.0</td>
      <td>237.0</td>
      <td>237.0</td>
      <td>238.0</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
  </tbody>
</table>
<p>3 rows × 59 columns</p>

```python
raw_data_test = raw_data.loc[10:,'Item':'Y2013']
raw_data_test.iloc[[0]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>Y1962</th>
      <th>Y1963</th>
      <th>Y1964</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 59 columns</p>

```python
raw_data_test.loc[[10]]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>Y1962</th>
      <th>Y1963</th>
      <th>Y1964</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>1 rows × 59 columns</p>

```python
raw_data.loc[raw_data['Item'] == 'Sugar beet']
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>103</td>
      <td>AL</td>
      <td>3</td>
      <td>Albania</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td>699</td>
      <td>AM</td>
      <td>1</td>
      <td>Armenia</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>40.07</td>
      <td>45.04</td>
      <td>...</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>832</td>
      <td>AU</td>
      <td>10</td>
      <td>Australia</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-25.27</td>
      <td>133.78</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1099</td>
      <td>AZ</td>
      <td>52</td>
      <td>Azerbaijan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>40.14</td>
      <td>47.58</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20020</td>
      <td>AE</td>
      <td>225</td>
      <td>United Arab Emirates</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>23.42</td>
      <td>53.85</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>20676</td>
      <td>UZ</td>
      <td>235</td>
      <td>Uzbekistan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.38</td>
      <td>64.59</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>20900</td>
      <td>VE</td>
      <td>236</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>6.42</td>
      <td>-66.59</td>
      <td>...</td>
      <td>17.0</td>
      <td>20.0</td>
      <td>23.0</td>
      <td>21.0</td>
      <td>21.0</td>
      <td>21.0</td>
      <td>30.0</td>
      <td>35.0</td>
      <td>20</td>
      <td>22</td>
    </tr>
    <tr>
      <td>21135</td>
      <td>YE</td>
      <td>249</td>
      <td>Yemen</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>15.55</td>
      <td>48.52</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21374</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>66 rows × 63 columns</p>

```python
# is same as
raw_data[raw_data['Item'] == 'Sugar beet']
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>103</td>
      <td>AL</td>
      <td>3</td>
      <td>Albania</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td>699</td>
      <td>AM</td>
      <td>1</td>
      <td>Armenia</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>40.07</td>
      <td>45.04</td>
      <td>...</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <td>832</td>
      <td>AU</td>
      <td>10</td>
      <td>Australia</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-25.27</td>
      <td>133.78</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1099</td>
      <td>AZ</td>
      <td>52</td>
      <td>Azerbaijan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>40.14</td>
      <td>47.58</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20020</td>
      <td>AE</td>
      <td>225</td>
      <td>United Arab Emirates</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>23.42</td>
      <td>53.85</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>20676</td>
      <td>UZ</td>
      <td>235</td>
      <td>Uzbekistan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.38</td>
      <td>64.59</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>20900</td>
      <td>VE</td>
      <td>236</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>6.42</td>
      <td>-66.59</td>
      <td>...</td>
      <td>17.0</td>
      <td>20.0</td>
      <td>23.0</td>
      <td>21.0</td>
      <td>21.0</td>
      <td>21.0</td>
      <td>30.0</td>
      <td>35.0</td>
      <td>20</td>
      <td>22</td>
    </tr>
    <tr>
      <td>21135</td>
      <td>YE</td>
      <td>249</td>
      <td>Yemen</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>15.55</td>
      <td>48.52</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>13.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21374</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>66 rows × 63 columns</p>

```python
raw_data.loc[raw_data['Item'] == 'Sugar beet', 'Area']
```

```bash
    10                              Afghanistan
    103                                 Albania
    699                                 Armenia
    832                               Australia
    1099                             Azerbaijan
        ...
    20020                  United Arab Emirates
    20676                            Uzbekistan
    20900    Venezuela (Bolivarian Republic of)
    21135                                 Yemen
    21374                              Zimbabwe
    Name: Area, Length: 66, dtype: object
```

```python
raw_data.loc[raw_data['Item'] == 'Sugar beet', ['Area']]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>Afghanistan</td>
    </tr>
    <tr>
      <td>103</td>
      <td>Albania</td>
    </tr>
    <tr>
      <td>699</td>
      <td>Armenia</td>
    </tr>
    <tr>
      <td>832</td>
      <td>Australia</td>
    </tr>
    <tr>
      <td>1099</td>
      <td>Azerbaijan</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20020</td>
      <td>United Arab Emirates</td>
    </tr>
    <tr>
      <td>20676</td>
      <td>Uzbekistan</td>
    </tr>
    <tr>
      <td>20900</td>
      <td>Venezuela (Bolivarian Republic of)</td>
    </tr>
    <tr>
      <td>21135</td>
      <td>Yemen</td>
    </tr>
    <tr>
      <td>21374</td>
      <td>Zimbabwe</td>
    </tr>
  </tbody>
</table>
<p>66 rows × 1 columns</p>

```python
# is not same as
raw_data[raw_data['Item'] == 'Sugar beet', ['Area']]
```

```bash
    ---------------------------------------------------------------------------
    TypeError                                 Traceback (most recent call last)
    <ipython-input-38-d794b59d3bc7> in <module>
          1 # is not same as
    ----> 2 raw_data[raw_data['Item'] == 'Sugar beet', ['Area']]
    
    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\frame.py in __getitem__(self, key)
       2978             if self.columns.nlevels > 1:
       2979                 return self._getitem_multilevel(key)
    -> 2980             indexer = self.columns.get_loc(key)
       2981             if is_integer(indexer):
       2982                 indexer = [indexer]
    
    C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\indexes\base.py in get_loc(self, key, method, tolerance)
       2895                 )
       2896             try:
    -> 2897                 return self._engine.get_loc(key)
       2898             except KeyError:
       2899                 return self._engine.get_loc(self._maybe_cast_indexer(key))
    
    pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()
    
    pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()
    
    TypeError: '(0        False
    1        False
    2        False
    3        False
    4        False
             ...  
    21472    False
    21473    False
    21474    False
    21475    False
    21476    False
    Name: Item, Length: 21477, dtype: bool, ['Area'])' is an invalid key
```

```python
raw_data.loc[raw_data['Item'] == 'Sugar beet', ['Area', 'Item', 'latitude']]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Item</th>
      <th>latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>Afghanistan</td>
      <td>Sugar beet</td>
      <td>33.94</td>
    </tr>
    <tr>
      <td>103</td>
      <td>Albania</td>
      <td>Sugar beet</td>
      <td>41.15</td>
    </tr>
    <tr>
      <td>699</td>
      <td>Armenia</td>
      <td>Sugar beet</td>
      <td>40.07</td>
    </tr>
    <tr>
      <td>832</td>
      <td>Australia</td>
      <td>Sugar beet</td>
      <td>-25.27</td>
    </tr>
    <tr>
      <td>1099</td>
      <td>Azerbaijan</td>
      <td>Sugar beet</td>
      <td>40.14</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20020</td>
      <td>United Arab Emirates</td>
      <td>Sugar beet</td>
      <td>23.42</td>
    </tr>
    <tr>
      <td>20676</td>
      <td>Uzbekistan</td>
      <td>Sugar beet</td>
      <td>41.38</td>
    </tr>
    <tr>
      <td>20900</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>Sugar beet</td>
      <td>6.42</td>
    </tr>
    <tr>
      <td>21135</td>
      <td>Yemen</td>
      <td>Sugar beet</td>
      <td>15.55</td>
    </tr>
    <tr>
      <td>21374</td>
      <td>Zimbabwe</td>
      <td>Sugar beet</td>
      <td>-19.02</td>
    </tr>
  </tbody>
</table>
<p>66 rows × 3 columns</p>

```python
raw_data.loc[raw_data['Item'] == 'Sugar beet', 'Area':'latitude']
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>10</td>
      <td>Afghanistan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
    </tr>
    <tr>
      <td>103</td>
      <td>Albania</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
    </tr>
    <tr>
      <td>699</td>
      <td>Armenia</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>40.07</td>
    </tr>
    <tr>
      <td>832</td>
      <td>Australia</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-25.27</td>
    </tr>
    <tr>
      <td>1099</td>
      <td>Azerbaijan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>40.14</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20020</td>
      <td>United Arab Emirates</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>23.42</td>
    </tr>
    <tr>
      <td>20676</td>
      <td>Uzbekistan</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.38</td>
    </tr>
    <tr>
      <td>20900</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>6.42</td>
    </tr>
    <tr>
      <td>21135</td>
      <td>Yemen</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>15.55</td>
    </tr>
    <tr>
      <td>21374</td>
      <td>Zimbabwe</td>
      <td>2537</td>
      <td>Sugar beet</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
    </tr>
  </tbody>
</table>
<p>66 rows × 7 columns</p>

```python
raw_data.loc[raw_data['Area'].str.endswith('many')]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>7532</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>8006.0</td>
      <td>11084.0</td>
      <td>10644.0</td>
      <td>8993.0</td>
      <td>10669.0</td>
      <td>10608.0</td>
      <td>8189.0</td>
      <td>9242.0</td>
      <td>7868</td>
      <td>7494</td>
    </tr>
    <tr>
      <td>7533</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>6524.0</td>
      <td>6931.0</td>
      <td>6796.0</td>
      <td>6886.0</td>
      <td>6868.0</td>
      <td>7137.0</td>
      <td>7235.0</td>
      <td>7204.0</td>
      <td>6712</td>
      <td>6900</td>
    </tr>
    <tr>
      <td>7534</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>12.0</td>
      <td>17.0</td>
      <td>8.0</td>
      <td>18.0</td>
      <td>13.0</td>
      <td>12.0</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <td>7535</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>206.0</td>
      <td>212.0</td>
      <td>226.0</td>
      <td>244.0</td>
      <td>251.0</td>
      <td>257.0</td>
      <td>243.0</td>
      <td>271.0</td>
      <td>275</td>
      <td>277</td>
    </tr>
    <tr>
      <td>7536</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>7571.0</td>
      <td>6878.0</td>
      <td>7845.0</td>
      <td>6940.0</td>
      <td>7255.0</td>
      <td>7592.0</td>
      <td>7543.0</td>
      <td>6062.0</td>
      <td>6316</td>
      <td>6598</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>7674</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>20252.0</td>
      <td>20878.0</td>
      <td>21323.0</td>
      <td>21661.0</td>
      <td>21091.0</td>
      <td>22077.0</td>
      <td>21431.0</td>
      <td>21171.0</td>
      <td>21169</td>
      <td>21401</td>
    </tr>
    <tr>
      <td>7675</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>32.0</td>
      <td>51.0</td>
      <td>28.0</td>
      <td>25.0</td>
      <td>29.0</td>
      <td>25.0</td>
      <td>6</td>
      <td>5</td>
    </tr>
    <tr>
      <td>7676</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>1122.0</td>
      <td>1177.0</td>
      <td>1238.0</td>
      <td>1287.0</td>
      <td>1226.0</td>
      <td>1187.0</td>
      <td>1175.0</td>
      <td>1188.0</td>
      <td>1128</td>
      <td>1039</td>
    </tr>
    <tr>
      <td>7677</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <td>7678</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>147 rows × 63 columns</p>

```python
# is same as
raw_data.loc[raw_data['Area'].isin(['Germany'])]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>7532</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>8006.0</td>
      <td>11084.0</td>
      <td>10644.0</td>
      <td>8993.0</td>
      <td>10669.0</td>
      <td>10608.0</td>
      <td>8189.0</td>
      <td>9242.0</td>
      <td>7868</td>
      <td>7494</td>
    </tr>
    <tr>
      <td>7533</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>6524.0</td>
      <td>6931.0</td>
      <td>6796.0</td>
      <td>6886.0</td>
      <td>6868.0</td>
      <td>7137.0</td>
      <td>7235.0</td>
      <td>7204.0</td>
      <td>6712</td>
      <td>6900</td>
    </tr>
    <tr>
      <td>7534</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>12.0</td>
      <td>17.0</td>
      <td>8.0</td>
      <td>18.0</td>
      <td>13.0</td>
      <td>12.0</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <td>7535</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>206.0</td>
      <td>212.0</td>
      <td>226.0</td>
      <td>244.0</td>
      <td>251.0</td>
      <td>257.0</td>
      <td>243.0</td>
      <td>271.0</td>
      <td>275</td>
      <td>277</td>
    </tr>
    <tr>
      <td>7536</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>7571.0</td>
      <td>6878.0</td>
      <td>7845.0</td>
      <td>6940.0</td>
      <td>7255.0</td>
      <td>7592.0</td>
      <td>7543.0</td>
      <td>6062.0</td>
      <td>6316</td>
      <td>6598</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>7674</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>20252.0</td>
      <td>20878.0</td>
      <td>21323.0</td>
      <td>21661.0</td>
      <td>21091.0</td>
      <td>22077.0</td>
      <td>21431.0</td>
      <td>21171.0</td>
      <td>21169</td>
      <td>21401</td>
    </tr>
    <tr>
      <td>7675</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>32.0</td>
      <td>51.0</td>
      <td>28.0</td>
      <td>25.0</td>
      <td>29.0</td>
      <td>25.0</td>
      <td>6</td>
      <td>5</td>
    </tr>
    <tr>
      <td>7676</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>1122.0</td>
      <td>1177.0</td>
      <td>1238.0</td>
      <td>1287.0</td>
      <td>1226.0</td>
      <td>1187.0</td>
      <td>1175.0</td>
      <td>1188.0</td>
      <td>1128</td>
      <td>1039</td>
    </tr>
    <tr>
      <td>7677</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <td>7678</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>147 rows × 63 columns</p>

```python
raw_data.loc[raw_data['Area'].isin(['Germany', 'France'])]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>6910</td>
      <td>FR</td>
      <td>68</td>
      <td>France</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>46.23</td>
      <td>2.21</td>
      <td>...</td>
      <td>9867.0</td>
      <td>11311.0</td>
      <td>10091.0</td>
      <td>8483.0</td>
      <td>10283.0</td>
      <td>8351.0</td>
      <td>6262.0</td>
      <td>7727.0</td>
      <td>7179</td>
      <td>7822</td>
    </tr>
    <tr>
      <td>6911</td>
      <td>FR</td>
      <td>68</td>
      <td>France</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>46.23</td>
      <td>2.21</td>
      <td>...</td>
      <td>6057.0</td>
      <td>6140.0</td>
      <td>6402.0</td>
      <td>6102.0</td>
      <td>6677.0</td>
      <td>6331.0</td>
      <td>6986.0</td>
      <td>6765.0</td>
      <td>6984</td>
      <td>6971</td>
    </tr>
    <tr>
      <td>6912</td>
      <td>FR</td>
      <td>68</td>
      <td>France</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>46.23</td>
      <td>2.21</td>
      <td>...</td>
      <td>80.0</td>
      <td>75.0</td>
      <td>84.0</td>
      <td>88.0</td>
      <td>92.0</td>
      <td>82.0</td>
      <td>81.0</td>
      <td>91.0</td>
      <td>99</td>
      <td>101</td>
    </tr>
    <tr>
      <td>6913</td>
      <td>FR</td>
      <td>68</td>
      <td>France</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>46.23</td>
      <td>2.21</td>
      <td>...</td>
      <td>296.0</td>
      <td>325.0</td>
      <td>317.0</td>
      <td>367.0</td>
      <td>350.0</td>
      <td>347.0</td>
      <td>339.0</td>
      <td>361.0</td>
      <td>339</td>
      <td>314</td>
    </tr>
    <tr>
      <td>6914</td>
      <td>FR</td>
      <td>68</td>
      <td>France</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>46.23</td>
      <td>2.21</td>
      <td>...</td>
      <td>3537.0</td>
      <td>3560.0</td>
      <td>4014.0</td>
      <td>3446.0</td>
      <td>4275.0</td>
      <td>4428.0</td>
      <td>4332.0</td>
      <td>3137.0</td>
      <td>3163</td>
      <td>2865</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>7674</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>20252.0</td>
      <td>20878.0</td>
      <td>21323.0</td>
      <td>21661.0</td>
      <td>21091.0</td>
      <td>22077.0</td>
      <td>21431.0</td>
      <td>21171.0</td>
      <td>21169</td>
      <td>21401</td>
    </tr>
    <tr>
      <td>7675</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>32.0</td>
      <td>51.0</td>
      <td>28.0</td>
      <td>25.0</td>
      <td>29.0</td>
      <td>25.0</td>
      <td>6</td>
      <td>5</td>
    </tr>
    <tr>
      <td>7676</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>1122.0</td>
      <td>1177.0</td>
      <td>1238.0</td>
      <td>1287.0</td>
      <td>1226.0</td>
      <td>1187.0</td>
      <td>1175.0</td>
      <td>1188.0</td>
      <td>1128</td>
      <td>1039</td>
    </tr>
    <tr>
      <td>7677</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <td>7678</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>287 rows × 63 columns</p>

```python
raw_data.loc[(raw_data['Area'].str.endswith('many')) & (raw_data['Element'] == 'Feed')]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>7532</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>8006.0</td>
      <td>11084.0</td>
      <td>10644.0</td>
      <td>8993.0</td>
      <td>10669.0</td>
      <td>10608.0</td>
      <td>8189.0</td>
      <td>9242.0</td>
      <td>7868</td>
      <td>7494</td>
    </tr>
    <tr>
      <td>7534</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>7.0</td>
      <td>9.0</td>
      <td>12.0</td>
      <td>17.0</td>
      <td>8.0</td>
      <td>18.0</td>
      <td>13.0</td>
      <td>12.0</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <td>7536</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>7571.0</td>
      <td>6878.0</td>
      <td>7845.0</td>
      <td>6940.0</td>
      <td>7255.0</td>
      <td>7592.0</td>
      <td>7543.0</td>
      <td>6062.0</td>
      <td>6316</td>
      <td>6598</td>
    </tr>
    <tr>
      <td>7538</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>3356.0</td>
      <td>3481.0</td>
      <td>2947.0</td>
      <td>3752.0</td>
      <td>4199.0</td>
      <td>3895.0</td>
      <td>3572.0</td>
      <td>4251.0</td>
      <td>5434</td>
      <td>6136</td>
    </tr>
    <tr>
      <td>7540</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2515</td>
      <td>Rye and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>1400.0</td>
      <td>1300.0</td>
      <td>1287.0</td>
      <td>1391.0</td>
      <td>1858.0</td>
      <td>2416.0</td>
      <td>1625.0</td>
      <td>1360.0</td>
      <td>2150</td>
      <td>3318</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>7661</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2918</td>
      <td>Vegetables</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>89.0</td>
      <td>83.0</td>
      <td>84.0</td>
      <td>84.0</td>
      <td>89.0</td>
      <td>94.0</td>
      <td>89.0</td>
      <td>89.0</td>
      <td>91</td>
      <td>90</td>
    </tr>
    <tr>
      <td>7668</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2945</td>
      <td>Offals</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td>7670</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2946</td>
      <td>Animal fats</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>16.0</td>
      <td>18.0</td>
      <td>20.0</td>
      <td>15.0</td>
      <td>16.0</td>
      <td>14.0</td>
      <td>18.0</td>
      <td>18.0</td>
      <td>20</td>
      <td>17</td>
    </tr>
    <tr>
      <td>7673</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>1813.0</td>
      <td>1701.0</td>
      <td>1595.0</td>
      <td>1633.0</td>
      <td>2224.0</td>
      <td>1503.0</td>
      <td>1468.0</td>
      <td>1771.0</td>
      <td>2088</td>
      <td>2064</td>
    </tr>
    <tr>
      <td>7675</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>11.0</td>
      <td>11.0</td>
      <td>32.0</td>
      <td>51.0</td>
      <td>28.0</td>
      <td>25.0</td>
      <td>29.0</td>
      <td>25.0</td>
      <td>6</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>43 rows × 63 columns</p>

```python
raw_data.loc[(raw_data['Y2004'] < 1000) & (raw_data['Y2004'] > 990)]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3754</td>
      <td>CL</td>
      <td>40</td>
      <td>Chile</td>
      <td>2531</td>
      <td>Potatoes and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-35.68</td>
      <td>-71.54</td>
      <td>...</td>
      <td>998.0</td>
      <td>980.0</td>
      <td>1018.0</td>
      <td>875.0</td>
      <td>884.0</td>
      <td>850.0</td>
      <td>1003.0</td>
      <td>1132.0</td>
      <td>1096</td>
      <td>1089</td>
    </tr>
    <tr>
      <td>7630</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2744</td>
      <td>Eggs</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>999.0</td>
      <td>974.0</td>
      <td>1011.0</td>
      <td>997.0</td>
      <td>1011.0</td>
      <td>1016.0</td>
      <td>1032.0</td>
      <td>1052.0</td>
      <td>990</td>
      <td>1010</td>
    </tr>
    <tr>
      <td>7672</td>
      <td>DE</td>
      <td>79</td>
      <td>Germany</td>
      <td>2949</td>
      <td>Eggs</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>51.17</td>
      <td>10.45</td>
      <td>...</td>
      <td>999.0</td>
      <td>974.0</td>
      <td>1011.0</td>
      <td>997.0</td>
      <td>1011.0</td>
      <td>1016.0</td>
      <td>1032.0</td>
      <td>1052.0</td>
      <td>990</td>
      <td>1010</td>
    </tr>
    <tr>
      <td>9038</td>
      <td>IN</td>
      <td>100</td>
      <td>India</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>20.59</td>
      <td>78.96</td>
      <td>...</td>
      <td>995.0</td>
      <td>954.0</td>
      <td>944.0</td>
      <td>691.0</td>
      <td>740.0</td>
      <td>1276.0</td>
      <td>984.0</td>
      <td>1217.0</td>
      <td>1152</td>
      <td>835</td>
    </tr>
    <tr>
      <td>9104</td>
      <td>IN</td>
      <td>100</td>
      <td>India</td>
      <td>2641</td>
      <td>Pimento</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>20.59</td>
      <td>78.96</td>
      <td>...</td>
      <td>998.0</td>
      <td>844.0</td>
      <td>1012.0</td>
      <td>1010.0</td>
      <td>1019.0</td>
      <td>941.0</td>
      <td>893.0</td>
      <td>954.0</td>
      <td>871</td>
      <td>1018</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>13436</td>
      <td>MM</td>
      <td>28</td>
      <td>Myanmar</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>21.91</td>
      <td>95.96</td>
      <td>...</td>
      <td>992.0</td>
      <td>1069.0</td>
      <td>1117.0</td>
      <td>1253.0</td>
      <td>1298.0</td>
      <td>1472.0</td>
      <td>1581.0</td>
      <td>1660.0</td>
      <td>1600</td>
      <td>1677</td>
    </tr>
    <tr>
      <td>13669</td>
      <td>NP</td>
      <td>149</td>
      <td>Nepal</td>
      <td>2919</td>
      <td>Fruits - Excluding Wine</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>28.39</td>
      <td>84.12</td>
      <td>...</td>
      <td>993.0</td>
      <td>1050.0</td>
      <td>1044.0</td>
      <td>1098.0</td>
      <td>1195.0</td>
      <td>1178.0</td>
      <td>1301.0</td>
      <td>1428.0</td>
      <td>1859</td>
      <td>1700</td>
    </tr>
    <tr>
      <td>15525</td>
      <td>PT</td>
      <td>174</td>
      <td>Portugal</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>39.40</td>
      <td>-8.22</td>
      <td>...</td>
      <td>991.0</td>
      <td>982.0</td>
      <td>995.0</td>
      <td>1002.0</td>
      <td>976.0</td>
      <td>970.0</td>
      <td>978.0</td>
      <td>980.0</td>
      <td>994</td>
      <td>1000</td>
    </tr>
    <tr>
      <td>19714</td>
      <td>TM</td>
      <td>213</td>
      <td>Turkmenistan</td>
      <td>2905</td>
      <td>Cereals - Excluding Beer</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>38.97</td>
      <td>59.56</td>
      <td>...</td>
      <td>995.0</td>
      <td>1009.0</td>
      <td>1031.0</td>
      <td>1019.0</td>
      <td>1017.0</td>
      <td>1023.0</td>
      <td>1019.0</td>
      <td>1039.0</td>
      <td>1036</td>
      <td>1059</td>
    </tr>
    <tr>
      <td>20768</td>
      <td>UZ</td>
      <td>235</td>
      <td>Uzbekistan</td>
      <td>2919</td>
      <td>Fruits - Excluding Wine</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>41.38</td>
      <td>64.59</td>
      <td>...</td>
      <td>993.0</td>
      <td>1079.0</td>
      <td>1448.0</td>
      <td>1538.0</td>
      <td>1693.0</td>
      <td>1875.0</td>
      <td>2108.0</td>
      <td>2159.0</td>
      <td>2422</td>
      <td>2638</td>
    </tr>
  </tbody>
</table>
<p>14 rows × 63 columns</p>

```python
raw_data.loc[(raw_data['Y2004'] < 1000) & (raw_data['Y2004'] > 990), ['Area', 'Item', 'latitude']]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Item</th>
      <th>latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3754</td>
      <td>Chile</td>
      <td>Potatoes and products</td>
      <td>-35.68</td>
    </tr>
    <tr>
      <td>7630</td>
      <td>Germany</td>
      <td>Eggs</td>
      <td>51.17</td>
    </tr>
    <tr>
      <td>7672</td>
      <td>Germany</td>
      <td>Eggs</td>
      <td>51.17</td>
    </tr>
    <tr>
      <td>9038</td>
      <td>India</td>
      <td>Barley and products</td>
      <td>20.59</td>
    </tr>
    <tr>
      <td>9104</td>
      <td>India</td>
      <td>Pimento</td>
      <td>20.59</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>13436</td>
      <td>Myanmar</td>
      <td>Milk - Excluding Butter</td>
      <td>21.91</td>
    </tr>
    <tr>
      <td>13669</td>
      <td>Nepal</td>
      <td>Fruits - Excluding Wine</td>
      <td>28.39</td>
    </tr>
    <tr>
      <td>15525</td>
      <td>Portugal</td>
      <td>Wheat and products</td>
      <td>39.40</td>
    </tr>
    <tr>
      <td>19714</td>
      <td>Turkmenistan</td>
      <td>Cereals - Excluding Beer</td>
      <td>38.97</td>
    </tr>
    <tr>
      <td>20768</td>
      <td>Uzbekistan</td>
      <td>Fruits - Excluding Wine</td>
      <td>41.38</td>
    </tr>
  </tbody>
</table>
<p>14 rows × 3 columns</p>

```python
raw_data.loc[raw_data['Item'].apply(lambda x: len(x.split(' ')) == 5)]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>37</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>205.0</td>
      <td>230.0</td>
      <td>194.0</td>
      <td>232.0</td>
      <td>198.0</td>
      <td>212.0</td>
      <td>257.0</td>
      <td>334.0</td>
      <td>472</td>
      <td>483</td>
    </tr>
    <tr>
      <td>141</td>
      <td>AL</td>
      <td>3</td>
      <td>Albania</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>...</td>
      <td>90.0</td>
      <td>104.0</td>
      <td>112.0</td>
      <td>126.0</td>
      <td>133.0</td>
      <td>139.0</td>
      <td>145.0</td>
      <td>148.0</td>
      <td>148</td>
      <td>153</td>
    </tr>
    <tr>
      <td>267</td>
      <td>DZ</td>
      <td>4</td>
      <td>Algeria</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>28.03</td>
      <td>1.66</td>
      <td>...</td>
      <td>196.0</td>
      <td>240.0</td>
      <td>275.0</td>
      <td>212.0</td>
      <td>333.0</td>
      <td>460.0</td>
      <td>505.0</td>
      <td>342.0</td>
      <td>492</td>
      <td>512</td>
    </tr>
    <tr>
      <td>375</td>
      <td>AO</td>
      <td>7</td>
      <td>Angola</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-11.20</td>
      <td>17.87</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <td>493</td>
      <td>AG</td>
      <td>8</td>
      <td>Antigua and Barbuda</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>17.06</td>
      <td>-61.80</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20941</td>
      <td>VE</td>
      <td>236</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>6.42</td>
      <td>-66.59</td>
      <td>...</td>
      <td>31.0</td>
      <td>33.0</td>
      <td>39.0</td>
      <td>48.0</td>
      <td>55.0</td>
      <td>41.0</td>
      <td>47.0</td>
      <td>53.0</td>
      <td>55</td>
      <td>58</td>
    </tr>
    <tr>
      <td>21053</td>
      <td>VN</td>
      <td>237</td>
      <td>Viet Nam</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>14.06</td>
      <td>108.28</td>
      <td>...</td>
      <td>33.0</td>
      <td>40.0</td>
      <td>41.0</td>
      <td>50.0</td>
      <td>54.0</td>
      <td>77.0</td>
      <td>64.0</td>
      <td>67.0</td>
      <td>53</td>
      <td>55</td>
    </tr>
    <tr>
      <td>21174</td>
      <td>YE</td>
      <td>249</td>
      <td>Yemen</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>15.55</td>
      <td>48.52</td>
      <td>...</td>
      <td>96.0</td>
      <td>101.0</td>
      <td>111.0</td>
      <td>121.0</td>
      <td>123.0</td>
      <td>123.0</td>
      <td>155.0</td>
      <td>138.0</td>
      <td>143</td>
      <td>144</td>
    </tr>
    <tr>
      <td>21294</td>
      <td>ZM</td>
      <td>251</td>
      <td>Zambia</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-13.13</td>
      <td>27.85</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td>21415</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>173 rows × 63 columns</p>

```python
# is same as
TF_indexing = raw_data['Item'].apply(lambda x: len(x.split(' ')) == 5)
raw_data.loc[TF_indexing]
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>37</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>205.0</td>
      <td>230.0</td>
      <td>194.0</td>
      <td>232.0</td>
      <td>198.0</td>
      <td>212.0</td>
      <td>257.0</td>
      <td>334.0</td>
      <td>472</td>
      <td>483</td>
    </tr>
    <tr>
      <td>141</td>
      <td>AL</td>
      <td>3</td>
      <td>Albania</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>...</td>
      <td>90.0</td>
      <td>104.0</td>
      <td>112.0</td>
      <td>126.0</td>
      <td>133.0</td>
      <td>139.0</td>
      <td>145.0</td>
      <td>148.0</td>
      <td>148</td>
      <td>153</td>
    </tr>
    <tr>
      <td>267</td>
      <td>DZ</td>
      <td>4</td>
      <td>Algeria</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>28.03</td>
      <td>1.66</td>
      <td>...</td>
      <td>196.0</td>
      <td>240.0</td>
      <td>275.0</td>
      <td>212.0</td>
      <td>333.0</td>
      <td>460.0</td>
      <td>505.0</td>
      <td>342.0</td>
      <td>492</td>
      <td>512</td>
    </tr>
    <tr>
      <td>375</td>
      <td>AO</td>
      <td>7</td>
      <td>Angola</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-11.20</td>
      <td>17.87</td>
      <td>...</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <td>493</td>
      <td>AG</td>
      <td>8</td>
      <td>Antigua and Barbuda</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>17.06</td>
      <td>-61.80</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20941</td>
      <td>VE</td>
      <td>236</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>6.42</td>
      <td>-66.59</td>
      <td>...</td>
      <td>31.0</td>
      <td>33.0</td>
      <td>39.0</td>
      <td>48.0</td>
      <td>55.0</td>
      <td>41.0</td>
      <td>47.0</td>
      <td>53.0</td>
      <td>55</td>
      <td>58</td>
    </tr>
    <tr>
      <td>21053</td>
      <td>VN</td>
      <td>237</td>
      <td>Viet Nam</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>14.06</td>
      <td>108.28</td>
      <td>...</td>
      <td>33.0</td>
      <td>40.0</td>
      <td>41.0</td>
      <td>50.0</td>
      <td>54.0</td>
      <td>77.0</td>
      <td>64.0</td>
      <td>67.0</td>
      <td>53</td>
      <td>55</td>
    </tr>
    <tr>
      <td>21174</td>
      <td>YE</td>
      <td>249</td>
      <td>Yemen</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>15.55</td>
      <td>48.52</td>
      <td>...</td>
      <td>96.0</td>
      <td>101.0</td>
      <td>111.0</td>
      <td>121.0</td>
      <td>123.0</td>
      <td>123.0</td>
      <td>155.0</td>
      <td>138.0</td>
      <td>143</td>
      <td>144</td>
    </tr>
    <tr>
      <td>21294</td>
      <td>ZM</td>
      <td>251</td>
      <td>Zambia</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-13.13</td>
      <td>27.85</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <td>21415</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2620</td>
      <td>Grapes and products (excl wine)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>173 rows × 63 columns</p>



```python
raw_data.loc[TF_indexing, ['Area', 'Item', 'latitude']]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
      <th>Item</th>
      <th>latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>37</td>
      <td>Afghanistan</td>
      <td>Grapes and products (excl wine)</td>
      <td>33.94</td>
    </tr>
    <tr>
      <td>141</td>
      <td>Albania</td>
      <td>Grapes and products (excl wine)</td>
      <td>41.15</td>
    </tr>
    <tr>
      <td>267</td>
      <td>Algeria</td>
      <td>Grapes and products (excl wine)</td>
      <td>28.03</td>
    </tr>
    <tr>
      <td>375</td>
      <td>Angola</td>
      <td>Grapes and products (excl wine)</td>
      <td>-11.20</td>
    </tr>
    <tr>
      <td>493</td>
      <td>Antigua and Barbuda</td>
      <td>Grapes and products (excl wine)</td>
      <td>17.06</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>20941</td>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>Grapes and products (excl wine)</td>
      <td>6.42</td>
    </tr>
    <tr>
      <td>21053</td>
      <td>Viet Nam</td>
      <td>Grapes and products (excl wine)</td>
      <td>14.06</td>
    </tr>
    <tr>
      <td>21174</td>
      <td>Yemen</td>
      <td>Grapes and products (excl wine)</td>
      <td>15.55</td>
    </tr>
    <tr>
      <td>21294</td>
      <td>Zambia</td>
      <td>Grapes and products (excl wine)</td>
      <td>-13.13</td>
    </tr>
    <tr>
      <td>21415</td>
      <td>Zimbabwe</td>
      <td>Grapes and products (excl wine)</td>
      <td>-19.02</td>
    </tr>
  </tbody>
</table>
<p>173 rows × 3 columns</p>



```python
raw_data_test = raw_data.copy()
raw_data_test.loc[(raw_data_test['Y2004'] < 1000) & (raw_data_test['Y2004'] > 990), ['Area']]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3754</td>
      <td>Chile</td>
    </tr>
    <tr>
      <td>7630</td>
      <td>Germany</td>
    </tr>
    <tr>
      <td>7672</td>
      <td>Germany</td>
    </tr>
    <tr>
      <td>9038</td>
      <td>India</td>
    </tr>
    <tr>
      <td>9104</td>
      <td>India</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>13436</td>
      <td>Myanmar</td>
    </tr>
    <tr>
      <td>13669</td>
      <td>Nepal</td>
    </tr>
    <tr>
      <td>15525</td>
      <td>Portugal</td>
    </tr>
    <tr>
      <td>19714</td>
      <td>Turkmenistan</td>
    </tr>
    <tr>
      <td>20768</td>
      <td>Uzbekistan</td>
    </tr>
  </tbody>
</table>
<p>14 rows × 1 columns</p>



```python
raw_data_test.loc[(raw_data_test['Y2004'] < 1000) & (raw_data_test['Y2004'] > 990), ['Area']] = 'Company'
raw_data_test.loc[(raw_data_test['Y2004'] < 1000) & (raw_data_test['Y2004'] > 980), ['Area']]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>3754</td>
      <td>Company</td>
    </tr>
    <tr>
      <td>4636</td>
      <td>Congo</td>
    </tr>
    <tr>
      <td>5143</td>
      <td>Cuba</td>
    </tr>
    <tr>
      <td>7542</td>
      <td>Germany</td>
    </tr>
    <tr>
      <td>7630</td>
      <td>Company</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>14561</td>
      <td>Norway</td>
    </tr>
    <tr>
      <td>15525</td>
      <td>Company</td>
    </tr>
    <tr>
      <td>15922</td>
      <td>Romania</td>
    </tr>
    <tr>
      <td>19714</td>
      <td>Company</td>
    </tr>
    <tr>
      <td>20768</td>
      <td>Company</td>
    </tr>
  </tbody>
</table>
<p>21 rows × 1 columns</p>



```python
raw_data['Y2007'].sum(), raw_data['Y2007'].mean(), raw_data['Y2007'].median(), raw_data['Y2007'].nunique(), raw_data['Y2007'].count(), raw_data['Y2007'].max(), raw_data['Y2007'].min()
```


    (10867788.0, 508.48210358863986, 7.0, 1994, 21373, 402975.0, 0.0)



```python
[raw_data['Y2007'].sum(),
 raw_data['Y2007'].mean(),
 raw_data['Y2007'].median(),
 raw_data['Y2007'].nunique(),
 raw_data['Y2007'].count(),
 raw_data['Y2007'].max(),
 raw_data['Y2007'].min(),
 raw_data['Y2007'].isna().sum(),
 raw_data['Y2007'].fillna(0)]
```


    [10867788.0,
     508.48210358863986,
     7.0,
     1994,
     21373,
     402975.0,
     0.0,
     104,
     0        4164.0
     1         455.0
     2         263.0
     3          48.0
     4         249.0
               ...  
     21472     356.0
     21473       6.0
     21474      14.0
     21475       0.0
     21476       0.0
     Name: Y2007, Length: 21477, dtype: float64]



```python
# Delete the "Area" column from the dataframe
raw_data.drop("Area", axis=1)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>1928.0</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>183.0</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>76.0</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>237.0</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>210.0</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>230.0</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>27.0</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>6.0</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 62 columns</p>



```python
# alternatively, delete columns using the columns parameter of drop
raw_data.drop(columns="Area")
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>1928.0</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>183.0</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>76.0</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>237.0</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>210.0</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>230.0</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>27.0</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>6.0</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 62 columns</p>



```python
# Delete the Area column from the dataframe and the original 'data' object is changed when inplace=True
raw_data.drop("Area", axis=1, inplace=False)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>1928.0</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>183.0</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>76.0</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>237.0</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>210.0</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>230.0</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>27.0</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>6.0</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 62 columns</p>



```python
# Delete multiple columns from the dataframe
raw_data.drop(["Y2011", "Y2012", "Y2013"], axis=1)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2001</th>
      <th>Y2002</th>
      <th>Y2003</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>2668.0</td>
      <td>2776.0</td>
      <td>3095.0</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>411.0</td>
      <td>448.0</td>
      <td>460.0</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>29.0</td>
      <td>70.0</td>
      <td>48.0</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>83.0</td>
      <td>122.0</td>
      <td>144.0</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>48.0</td>
      <td>89.0</td>
      <td>63.0</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>439.0</td>
      <td>360.0</td>
      <td>386.0</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>16.0</td>
      <td>14.0</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 60 columns</p>



```python
# Delete the rows with labels 0,1,5
raw_data.drop([0,1,5], axis=0)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>6</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2517</td>
      <td>Millet and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>15.0</td>
      <td>21.0</td>
      <td>11.0</td>
      <td>19.0</td>
      <td>21.0</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>14.0</td>
      <td>14</td>
      <td>12</td>
    </tr>
    <tr>
      <td>7</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2520</td>
      <td>Cereals, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21474 rows × 63 columns</p>



```python
# Delete the rows with label "Afghanistan". For label-based deletion, set the index first on the dataframe
raw_data.set_index("Area")
raw_data.set_index("Area").drop("Afghanistan", axis=0)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>Y1961</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
    <tr>
      <th>Area</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Albania</td>
      <td>AL</td>
      <td>3</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>10.0</td>
      <td>...</td>
      <td>28.0</td>
      <td>28.0</td>
      <td>30.0</td>
      <td>28.0</td>
      <td>28.0</td>
      <td>30.0</td>
      <td>26.0</td>
      <td>25.0</td>
      <td>20</td>
      <td>18</td>
    </tr>
    <tr>
      <td>Albania</td>
      <td>AL</td>
      <td>3</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>166.0</td>
      <td>...</td>
      <td>449.0</td>
      <td>468.0</td>
      <td>422.0</td>
      <td>425.0</td>
      <td>435.0</td>
      <td>415.0</td>
      <td>432.0</td>
      <td>439.0</td>
      <td>440</td>
      <td>440</td>
    </tr>
    <tr>
      <td>Albania</td>
      <td>AL</td>
      <td>3</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>Albania</td>
      <td>AL</td>
      <td>3</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>2.0</td>
      <td>...</td>
      <td>23.0</td>
      <td>24.0</td>
      <td>30.0</td>
      <td>27.0</td>
      <td>20.0</td>
      <td>23.0</td>
      <td>24.0</td>
      <td>21.0</td>
      <td>22</td>
      <td>25</td>
    </tr>
    <tr>
      <td>Albania</td>
      <td>AL</td>
      <td>3</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>41.15</td>
      <td>20.17</td>
      <td>2.0</td>
      <td>...</td>
      <td>9.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>7</td>
      <td>7</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>Zimbabwe</td>
      <td>ZW</td>
      <td>181</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>230.0</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>Zimbabwe</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>27.0</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>Zimbabwe</td>
      <td>ZW</td>
      <td>181</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>6.0</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>Zimbabwe</td>
      <td>ZW</td>
      <td>181</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>Zimbabwe</td>
      <td>ZW</td>
      <td>181</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>0.0</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21394 rows × 62 columns</p>



```python
# Delete the first five rows using iloc selector
raw_data.iloc[5:,]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>5</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>231.0</td>
      <td>67.0</td>
      <td>82.0</td>
      <td>67.0</td>
      <td>69.0</td>
      <td>71.0</td>
      <td>82.0</td>
      <td>73.0</td>
      <td>77</td>
      <td>76</td>
    </tr>
    <tr>
      <td>6</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2517</td>
      <td>Millet and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>15.0</td>
      <td>21.0</td>
      <td>11.0</td>
      <td>19.0</td>
      <td>21.0</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>14.0</td>
      <td>14</td>
      <td>12</td>
    </tr>
    <tr>
      <td>7</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2520</td>
      <td>Cereals, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>8</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2531</td>
      <td>Potatoes and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>276.0</td>
      <td>294.0</td>
      <td>294.0</td>
      <td>260.0</td>
      <td>242.0</td>
      <td>250.0</td>
      <td>192.0</td>
      <td>169.0</td>
      <td>196</td>
      <td>230</td>
    </tr>
    <tr>
      <td>9</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2536</td>
      <td>Sugar cane</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>50.0</td>
      <td>29.0</td>
      <td>61.0</td>
      <td>65.0</td>
      <td>54.0</td>
      <td>114.0</td>
      <td>83.0</td>
      <td>83.0</td>
      <td>69</td>
      <td>81</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21472 rows × 63 columns</p>


# Renaming colmnns, exporting and saving Pandas DataFrames
Column renames are achieved easily in Pandas using the DataFrame rename function. The rename function is easy to use, and quite flexible.
- Rename by mapping old names to new names using a dictionary, with form {“old_column_name”: “new_column_name”, …}
- Rename by providing a function to change the column names with. Functions are applied to every column name.
After manipulation or calculations, saving your data back to CSV is the next step.
- to_csv to write a DataFrame to a CSV file,
- to_excel to write DataFrame information to a Microsoft Excel file.


```python
# Renaming of columns
raw_data.rename(columns={'Area':'New_Area'})
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>New_Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>



```python
display(raw_data)
raw_data.rename(columns={'Area':'New_Area'}, inplace=False)
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>



<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>New_Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>



```python
raw_data.rename(columns={'Area':'New_Area',
                         'Y2013':'Year_2013'}, inplace=False)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>New_Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Year_2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>



```python
raw_data.rename(columns=lambda x: x.upper().replace(' ', '_'), inplace=False)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AREA_ABBREVIATION</th>
      <th>AREA_CODE</th>
      <th>AREA</th>
      <th>ITEM_CODE</th>
      <th>ITEM</th>
      <th>ELEMENT_CODE</th>
      <th>ELEMENT</th>
      <th>UNIT</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>2</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>58.0</td>
      <td>236.0</td>
      <td>262.0</td>
      <td>263.0</td>
      <td>230.0</td>
      <td>379.0</td>
      <td>315.0</td>
      <td>203.0</td>
      <td>367</td>
      <td>360</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>4</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>120.0</td>
      <td>208.0</td>
      <td>233.0</td>
      <td>249.0</td>
      <td>247.0</td>
      <td>195.0</td>
      <td>178.0</td>
      <td>191.0</td>
      <td>200</td>
      <td>200</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21473</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5521</td>
      <td>Feed</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>5.0</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>6.0</td>
      <td>9.0</td>
      <td>5.0</td>
      <td>15.0</td>
      <td>15.0</td>
      <td>15</td>
      <td>15</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>21477 rows × 63 columns</p>



```python
# Exporting and saving
# Output data to a CSV file
# If you don't want row numbers in my output file, hence index=False, and to avoid character issues, you typically use utf8 encoding for input/output.
raw_data.to_csv("Tutorial_Pandas_Output_Filename.csv", index=False, encoding='utf8')
# Output data to an Excel file.
# For the excel output to work, you may need to install the "xlsxwriter" package.
raw_data.to_excel("Tutorial_Pandas_Output_Filename.xlsx", sheet_name="Sheet 1", index=False)
```
# Summarising, grouping, and aggregating data in Pandas
The .describe() function is a useful summarisation tool that will quickly display statistics for any variable or group it is applied to. 
The describe() output varies depending on whether you apply it to a numeric or character column. 
| Function | Description                         |
|----------|-------------------------------------|
| count    | Number of non-null observations     |
| sum      | Sum of values                       |
| mean     | Mean of values                      |
| mad      | Mean absolute deviation             |
| median   | Arithmetic median of values         |
| min      | Minimum                             |
| max      | Maximum                             |
| mode     | Mode                                |
| abs      | Absolute Value                      |
| prod     | Product of values                   |
| std      | Unbiased standard deviation         |
| var      | Unbiased variance                   |
| sem      | Unbiased standard error of the mean |
| skew     | Unbiased skewness (3rd moment)      |
| kurt     | Unbiased kurtosis (4th moment)      |
| quantile | Sample quantile (value at %)        |
| cumsum   | Cumulative sum                      |
| cumprod  | Cumulative product                  |
| cummax   | Cumulative maximum                  |
| cummin   | Cumulative minimum                  |
We'll be grouping large data frames by different variables, and applying summary functions on each group. 
This is accomplished in Pandas using the “groupby()” and “agg()” functions of Panda’s DataFrame objects. 
(http://pandas.pydata.org/pandas-docs/stable/groupby.html)
- Groupby essentially splits the data into different groups depending on a variable of your choice. 
- The groupby() function returns a GroupBy object, but essentially describes how the rows of the original data set has been split.
- The GroupBy object.groups variable is a dictionary whose keys are the computed unique groups and corresponding values being the axis labels belonging to each group. 
- Functions like max(), min(), mean(), first(), last() can be quickly applied to the GroupBy object to obtain summary statistics for each group – an immensely useful function. 
- If you calculate more than one column of results, your result will be a Dataframe. For a single column of results, the agg function, by default, will produce a Series. You can change this by selecting your operation column differently (ex. [[]])
- The groupby output will have an index or multi-index on rows corresponding to your chosen grouping variables. To avoid setting this index, pass “as_index=False” to the groupby operation. 
The aggregation functionality provided by the agg() function allows multiple statistics to be calculated per group in one calculation.
![](https://shanelynnwebsite-mid9n9g1q9y8tt.netdna-ssl.com/wp-content/uploads/2016/03/pandas_aggregation-1024x409.png)
- When multiple statistics are calculated on columns, the resulting dataframe will have a multi-index set on the column axis. This can be difficult to work with, and be better to rename columns after a groupby operation.
- A neater approach is using the ravel() method on the grouped columns. Ravel() turns a Pandas multi-index into a simpler array, which we can combine into sensible column names.


```python
# Summarising
url_path = 'https://shanelynnwebsite-mid9n9g1q9y8tt.netdna-ssl.com/wp-content/uploads/2015/06/phone_data.csv'
raw_phone = pd.read_csv(url_path)
raw_phone
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>date</th>
      <th>duration</th>
      <th>item</th>
      <th>month</th>
      <th>network</th>
      <th>network_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0</td>
      <td>15/10/14 06:58</td>
      <td>34.429</td>
      <td>data</td>
      <td>2014-11</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1</td>
      <td>15/10/14 06:58</td>
      <td>13.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Vodafone</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2</td>
      <td>15/10/14 14:46</td>
      <td>23.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Meteor</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>3</td>
      <td>3</td>
      <td>15/10/14 14:48</td>
      <td>4.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Tesco</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4</td>
      <td>15/10/14 17:27</td>
      <td>4.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Tesco</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>825</td>
      <td>825</td>
      <td>13/03/15 00:38</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>world</td>
      <td>world</td>
    </tr>
    <tr>
      <td>826</td>
      <td>826</td>
      <td>13/03/15 00:39</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>Vodafone</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>827</td>
      <td>827</td>
      <td>13/03/15 06:58</td>
      <td>34.429</td>
      <td>data</td>
      <td>2015-03</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>828</td>
      <td>828</td>
      <td>14/03/15 00:13</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>world</td>
      <td>world</td>
    </tr>
    <tr>
      <td>829</td>
      <td>829</td>
      <td>14/03/15 00:16</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>world</td>
      <td>world</td>
    </tr>
  </tbody>
</table>
<p>830 rows × 7 columns</p>



```python
if 'date' in raw_phone.columns:
    raw_phone['date'] = pd.to_datetime(raw_phone['date'])
raw_phone
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>date</th>
      <th>duration</th>
      <th>item</th>
      <th>month</th>
      <th>network</th>
      <th>network_type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>0</td>
      <td>2014-10-15 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>2014-11</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1</td>
      <td>2014-10-15 06:58:00</td>
      <td>13.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Vodafone</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2</td>
      <td>2014-10-15 14:46:00</td>
      <td>23.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Meteor</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>3</td>
      <td>3</td>
      <td>2014-10-15 14:48:00</td>
      <td>4.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Tesco</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>4</td>
      <td>4</td>
      <td>2014-10-15 17:27:00</td>
      <td>4.000</td>
      <td>call</td>
      <td>2014-11</td>
      <td>Tesco</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>825</td>
      <td>825</td>
      <td>2015-03-13 00:38:00</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>world</td>
      <td>world</td>
    </tr>
    <tr>
      <td>826</td>
      <td>826</td>
      <td>2015-03-13 00:39:00</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>Vodafone</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>827</td>
      <td>827</td>
      <td>2015-03-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>2015-03</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>828</td>
      <td>828</td>
      <td>2015-03-14 00:13:00</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>world</td>
      <td>world</td>
    </tr>
    <tr>
      <td>829</td>
      <td>829</td>
      <td>2015-03-14 00:16:00</td>
      <td>1.000</td>
      <td>sms</td>
      <td>2015-03</td>
      <td>world</td>
      <td>world</td>
    </tr>
  </tbody>
</table>
<p>830 rows × 7 columns</p>



```python
raw_phone['duration'].max()
```


    10528.0



```python
raw_phone['item'].unique()
```


    array(['data', 'call', 'sms'], dtype=object)



```python
raw_phone['duration'][raw_phone['item'] == 'data'].max()
```


    34.429



```python
raw_phone['network'].unique()
```


    array(['data', 'Vodafone', 'Meteor', 'Tesco', 'Three', 'voicemail',
           'landline', 'special', 'world'], dtype=object)



```python
raw_phone['month'].value_counts()
```


    2014-11    230
    2015-01    205
    2014-12    157
    2015-02    137
    2015-03    101
    Name: month, dtype: int64



```python
# Grouping
raw_phone.groupby(['month']).groups
```


    {'2014-11': Int64Index([  0,   1,   2,   3,   4,   5,   6,   7,   8,   9,
                 ...
                 220, 221, 222, 223, 224, 225, 226, 227, 229, 230],
                dtype='int64', length=230),
     '2014-12': Int64Index([228, 231, 232, 233, 234, 235, 236, 237, 238, 239,
                 ...
                 377, 378, 379, 380, 382, 383, 384, 385, 387, 388],
                dtype='int64', length=157),
     '2015-01': Int64Index([381, 386, 389, 390, 391, 392, 393, 394, 395, 396,
                 ...
                 583, 584, 585, 587, 588, 589, 590, 591, 592, 593],
                dtype='int64', length=205),
     '2015-02': Int64Index([577, 586, 594, 595, 596, 597, 598, 599, 600, 601,
                 ...
                 719, 720, 721, 722, 723, 724, 725, 726, 727, 728],
                dtype='int64', length=137),
     '2015-03': Int64Index([729, 730, 731, 732, 733, 734, 735, 736, 737, 738,
                 ...
                 820, 821, 822, 823, 824, 825, 826, 827, 828, 829],
                dtype='int64', length=101)}



```python
raw_phone.groupby(['month']).groups.keys()
```


    dict_keys(['2014-11', '2014-12', '2015-01', '2015-02', '2015-03'])



```python
raw_phone.groupby(['month']).first()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>index</th>
      <th>date</th>
      <th>duration</th>
      <th>item</th>
      <th>network</th>
      <th>network_type</th>
    </tr>
    <tr>
      <th>month</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>2014-11</td>
      <td>0</td>
      <td>2014-10-15 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>2014-12</td>
      <td>228</td>
      <td>2014-11-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>2015-01</td>
      <td>381</td>
      <td>2014-12-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>2015-02</td>
      <td>577</td>
      <td>2015-01-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>2015-03</td>
      <td>729</td>
      <td>2015-12-02 20:15:00</td>
      <td>69.000</td>
      <td>call</td>
      <td>landline</td>
      <td>landline</td>
    </tr>
  </tbody>
</table>



```python
raw_phone.groupby(['month'])['duration'].sum()
```


    month
    2014-11    26639.441
    2014-12    14641.870
    2015-01    18223.299
    2015-02    15522.299
    2015-03    22750.441
    Name: duration, dtype: float64



```python
raw_phone.groupby(['month'], as_index=False)[['duration']].sum()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>month</th>
      <th>duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2014-11</td>
      <td>26639.441</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2014-12</td>
      <td>14641.870</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2015-01</td>
      <td>18223.299</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2015-02</td>
      <td>15522.299</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2015-03</td>
      <td>22750.441</td>
    </tr>
  </tbody>
</table>



```python
raw_phone.groupby(['month'])['date'].count()
```


    month
    2014-11    230
    2014-12    157
    2015-01    205
    2015-02    137
    2015-03    101
    Name: date, dtype: int64



```python
raw_phone[raw_phone['item'] == 'call'].groupby('network')['duration'].sum()
```


    network
    Meteor        7200.0
    Tesco        13828.0
    Three        36464.0
    Vodafone     14621.0
    landline     18433.0
    voicemail     1775.0
    Name: duration, dtype: float64



```python
raw_phone.groupby(['month', 'item']).groups
```


    {('2014-11',
      'call'): Int64Index([  1,   2,   3,   4,   5,   7,   8,   9,  10,  19,
                 ...
                 194, 195, 196, 200, 201, 203, 216, 222, 223, 224],
                dtype='int64', length=107),
     ('2014-11',
      'data'): Int64Index([  0,   6,  13,  26,  39,  45,  54,  56,  58,  66,  80,  81,  87,
                  92,  95,  97, 101, 111, 114, 120, 131, 151, 159, 170, 182, 189,
                 192, 199, 208],
                dtype='int64'),
     ('2014-11',
      'sms'): Int64Index([ 11,  12,  14,  15,  16,  17,  18,  22,  23,  24,  25,  33,  36,
                  37,  38,  52,  53,  61,  62,  63,  67,  68,  69,  70,  71,  72,
                  73,  74,  75,  76,  77,  79, 102, 103, 107, 108, 121, 125, 132,
                 133, 134, 135, 138, 142, 143, 144, 145, 148, 149, 153, 154, 155,
                 157, 158, 160, 161, 167, 173, 174, 175, 176, 177, 178, 179, 180,
                 181, 185, 186, 187, 188, 197, 198, 202, 204, 205, 206, 207, 209,
                 210, 211, 212, 213, 214, 215, 217, 218, 219, 220, 221, 225, 226,
                 227, 229, 230],
                dtype='int64'),
     ('2014-12',
      'call'): Int64Index([232, 236, 250, 251, 252, 255, 256, 258, 259, 260, 261, 267, 268,
                 269, 270, 271, 272, 273, 274, 276, 277, 278, 279, 280, 282, 283,
                 284, 285, 286, 287, 290, 292, 295, 297, 298, 299, 300, 301, 302,
                 303, 306, 309, 311, 312, 313, 314, 320, 322, 327, 329, 337, 342,
                 344, 345, 347, 348, 349, 350, 352, 353, 354, 356, 362, 364, 365,
                 366, 367, 368, 369, 373, 375, 379, 380, 382, 383, 384, 385, 387,
                 388],
                dtype='int64'),
     ('2014-12',
      'data'): Int64Index([228, 231, 234, 235, 237, 238, 249, 254, 263, 275, 281, 288, 291,
                 305, 321, 324, 328, 330, 338, 341, 343, 346, 351, 355, 363, 372,
                 374, 376, 377, 378],
                dtype='int64'),
     ('2014-12',
      'sms'): Int64Index([233, 239, 240, 241, 242, 243, 244, 245, 246, 247, 248, 253, 257,
                 262, 264, 265, 266, 289, 293, 294, 296, 304, 307, 308, 310, 315,
                 316, 317, 318, 319, 323, 325, 326, 331, 332, 333, 334, 335, 336,
                 339, 340, 357, 358, 359, 360, 361, 370, 371],
                dtype='int64'),
     ('2015-01',
      'call'): Int64Index([392, 398, 401, 402, 403, 404, 405, 406, 407, 408, 411, 412, 413,
                 414, 415, 416, 417, 418, 423, 425, 428, 431, 433, 438, 441, 442,
                 444, 445, 446, 448, 450, 451, 454, 455, 456, 457, 459, 460, 461,
                 466, 475, 497, 498, 499, 506, 510, 511, 513, 514, 515, 517, 518,
                 519, 520, 523, 524, 525, 526, 527, 528, 532, 533, 535, 536, 542,
                 543, 544, 545, 547, 548, 557, 558, 559, 561, 562, 563, 564, 565,
                 570, 572, 573, 574, 578, 584, 585, 587, 588, 589],
                dtype='int64'),
     ('2015-01',
      'data'): Int64Index([381, 386, 389, 396, 397, 400, 409, 420, 426, 427, 443, 453, 463,
                 465, 468, 473, 474, 476, 496, 504, 505, 509, 512, 516, 529, 537,
                 541, 555, 560, 568, 571],
                dtype='int64'),
     ('2015-01',
      'sms'): Int64Index([390, 391, 393, 394, 395, 399, 410, 419, 421, 422, 424, 429, 430,
                 432, 434, 435, 436, 437, 439, 440, 447, 449, 452, 458, 462, 464,
                 467, 469, 470, 471, 472, 477, 478, 479, 480, 481, 482, 483, 484,
                 485, 486, 487, 488, 489, 490, 491, 492, 493, 494, 495, 500, 501,
                 502, 503, 507, 508, 521, 522, 530, 531, 534, 538, 539, 540, 546,
                 549, 550, 551, 552, 553, 554, 556, 566, 567, 569, 575, 576, 579,
                 580, 581, 582, 583, 590, 591, 592, 593],
                dtype='int64'),
     ('2015-02',
      'call'): Int64Index([595, 597, 599, 600, 601, 602, 603, 611, 612, 614, 615, 618, 619,
                 620, 626, 627, 629, 630, 631, 632, 633, 635, 636, 638, 639, 640,
                 644, 647, 648, 650, 651, 653, 654, 656, 657, 658, 662, 664, 665,
                 666, 668, 669, 671, 672, 674, 676, 677, 682, 684, 686, 687, 690,
                 691, 693, 694, 695, 696, 700, 702, 708, 709, 710, 711, 716, 718,
                 719, 720],
                dtype='int64'),
     ('2015-02',
      'data'): Int64Index([577, 586, 594, 598, 610, 613, 616, 621, 625, 634, 637, 645, 646,
                 649, 652, 655, 659, 667, 670, 673, 675, 683, 685, 689, 692, 697,
                 715, 717, 725, 727, 728],
                dtype='int64'),
     ('2015-02',
      'sms'): Int64Index([596, 604, 605, 606, 607, 608, 609, 617, 622, 623, 624, 628, 641,
                 642, 643, 660, 661, 663, 678, 679, 680, 681, 688, 698, 699, 701,
                 703, 704, 705, 706, 707, 712, 713, 714, 721, 722, 723, 724, 726],
                dtype='int64'),
     ('2015-03',
      'call'): Int64Index([729, 730, 732, 733, 735, 736, 738, 741, 742, 744, 745, 752, 756,
                 758, 762, 763, 764, 769, 770, 771, 773, 774, 776, 777, 778, 780,
                 781, 782, 783, 784, 785, 786, 787, 788, 792, 799, 800, 801, 802,
                 803, 805, 806, 807, 808, 809, 810, 816],
                dtype='int64'),
     ('2015-03',
      'data'): Int64Index([731, 734, 737, 739, 740, 743, 746, 751, 753, 754, 755, 757, 761,
                 772, 775, 779, 791, 793, 804, 811, 817, 818, 819, 820, 821, 822,
                 823, 824, 827],
                dtype='int64'),
     ('2015-03',
      'sms'): Int64Index([747, 748, 749, 750, 759, 760, 765, 766, 767, 768, 789, 790, 794,
                 795, 796, 797, 798, 812, 813, 814, 815, 825, 826, 828, 829],
                dtype='int64')}



```python
raw_phone.groupby(['month', 'item']).groups.keys()
```


    dict_keys([('2014-11', 'call'), ('2014-11', 'data'), ('2014-11', 'sms'), ('2014-12', 'call'), ('2014-12', 'data'), ('2014-12', 'sms'), ('2015-01', 'call'), ('2015-01', 'data'), ('2015-01', 'sms'), ('2015-02', 'call'), ('2015-02', 'data'), ('2015-02', 'sms'), ('2015-03', 'call'), ('2015-03', 'data'), ('2015-03', 'sms')])



```python
raw_phone.groupby(['month', 'item']).first()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>index</th>
      <th>date</th>
      <th>duration</th>
      <th>network</th>
      <th>network_type</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1</td>
      <td>2014-10-15 06:58:00</td>
      <td>13.000</td>
      <td>Vodafone</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td>data</td>
      <td>0</td>
      <td>2014-10-15 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>11</td>
      <td>2014-10-16 22:18:00</td>
      <td>1.000</td>
      <td>Meteor</td>
      <td>mobile</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>232</td>
      <td>2014-11-14 17:24:00</td>
      <td>124.000</td>
      <td>voicemail</td>
      <td>voicemail</td>
    </tr>
    <tr>
      <td>data</td>
      <td>228</td>
      <td>2014-11-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>577</td>
      <td>2015-01-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>596</td>
      <td>2015-01-15 12:23:00</td>
      <td>1.000</td>
      <td>special</td>
      <td>special</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>729</td>
      <td>2015-12-02 20:15:00</td>
      <td>69.000</td>
      <td>landline</td>
      <td>landline</td>
    </tr>
    <tr>
      <td>data</td>
      <td>731</td>
      <td>2015-02-13 06:58:00</td>
      <td>34.429</td>
      <td>data</td>
      <td>data</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>747</td>
      <td>2015-02-19 18:46:00</td>
      <td>1.000</td>
      <td>Vodafone</td>
      <td>mobile</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 5 columns</p>



```python
raw_phone.groupby(['month', 'item'])['duration'].sum()
```


    month    item
    2014-11  call    25547.000
             data      998.441
             sms        94.000
    2014-12  call    13561.000
             data     1032.870
                       ...    
    2015-02  data     1067.299
             sms        39.000
    2015-03  call    21727.000
             data      998.441
             sms        25.000
    Name: duration, Length: 15, dtype: float64



```python
raw_phone.groupby(['month', 'item'])['date'].count()
```


    month    item
    2014-11  call    107
             data     29
             sms      94
    2014-12  call     79
             data     30
                    ... 
    2015-02  data     31
             sms      39
    2015-03  call     47
             data     29
             sms      25
    Name: date, Length: 15, dtype: int64



```python
raw_phone.groupby(['month', 'network_type'])['date'].count()
```


    month    network_type
    2014-11  data             29
             landline          5
             mobile          189
             special           1
             voicemail         6
                            ... 
    2015-03  data             29
             landline         11
             mobile           54
             voicemail         4
             world             3
    Name: date, Length: 24, dtype: int64



```python
raw_phone.groupby(['month', 'network_type'])[['date']].count()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>date</th>
    </tr>
    <tr>
      <th>month</th>
      <th>network_type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="5" valign="top">2014-11</td>
      <td>data</td>
      <td>29</td>
    </tr>
    <tr>
      <td>landline</td>
      <td>5</td>
    </tr>
    <tr>
      <td>mobile</td>
      <td>189</td>
    </tr>
    <tr>
      <td>special</td>
      <td>1</td>
    </tr>
    <tr>
      <td>voicemail</td>
      <td>6</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="5" valign="top">2015-03</td>
      <td>data</td>
      <td>29</td>
    </tr>
    <tr>
      <td>landline</td>
      <td>11</td>
    </tr>
    <tr>
      <td>mobile</td>
      <td>54</td>
    </tr>
    <tr>
      <td>voicemail</td>
      <td>4</td>
    </tr>
    <tr>
      <td>world</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>24 rows × 1 columns</p>



```python
raw_phone.groupby(['month', 'network_type'], as_index=False)[['date']].count()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>month</th>
      <th>network_type</th>
      <th>date</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2014-11</td>
      <td>data</td>
      <td>29</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2014-11</td>
      <td>landline</td>
      <td>5</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2014-11</td>
      <td>mobile</td>
      <td>189</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2014-11</td>
      <td>special</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2014-11</td>
      <td>voicemail</td>
      <td>6</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>19</td>
      <td>2015-03</td>
      <td>data</td>
      <td>29</td>
    </tr>
    <tr>
      <td>20</td>
      <td>2015-03</td>
      <td>landline</td>
      <td>11</td>
    </tr>
    <tr>
      <td>21</td>
      <td>2015-03</td>
      <td>mobile</td>
      <td>54</td>
    </tr>
    <tr>
      <td>22</td>
      <td>2015-03</td>
      <td>voicemail</td>
      <td>4</td>
    </tr>
    <tr>
      <td>23</td>
      <td>2015-03</td>
      <td>world</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>24 rows × 3 columns</p>



```python
raw_phone.groupby(['month', 'network_type'])[['date']].count().shape
```


    (24, 1)



```python
raw_phone.groupby(['month', 'network_type'], as_index=False)[['date']].count().shape
```


    (24, 3)



```python
# Aggregating
raw_phone.groupby(['month'], as_index=False)[['duration']].sum()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>month</th>
      <th>duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2014-11</td>
      <td>26639.441</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2014-12</td>
      <td>14641.870</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2015-01</td>
      <td>18223.299</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2015-02</td>
      <td>15522.299</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2015-03</td>
      <td>22750.441</td>
    </tr>
  </tbody>
</table>



```python
# is same as
raw_phone.groupby(['month'], as_index=False).agg({'duration':'sum'})
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>month</th>
      <th>duration</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2014-11</td>
      <td>26639.441</td>
    </tr>
    <tr>
      <td>1</td>
      <td>2014-12</td>
      <td>14641.870</td>
    </tr>
    <tr>
      <td>2</td>
      <td>2015-01</td>
      <td>18223.299</td>
    </tr>
    <tr>
      <td>3</td>
      <td>2015-02</td>
      <td>15522.299</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2015-03</td>
      <td>22750.441</td>
    </tr>
  </tbody>
</table>



```python
raw_phone.groupby(['month', 'item']).agg({'duration':'sum',
                                          'network_type':'count',
                                          'date':'first'})
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>duration</th>
      <th>network_type</th>
      <th>date</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 3 columns</p>



```python
# is same as
aggregation_logic = {'duration':'sum',
                     'network_type':'count',
                     'date':'first'}
raw_phone.groupby(['month', 'item']).agg(aggregation_logic)
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>duration</th>
      <th>network_type</th>
      <th>date</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 3 columns</p>



```python
aggregation_logic = {'duration':[min, max, sum],
                     'network_type':'count',
                     'date':[min, 'first', 'nunique']}
raw_phone.groupby(['month', 'item']).agg(aggregation_logic)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }
    .dataframe thead tr th {
        text-align: left;
    }
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3" halign="left">duration</th>
      <th>network_type</th>
      <th colspan="3" halign="left">date</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>min</th>
      <th>max</th>
      <th>sum</th>
      <th>count</th>
      <th>min</th>
      <th>first</th>
      <th>nunique</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1.000</td>
      <td>1940.000</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-01-11 15:13:00</td>
      <td>2014-10-15 06:58:00</td>
      <td>104</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-01-11 06:58:00</td>
      <td>2014-10-15 06:58:00</td>
      <td>29</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-03-11 08:40:00</td>
      <td>2014-10-16 22:18:00</td>
      <td>79</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>2.000</td>
      <td>2120.000</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-02-12 11:40:00</td>
      <td>2014-11-14 17:24:00</td>
      <td>76</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-01-12 06:58:00</td>
      <td>2014-11-13 06:58:00</td>
      <td>30</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-02 06:58:00</td>
      <td>2015-01-13 06:58:00</td>
      <td>31</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
      <td>2015-01-15 12:23:00</td>
      <td>27</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>2.000</td>
      <td>10528.000</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-01-03 12:19:00</td>
      <td>2015-12-02 20:15:00</td>
      <td>47</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-01-03 06:58:00</td>
      <td>2015-02-13 06:58:00</td>
      <td>29</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-03 09:19:00</td>
      <td>2015-02-19 18:46:00</td>
      <td>17</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 7 columns</p>



```python
aggregation_logic = {'duration':[min, max, sum],
                     'network_type':'count',
                     'date':['first', lambda x: max(x)-min(x)]}
raw_phone.groupby(['month', 'item']).agg(aggregation_logic)
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }
    .dataframe thead tr th {
        text-align: left;
    }
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3" halign="left">duration</th>
      <th>network_type</th>
      <th colspan="2" halign="left">date</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>min</th>
      <th>max</th>
      <th>sum</th>
      <th>count</th>
      <th>first</th>
      <th>&lt;lambda_0&gt;</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1.000</td>
      <td>1940.000</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 03:48:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
      <td>275 days 10:40:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>2.000</td>
      <td>2120.000</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
      <td>305 days 08:14:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
      <td>260 days 09:17:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>2.000</td>
      <td>10528.000</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
      <td>333 days 08:32:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
      <td>59 days 01:11:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 6 columns</p>



```python
raw_phone_test = raw_phone.groupby(['month', 'item']).agg(aggregation_logic)
raw_phone_test
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }
    .dataframe thead tr th {
        text-align: left;
    }
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3" halign="left">duration</th>
      <th>network_type</th>
      <th colspan="2" halign="left">date</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>min</th>
      <th>max</th>
      <th>sum</th>
      <th>count</th>
      <th>first</th>
      <th>&lt;lambda_0&gt;</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1.000</td>
      <td>1940.000</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 03:48:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
      <td>275 days 10:40:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>2.000</td>
      <td>2120.000</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
      <td>305 days 08:14:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
      <td>260 days 09:17:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>2.000</td>
      <td>10528.000</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
      <td>333 days 08:32:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
      <td>59 days 01:11:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 6 columns</p>



```python
raw_phone_test.columns = raw_phone_test.columns.droplevel(level=0)
raw_phone_test
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>min</th>
      <th>max</th>
      <th>sum</th>
      <th>count</th>
      <th>first</th>
      <th>&lt;lambda_0&gt;</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1.000</td>
      <td>1940.000</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 03:48:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
      <td>275 days 10:40:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>2.000</td>
      <td>2120.000</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
      <td>305 days 08:14:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
      <td>260 days 09:17:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>2.000</td>
      <td>10528.000</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
      <td>333 days 08:32:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
      <td>59 days 01:11:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 6 columns</p>



```python
raw_phone_test.rename(columns={'min':'min_duration',
                               'max':'max_duration',
                               'sum':'sum_duration',
                               '<lambda>':'date_difference'})
raw_phone_test = raw_phone.groupby(['month', 'item']).agg(aggregation_logic)
raw_phone_test
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }
    .dataframe tbody tr th {
        vertical-align: top;
    }
    .dataframe thead tr th {
        text-align: left;
    }
    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th></th>
      <th colspan="3" halign="left">duration</th>
      <th>network_type</th>
      <th colspan="2" halign="left">date</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th>min</th>
      <th>max</th>
      <th>sum</th>
      <th>count</th>
      <th>first</th>
      <th>&lt;lambda_0&gt;</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1.000</td>
      <td>1940.000</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 03:48:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
      <td>275 days 10:40:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>2.000</td>
      <td>2120.000</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
      <td>305 days 08:14:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
      <td>260 days 09:17:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>2.000</td>
      <td>10528.000</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
      <td>333 days 08:32:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
      <td>59 days 01:11:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 6 columns</p>



```python
raw_phone_test.columns = ['_'.join(x) for x in raw_phone_test.columns.ravel()]
raw_phone_test
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>duration_min</th>
      <th>duration_max</th>
      <th>duration_sum</th>
      <th>network_type_count</th>
      <th>date_first</th>
      <th>date_&lt;lambda_0&gt;</th>
    </tr>
    <tr>
      <th>month</th>
      <th>item</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td rowspan="3" valign="top">2014-11</td>
      <td>call</td>
      <td>1.000</td>
      <td>1940.000</td>
      <td>25547.000</td>
      <td>107</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 03:48:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2014-10-15 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>94.000</td>
      <td>94</td>
      <td>2014-10-16 22:18:00</td>
      <td>275 days 10:40:00</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2014-12</td>
      <td>call</td>
      <td>2.000</td>
      <td>2120.000</td>
      <td>13561.000</td>
      <td>79</td>
      <td>2014-11-14 17:24:00</td>
      <td>305 days 08:14:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1032.870</td>
      <td>30</td>
      <td>2014-11-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td rowspan="2" valign="top">2015-02</td>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>1067.299</td>
      <td>31</td>
      <td>2015-01-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>39.000</td>
      <td>39</td>
      <td>2015-01-15 12:23:00</td>
      <td>260 days 09:17:00</td>
    </tr>
    <tr>
      <td rowspan="3" valign="top">2015-03</td>
      <td>call</td>
      <td>2.000</td>
      <td>10528.000</td>
      <td>21727.000</td>
      <td>47</td>
      <td>2015-12-02 20:15:00</td>
      <td>333 days 08:32:00</td>
    </tr>
    <tr>
      <td>data</td>
      <td>34.429</td>
      <td>34.429</td>
      <td>998.441</td>
      <td>29</td>
      <td>2015-02-13 06:58:00</td>
      <td>334 days 00:00:00</td>
    </tr>
    <tr>
      <td>sms</td>
      <td>1.000</td>
      <td>1.000</td>
      <td>25.000</td>
      <td>25</td>
      <td>2015-02-19 18:46:00</td>
      <td>59 days 01:11:00</td>
    </tr>
  </tbody>
</table>
<p>15 rows × 6 columns</p>


# Merge and join DataFrames with Pandas
(http://pandas.pydata.org/pandas-docs/stable/merging.html)  
In any real world data science situation with Python, you’ll be about 10 minutes in when you’ll need to merge or join Pandas Dataframes together to form your analysis dataset. 
Merging and joining dataframes is a core process that any aspiring data analyst will need to master.
- “Merging” two datasets is the process of bringing two datasets together into one, and aligning the rows from each based on common attributes or columns.
- The merging operation at its simplest takes a left dataframe (the first argument), a right dataframe (the second argument), and then a merge column name, or a column to merge “on”.
- In the output/result, rows from the left and right dataframes are matched up where there are common values of the merge column specified by “on”.
- By default, the Pandas merge operation acts with an “inner” merge.
There are three different types of merges available in Pandas. 
These merge types are common across most database and data-orientated languages (SQL, R, SAS) and are typically referred to as “joins”.
- Inner Merge / Inner join – The default Pandas behaviour, only keep rows where the merge “on” value exists in both the left and right dataframes.
- Left Merge / Left outer join – (aka left merge or left join) Keep every row in the left dataframe. Where there are missing values of the “on” variable in the right dataframe, add empty / NaN values in the result.
- Right Merge / Right outer join – (aka right merge or right join) Keep every row in the right dataframe. Where there are missing values of the “on” variable in the left column, add empty / NaN values in the result.
- Outer Merge / Full outer join – A full outer join returns all the rows from the left dataframe, all the rows from the right dataframe, and matches up rows where possible, with NaNs elsewhere.
![](https://i.stack.imgur.com/hMKKt.jpg)


```python
# Merge and join od dataframes
user_usage = pd.read_csv('https://raw.githubusercontent.com/shanealynn/Pandas-Merge-Tutorial/master/user_usage.csv')
user_device = pd.read_csv('https://raw.githubusercontent.com/shanealynn/Pandas-Merge-Tutorial/master/user_device.csv')
device_info = pd.read_csv('https://raw.githubusercontent.com/shanealynn/Pandas-Merge-Tutorial/master/android_devices.csv')
display(user_usage.head())
display(user_device.head())
display(device_info.head())
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22789</td>
    </tr>
    <tr>
      <td>3</td>
      <td>94.46</td>
      <td>35.17</td>
      <td>519.12</td>
      <td>22790</td>
    </tr>
    <tr>
      <td>4</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22792</td>
    </tr>
  </tbody>
</table>


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>22782</td>
      <td>26980</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,2</td>
      <td>2</td>
    </tr>
    <tr>
      <td>1</td>
      <td>22783</td>
      <td>29628</td>
      <td>android</td>
      <td>6.0</td>
      <td>Nexus 5</td>
      <td>3</td>
    </tr>
    <tr>
      <td>2</td>
      <td>22784</td>
      <td>28473</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G903F</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>22785</td>
      <td>15200</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>4</td>
      <td>22786</td>
      <td>28239</td>
      <td>android</td>
      <td>6.0</td>
      <td>ONE E1003</td>
      <td>1</td>
    </tr>
  </tbody>
</table>


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Retail Branding</th>
      <th>Marketing Name</th>
      <th>Device</th>
      <th>Model</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>AD681H</td>
      <td>Smartfren Andromax AD681H</td>
    </tr>
    <tr>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>FJL21</td>
      <td>FJL21</td>
    </tr>
    <tr>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>T31</td>
      <td>Panasonic T31</td>
    </tr>
    <tr>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>hws7721g</td>
      <td>MediaPad 7 Youth 2</td>
    </tr>
    <tr>
      <td>4</td>
      <td>3Q</td>
      <td>OC1020A</td>
      <td>OC1020A</td>
      <td>OC1020A</td>
    </tr>
  </tbody>
</table>



```python
# Q: if the usage patterns for users differ between different devices
result = pd.merge(left=user_usage, right=user_device, on='use_id')
result.head()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
      <td>12921</td>
      <td>android</td>
      <td>4.3</td>
      <td>GT-I9505</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
      <td>28714</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22789</td>
      <td>28714</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>94.46</td>
      <td>35.17</td>
      <td>519.12</td>
      <td>22790</td>
      <td>29592</td>
      <td>android</td>
      <td>5.1</td>
      <td>D2303</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22792</td>
      <td>28217</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G361F</td>
      <td>1</td>
    </tr>
  </tbody>
</table>



```python
print(user_usage.shape, user_device.shape, device_info.shape, result.shape)
```
    (240, 4) (272, 6) (14546, 4) (159, 9)
    


```python
user_usage['use_id'].isin(user_device['use_id']).value_counts()
```


    True     159
    False     81
    Name: use_id, dtype: int64



```python
result = pd.merge(left=user_usage, right=user_device, on='use_id', how='left')
print(user_usage.shape, result.shape, result['device'].isnull().sum())
```
    (240, 4) (240, 9) 81
    


```python
display(result.head(), result.tail())
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
      <td>12921.0</td>
      <td>android</td>
      <td>4.3</td>
      <td>GT-I9505</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22789</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>94.46</td>
      <td>35.17</td>
      <td>519.12</td>
      <td>22790</td>
      <td>29592.0</td>
      <td>android</td>
      <td>5.1</td>
      <td>D2303</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22792</td>
      <td>28217.0</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G361F</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>235</td>
      <td>260.66</td>
      <td>68.44</td>
      <td>896.96</td>
      <td>25008</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>236</td>
      <td>97.12</td>
      <td>36.50</td>
      <td>2815.00</td>
      <td>25040</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>237</td>
      <td>355.93</td>
      <td>12.37</td>
      <td>6828.09</td>
      <td>25046</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>238</td>
      <td>632.06</td>
      <td>120.46</td>
      <td>1453.16</td>
      <td>25058</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <td>239</td>
      <td>488.70</td>
      <td>906.92</td>
      <td>3089.85</td>
      <td>25220</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>



```python
result = pd.merge(left=user_usage, right=user_device, on='use_id', how='right')
print(user_device.shape, result.shape, result['device'].isnull().sum(), result['monthly_mb'].isnull().sum())
```
    (272, 6) (272, 9) 0 113
    


```python
display(result.head(), result.tail())
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
      <td>12921</td>
      <td>android</td>
      <td>4.3</td>
      <td>GT-I9505</td>
      <td>1</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
      <td>28714</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22789</td>
      <td>28714</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1</td>
    </tr>
    <tr>
      <td>3</td>
      <td>94.46</td>
      <td>35.17</td>
      <td>519.12</td>
      <td>22790</td>
      <td>29592</td>
      <td>android</td>
      <td>5.1</td>
      <td>D2303</td>
      <td>1</td>
    </tr>
    <tr>
      <td>4</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22792</td>
      <td>28217</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G361F</td>
      <td>1</td>
    </tr>
  </tbody>
</table>


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>267</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23047</td>
      <td>29720</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,1</td>
      <td>2</td>
    </tr>
    <tr>
      <td>268</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23048</td>
      <td>29724</td>
      <td>android</td>
      <td>6.0</td>
      <td>ONEPLUS A3003</td>
      <td>3</td>
    </tr>
    <tr>
      <td>269</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23050</td>
      <td>29726</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>270</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23051</td>
      <td>29726</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,2</td>
      <td>3</td>
    </tr>
    <tr>
      <td>271</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23052</td>
      <td>29727</td>
      <td>ios</td>
      <td>10.1</td>
      <td>iPhone8,4</td>
      <td>3</td>
    </tr>
  </tbody>
</table>



```python
print(user_usage['use_id'].unique().shape[0], user_device['use_id'].unique().shape[0], pd.concat([user_usage['use_id'], user_device['use_id']]).unique().shape[0])
```
    240 272 353
    


```python
result = pd.merge(left=user_usage, right=user_device, on='use_id', how='outer')
print(result.shape)
```
    (353, 9)
    


```python
print((result.apply(lambda x: x.isnull().sum(), axis=1) == 0).sum())
```
    159
    


```python
# Note that all rows from left and right merge dataframes are included, but NaNs will be in different columns depending if the data originated in the left or right dataframe.
result = pd.merge(left=user_usage, right=user_device, on='use_id', how='outer', indicator=True)
result.iloc[[0, 1, 200, 201, 350, 351]]
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
      <th>_merge</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
      <td>12921.0</td>
      <td>android</td>
      <td>4.3</td>
      <td>GT-I9505</td>
      <td>1.0</td>
      <td>both</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
      <td>both</td>
    </tr>
    <tr>
      <td>200</td>
      <td>28.79</td>
      <td>29.42</td>
      <td>3114.67</td>
      <td>23988</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <td>201</td>
      <td>616.56</td>
      <td>99.85</td>
      <td>5414.14</td>
      <td>24006</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>left_only</td>
    </tr>
    <tr>
      <td>350</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23050</td>
      <td>29726.0</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,2</td>
      <td>3.0</td>
      <td>right_only</td>
    </tr>
    <tr>
      <td>351</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>23051</td>
      <td>29726.0</td>
      <td>ios</td>
      <td>10.2</td>
      <td>iPhone7,2</td>
      <td>3.0</td>
      <td>right_only</td>
    </tr>
  </tbody>
</table>



```python
# For the question,
result1 = pd.merge(left=user_usage, right=user_device, on='use_id', how='left')
result1.head()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
      <td>12921.0</td>
      <td>android</td>
      <td>4.3</td>
      <td>GT-I9505</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22789</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>94.46</td>
      <td>35.17</td>
      <td>519.12</td>
      <td>22790</td>
      <td>29592.0</td>
      <td>android</td>
      <td>5.1</td>
      <td>D2303</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22792</td>
      <td>28217.0</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G361F</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>



```python
device_info.head()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Retail Branding</th>
      <th>Marketing Name</th>
      <th>Device</th>
      <th>Model</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>AD681H</td>
      <td>Smartfren Andromax AD681H</td>
    </tr>
    <tr>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>FJL21</td>
      <td>FJL21</td>
    </tr>
    <tr>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>T31</td>
      <td>Panasonic T31</td>
    </tr>
    <tr>
      <td>3</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>hws7721g</td>
      <td>MediaPad 7 Youth 2</td>
    </tr>
    <tr>
      <td>4</td>
      <td>3Q</td>
      <td>OC1020A</td>
      <td>OC1020A</td>
      <td>OC1020A</td>
    </tr>
  </tbody>
</table>



```python
result_final = pd.merge(left=result1, right=device_info[['Retail Branding', 'Marketing Name', 'Model']],
                        left_on='device', right_on='Model', how='left')
result_final[result_final['Retail Branding'] == 'Samsung'].head()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
      <th>Retail Branding</th>
      <th>Marketing Name</th>
      <th>Model</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>21.97</td>
      <td>4.82</td>
      <td>1557.33</td>
      <td>22787</td>
      <td>12921.0</td>
      <td>android</td>
      <td>4.3</td>
      <td>GT-I9505</td>
      <td>1.0</td>
      <td>Samsung</td>
      <td>Galaxy S4</td>
      <td>GT-I9505</td>
    </tr>
    <tr>
      <td>1</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22788</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
      <td>Samsung</td>
      <td>Galaxy S7</td>
      <td>SM-G930F</td>
    </tr>
    <tr>
      <td>2</td>
      <td>1710.08</td>
      <td>136.88</td>
      <td>7267.55</td>
      <td>22789</td>
      <td>28714.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>SM-G930F</td>
      <td>1.0</td>
      <td>Samsung</td>
      <td>Galaxy S7</td>
      <td>SM-G930F</td>
    </tr>
    <tr>
      <td>4</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22792</td>
      <td>28217.0</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G361F</td>
      <td>1.0</td>
      <td>Samsung</td>
      <td>Galaxy Core Prime</td>
      <td>SM-G361F</td>
    </tr>
    <tr>
      <td>5</td>
      <td>71.59</td>
      <td>79.26</td>
      <td>1557.33</td>
      <td>22793</td>
      <td>28217.0</td>
      <td>android</td>
      <td>5.1</td>
      <td>SM-G361F</td>
      <td>1.0</td>
      <td>Samsung</td>
      <td>Galaxy Core Prime</td>
      <td>SM-G361F</td>
    </tr>
  </tbody>
</table>



```python
result_final[result_final['Retail Branding'] == 'LGE'].head()
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform</th>
      <th>platform_version</th>
      <th>device</th>
      <th>use_type_id</th>
      <th>Retail Branding</th>
      <th>Marketing Name</th>
      <th>Model</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>78</td>
      <td>67.35</td>
      <td>15.38</td>
      <td>1557.33</td>
      <td>22892</td>
      <td>29671.0</td>
      <td>android</td>
      <td>7.1</td>
      <td>Nexus 5X</td>
      <td>1.0</td>
      <td>LGE</td>
      <td>Nexus 5X</td>
      <td>Nexus 5X</td>
    </tr>
    <tr>
      <td>95</td>
      <td>155.71</td>
      <td>10.14</td>
      <td>1557.33</td>
      <td>22922</td>
      <td>28845.0</td>
      <td>android</td>
      <td>6.0</td>
      <td>LG-H815</td>
      <td>1.0</td>
      <td>LGE</td>
      <td>LG G4</td>
      <td>LG-H815</td>
    </tr>
  </tbody>
</table>



```python
group1 = result_final[result_final['Retail Branding'] == 'Samsung']
group2 = result_final[result_final['Retail Branding'] == 'LGE']
display(group1.describe())
display(group2.describe())
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform_version</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.000000</td>
      <td>108.0</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>191.010093</td>
      <td>92.390463</td>
      <td>4017.318889</td>
      <td>22922.944444</td>
      <td>25208.361111</td>
      <td>5.453704</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>std</td>
      <td>237.723857</td>
      <td>82.747038</td>
      <td>5637.419003</td>
      <td>72.177848</td>
      <td>7359.871323</td>
      <td>0.664230</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>min</td>
      <td>8.140000</td>
      <td>0.790000</td>
      <td>0.000000</td>
      <td>22787.000000</td>
      <td>2873.000000</td>
      <td>4.100000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>69.137500</td>
      <td>27.630000</td>
      <td>1557.330000</td>
      <td>22871.000000</td>
      <td>26505.750000</td>
      <td>5.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>145.550000</td>
      <td>79.260000</td>
      <td>2076.450000</td>
      <td>22932.500000</td>
      <td>28953.000000</td>
      <td>6.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>239.125000</td>
      <td>127.970000</td>
      <td>3114.670000</td>
      <td>22980.500000</td>
      <td>29676.500000</td>
      <td>6.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>max</td>
      <td>1710.080000</td>
      <td>435.290000</td>
      <td>31146.670000</td>
      <td>23049.000000</td>
      <td>29725.000000</td>
      <td>6.000000</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>use_id</th>
      <th>user_id</th>
      <th>platform_version</th>
      <th>use_type_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>2.000000</td>
      <td>2.00000</td>
      <td>2.00</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>2.0</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>111.530000</td>
      <td>12.76000</td>
      <td>1557.33</td>
      <td>22907.000000</td>
      <td>29258.000000</td>
      <td>6.550000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>std</td>
      <td>62.479955</td>
      <td>3.70524</td>
      <td>0.00</td>
      <td>21.213203</td>
      <td>584.070201</td>
      <td>0.777817</td>
      <td>0.0</td>
    </tr>
    <tr>
      <td>min</td>
      <td>67.350000</td>
      <td>10.14000</td>
      <td>1557.33</td>
      <td>22892.000000</td>
      <td>28845.000000</td>
      <td>6.000000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>89.440000</td>
      <td>11.45000</td>
      <td>1557.33</td>
      <td>22899.500000</td>
      <td>29051.500000</td>
      <td>6.275000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>111.530000</td>
      <td>12.76000</td>
      <td>1557.33</td>
      <td>22907.000000</td>
      <td>29258.000000</td>
      <td>6.550000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>133.620000</td>
      <td>14.07000</td>
      <td>1557.33</td>
      <td>22914.500000</td>
      <td>29464.500000</td>
      <td>6.825000</td>
      <td>1.0</td>
    </tr>
    <tr>
      <td>max</td>
      <td>155.710000</td>
      <td>15.38000</td>
      <td>1557.33</td>
      <td>22922.000000</td>
      <td>29671.000000</td>
      <td>7.100000</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>



```python
result_final.groupby('Retail Branding').agg({'outgoing_mins_per_month':'mean',
                                             'outgoing_sms_per_month':'mean',
                                             'monthly_mb':'mean',
                                             'user_id':'count'})
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>outgoing_mins_per_month</th>
      <th>outgoing_sms_per_month</th>
      <th>monthly_mb</th>
      <th>user_id</th>
    </tr>
    <tr>
      <th>Retail Branding</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>HTC</td>
      <td>299.842955</td>
      <td>93.059318</td>
      <td>5144.077955</td>
      <td>44</td>
    </tr>
    <tr>
      <td>Huawei</td>
      <td>81.526667</td>
      <td>9.500000</td>
      <td>1561.226667</td>
      <td>3</td>
    </tr>
    <tr>
      <td>LGE</td>
      <td>111.530000</td>
      <td>12.760000</td>
      <td>1557.330000</td>
      <td>2</td>
    </tr>
    <tr>
      <td>Lava</td>
      <td>60.650000</td>
      <td>261.900000</td>
      <td>12458.670000</td>
      <td>2</td>
    </tr>
    <tr>
      <td>Lenovo</td>
      <td>215.920000</td>
      <td>12.930000</td>
      <td>1557.330000</td>
      <td>2</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>OnePlus</td>
      <td>354.855000</td>
      <td>48.330000</td>
      <td>6575.410000</td>
      <td>6</td>
    </tr>
    <tr>
      <td>Samsung</td>
      <td>191.010093</td>
      <td>92.390463</td>
      <td>4017.318889</td>
      <td>108</td>
    </tr>
    <tr>
      <td>Sony</td>
      <td>177.315625</td>
      <td>40.176250</td>
      <td>3212.000625</td>
      <td>16</td>
    </tr>
    <tr>
      <td>Vodafone</td>
      <td>42.750000</td>
      <td>46.830000</td>
      <td>5191.120000</td>
      <td>1</td>
    </tr>
    <tr>
      <td>ZTE</td>
      <td>42.750000</td>
      <td>46.830000</td>
      <td>5191.120000</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>11 rows × 4 columns</p>


# Basic Plotting Pandas DataFrames
(https://pandas.pydata.org/pandas-docs/stable/visualization.html)  
You’ll need to have the matplotlib plotting package installed to generate graphics, and  the "%matplotlib inline" notebook ‘magic’ activated for inline plots. 
You will also need "import matplotlib.pyplot as plt" to add figure labels and axis labels to your diagrams. 
A huge amount of functionality is provided by the .plot() command natively by Pandas. 


```python
# Plotting DataFrames
import matplotlib.pyplot as plt
raw_data['latitude'].plot(kind='hist', bins=100)
plt.xlabel('Latitude Value')
plt.show()
```

    <Figure size 640x480 with 1 Axes>


```python
raw_data.loc[raw_data['Element'] == 'Food']
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Area Abbreviation</th>
      <th>Area Code</th>
      <th>Area</th>
      <th>Item Code</th>
      <th>Item</th>
      <th>Element Code</th>
      <th>Element</th>
      <th>Unit</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>...</th>
      <th>Y2004</th>
      <th>Y2005</th>
      <th>Y2006</th>
      <th>Y2007</th>
      <th>Y2008</th>
      <th>Y2009</th>
      <th>Y2010</th>
      <th>Y2011</th>
      <th>Y2012</th>
      <th>Y2013</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2511</td>
      <td>Wheat and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>3249.0</td>
      <td>3486.0</td>
      <td>3704.0</td>
      <td>4164.0</td>
      <td>4252.0</td>
      <td>4538.0</td>
      <td>4605.0</td>
      <td>4711.0</td>
      <td>4810</td>
      <td>4895</td>
    </tr>
    <tr>
      <td>1</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2805</td>
      <td>Rice (Milled Equivalent)</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>419.0</td>
      <td>445.0</td>
      <td>546.0</td>
      <td>455.0</td>
      <td>490.0</td>
      <td>415.0</td>
      <td>442.0</td>
      <td>476.0</td>
      <td>425</td>
      <td>422</td>
    </tr>
    <tr>
      <td>3</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2513</td>
      <td>Barley and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>185.0</td>
      <td>43.0</td>
      <td>44.0</td>
      <td>48.0</td>
      <td>62.0</td>
      <td>55.0</td>
      <td>60.0</td>
      <td>72.0</td>
      <td>78</td>
      <td>89</td>
    </tr>
    <tr>
      <td>5</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2514</td>
      <td>Maize and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>231.0</td>
      <td>67.0</td>
      <td>82.0</td>
      <td>67.0</td>
      <td>69.0</td>
      <td>71.0</td>
      <td>82.0</td>
      <td>73.0</td>
      <td>77</td>
      <td>76</td>
    </tr>
    <tr>
      <td>6</td>
      <td>AF</td>
      <td>2</td>
      <td>Afghanistan</td>
      <td>2517</td>
      <td>Millet and products</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>33.94</td>
      <td>67.71</td>
      <td>...</td>
      <td>15.0</td>
      <td>21.0</td>
      <td>11.0</td>
      <td>19.0</td>
      <td>21.0</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>14.0</td>
      <td>14</td>
      <td>12</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>21470</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2949</td>
      <td>Eggs</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>18.0</td>
      <td>21.0</td>
      <td>22.0</td>
      <td>27.0</td>
      <td>27.0</td>
      <td>24.0</td>
      <td>24</td>
      <td>25</td>
    </tr>
    <tr>
      <td>21472</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2948</td>
      <td>Milk - Excluding Butter</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>373.0</td>
      <td>357.0</td>
      <td>359.0</td>
      <td>356.0</td>
      <td>341.0</td>
      <td>385.0</td>
      <td>418.0</td>
      <td>457.0</td>
      <td>426</td>
      <td>451</td>
    </tr>
    <tr>
      <td>21474</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2960</td>
      <td>Fish, Seafood</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>18.0</td>
      <td>14.0</td>
      <td>17.0</td>
      <td>14.0</td>
      <td>15.0</td>
      <td>18.0</td>
      <td>29.0</td>
      <td>40.0</td>
      <td>40</td>
      <td>40</td>
    </tr>
    <tr>
      <td>21475</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2961</td>
      <td>Aquatic Products, Other</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <td>21476</td>
      <td>ZW</td>
      <td>181</td>
      <td>Zimbabwe</td>
      <td>2928</td>
      <td>Miscellaneous</td>
      <td>5142</td>
      <td>Food</td>
      <td>1000 tonnes</td>
      <td>-19.02</td>
      <td>29.15</td>
      <td>...</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>17528 rows × 63 columns</p>



```python
raw_data_test = raw_data.loc[raw_data['Element'] == 'Food']
pd.DataFrame(raw_data_test.groupby('Area')['Y2013'].sum())
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Y2013</th>
    </tr>
    <tr>
      <th>Area</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Afghanistan</td>
      <td>21471</td>
    </tr>
    <tr>
      <td>Albania</td>
      <td>6952</td>
    </tr>
    <tr>
      <td>Algeria</td>
      <td>63455</td>
    </tr>
    <tr>
      <td>Angola</td>
      <td>30121</td>
    </tr>
    <tr>
      <td>Antigua and Barbuda</td>
      <td>119</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>Venezuela (Bolivarian Republic of)</td>
      <td>39706</td>
    </tr>
    <tr>
      <td>Viet Nam</td>
      <td>105399</td>
    </tr>
    <tr>
      <td>Yemen</td>
      <td>18325</td>
    </tr>
    <tr>
      <td>Zambia</td>
      <td>10180</td>
    </tr>
    <tr>
      <td>Zimbabwe</td>
      <td>9524</td>
    </tr>
  </tbody>
</table>
<p>174 rows × 1 columns</p>



```python
pd.DataFrame(raw_data_test.groupby('Area')['Y2013'].sum().sort_values(ascending=False))
```


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Y2013</th>
    </tr>
    <tr>
      <th>Area</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>China, mainland</td>
      <td>2499252</td>
    </tr>
    <tr>
      <td>India</td>
      <td>1238335</td>
    </tr>
    <tr>
      <td>United States of America</td>
      <td>641776</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>312488</td>
    </tr>
    <tr>
      <td>Russian Federation</td>
      <td>253892</td>
    </tr>
    <tr>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <td>Kiribati</td>
      <td>135</td>
    </tr>
    <tr>
      <td>Grenada</td>
      <td>122</td>
    </tr>
    <tr>
      <td>Antigua and Barbuda</td>
      <td>119</td>
    </tr>
    <tr>
      <td>Bermuda</td>
      <td>103</td>
    </tr>
    <tr>
      <td>Saint Kitts and Nevis</td>
      <td>56</td>
    </tr>
  </tbody>
</table>
<p>174 rows × 1 columns</p>

```python
pd.DataFrame(raw_data_test.groupby('Area')['Y2013'].sum().sort_values(ascending=False)[:10])
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Y2013</th>
    </tr>
    <tr>
      <th>Area</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>China, mainland</td>
      <td>2499252</td>
    </tr>
    <tr>
      <td>India</td>
      <td>1238335</td>
    </tr>
    <tr>
      <td>United States of America</td>
      <td>641776</td>
    </tr>
    <tr>
      <td>Brazil</td>
      <td>312488</td>
    </tr>
    <tr>
      <td>Russian Federation</td>
      <td>253892</td>
    </tr>
    <tr>
      <td>Indonesia</td>
      <td>237826</td>
    </tr>
    <tr>
      <td>Nigeria</td>
      <td>228877</td>
    </tr>
    <tr>
      <td>Pakistan</td>
      <td>180994</td>
    </tr>
    <tr>
      <td>Mexico</td>
      <td>166591</td>
    </tr>
    <tr>
      <td>Germany</td>
      <td>158473</td>
    </tr>
  </tbody>
</table>

```python
raw_data_test.groupby('Area')['Y2013'].sum().sort_values(ascending=False)[:10].plot(kind='bar')
plt.title('Top Ten Food Producers')
plt.ylabel('Food Produced (tonnes)')
```

```python
    Text(0, 0.5, 'Food Produced (tonnes)')
```

![png](Practice1_Tutorial_Pandas_KK_files/Practice1_Tutorial_Pandas_KK_129_1.png)
