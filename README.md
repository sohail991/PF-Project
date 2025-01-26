print("Running")
import pandas as pd
import pandas as pd

# Example: Creating a DataFrame
data = {'Column1': [1, 2, 3], 'Column2': [4, 5, 6]}
df = pd.DataFrame(data)

df =pd.read_csv("pcos_dataset.csv")
 # Data Cleaning
info how many column and row
df.info()
### We want to read all data
df = pd.DataFrame(df)
df.head()
### Check column to avoid error
column = list(df.columns)
column
### drop column that isnot needed
df.drop(columns='Country', inplace=True)  

### check n/a values
df.isna().sum()
### dropping n/a values and check again
df  =df.dropna(subset=["Country"])
df.isna().sum()
### Check duplicate
df.duplicated('Country').sum()
### Drop duplicate
df.drop_duplicates(subset='Country',inplace=True)
df.duplicated('Country').sum()
### Again duplicate
df.duplicated('Country').sum()
### Description
df.describe()
### If you want to seprate thing by commas
df['BMI'].str.split(',',expand=True)
### Data agregation
# Applying aggregation across all the columns 

numeric_df = df.select_dtypes(include=['number'])
numeric_df.aggregate(['sum', 'min'])
## We are going to find aggregation for these columns 
This function lets you perform several calculations at once, like getting the total, average, and maximum of a column in one go.

df.aggregate({"Age": ['sum', 'min'], 
              "Undiagnosed PCOS Likelihood": ['max', 'min'], 
             }) 
## Common Aggregations with groupby():
Think of it like sorting your data into groups (e.g., by city) and then calculating things like totals or averages for each group.
df.groupby('Ethnicity').agg({'Age': ['sum', 'mean', 'max', 'min']})

## Used to summarize and aggregate data with multiple dimensions.
It’s like an Excel pivot table. You can reorganize your data to show summaries by different Ethnicity and calculations.
df.pivot_table(values='Age', index='Ethnicity', aggfunc='sum')
## apply() – Custom Aggregation Function
Allows applying custom functions to each group.
Lets you write your own function to apply on each row or group of data.
df.groupby('Ethnicity')['Age'].apply(lambda x: x.max() - x.min())

## sum() – Add up values
Adds up all the numbers in a column.
df['Age'].sum()

## mean() – Calculating Average
Finds the average of a numerical column.
df['Age'].mean()
## count() – Counting Rows
Counts the number of non-null values.
df['Age'].count()
 ## median() – Median Value
Calculates the median (middle value) of a column.
df['Age'].median()
##  std() – Standard Deviation
Computes the standard deviation of a numerical column.
df['Age'].std()
## nunique() – Counting Unique Values
Finds the number of unique values in a column.
df['Ethnicity'].nunique()
## data preprocessing
## Handling Missing Values
Yes() / No() – Check for missing values
df.isnull().sum() 
### For non missing values
df.isnull().sum() 
dropna() – Remove missing values
Removes rows or columns with missing values.
Remove rows with any missing value
df.dropna()
### Remove columns with missing values
df.dropna(axis=1)
fillna() – Fill missing values
Fills missing values with a specified value or strategy.
df.fillna(0)  # Replace missing values with 0

## Handling Duplicates
duplicated() – Identify duplicate rows
Checks for duplicate rows in the DataFrame.
df.duplicated().sum()  # Count duplicate rows
df.drop_duplicates()  # Remove duplicate rows

## Data Transformation
astype() – Convert data types
Converts the data type of columns to a specific type.
# Adding the 'Gender' column with sample data
gender_list = ['Female', 'Male', 'Male', 'Female', 'Female', 'Male', 'Female', 'Male', 'Female', 'Male'] * (len(df) // 10 + 1)
df['Gender'] = gender_list[:len(df)]  # Adjust the length to match the DataFrame

# Convert categorical values to numeric
df['Gender'] = df['Gender'].map({'Male': 1, 'Female': 0})
## rename() – Rename columns
Renames column names for better understanding.
df.rename(columns={'Name': 'name'}, inplace=True)

df.rename(columns={'Customer Name': 'name'}, inplace=True)

## Data Encoding
get_dummies() – One-hot encoding
Converts categorical data into dummy variables.
# Adding a 'Country' column with sample data
df['Country'] = ['USA', 'Canada', 'Mexico']

# Applying one-hot encoding to the 'Country' column
pd.get_dummies(df, columns=['Country'])
 ## factorize() – Convert categorical values to numeric codes
Encodes labels into numeric values.
# Adding a 'Category' column with sample data
df['Category'] = ['A', 'B', 'C']

# Factorizing the 'Category' column
df['Category'], unique_values = pd.factorize(df['Category'])

## converting data type
df['Sales'] = df['Sales'].astype(float)
print(df.dtypes)
## Data plotting
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df.groupby('Country')['Sales'].sum().plot(kind='bar', title='Total  Country')
plt.ylabel('Total country')
plt.show()
df['Category'].plot(kind='hist', bins=5, title='Country Distribution')
plt.xlabel('country')
plt.show()

df['Category'].value_counts().plot(kind='pie', autopct='%1.1f%%', title='Country Category Distribution')
plt.ylabel('')  # Remove the default y-label
plt.show()

# Adding a 'City' column with sample data
df['City'] = ['New York', 'Toronto', 'Mexico City']

# Plotting the 'City' column
df['City'].value_counts().plot(kind="bar")
plt.title('City Distribution')
plt.xlabel('City')
plt.ylabel('Count')
plt.show()
df['City'].value_counts().plot(kind="area")
df['City'].value_counts().plot(kind="hist")
# Convert categorical 'City' column to numerical data
city_numeric = df['City'].factorize()[0]

# Plot KDE for the numerical data
sns.kdeplot(city_numeric)
plt.title('City Distribution (KDE)')
plt.xlabel('City')
plt.show()
df['Country'].value_counts().plot(kind="box")
df['City'].value_counts().plot(kind="area")
## saving data
# Ensure the file is not open in any other program and you have the necessary permissions
try:
	df = pd.read_csv("data.csv")
	df
	df.to_excel("newdata.xlsx", sheet_name="newsheet")
except PermissionError as e:
	print(f"PermissionError: {e}")
