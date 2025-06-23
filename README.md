# Task1
Elevate Labs-Task1: Data Cleaning and Preprocessing

#Code when using Python(Pandas)---->
import pandas as pd

# Step 1: Load the dataset
df = pd.read_csv("data/marketing_campaign.csv", sep="\t")

# Step 2: Fill missing values
numeric_cols = df.select_dtypes(include=['float64', 'int64']).columns
categorical_cols = df.select_dtypes(include=['object']).columns

# Fill numeric columns with mean
for col in numeric_cols:
    df[col] = df[col].fillna(df[col].mean())

# Fill categorical columns with mode
for col in categorical_cols:
    df[col] = df[col].fillna(df[col].mode()[0])

# Step 3: Remove duplicate records
df = df.drop_duplicates()

# Step 4: Standardize text values (lowercase and trimmed)
# First, check actual column names
df.columns = df.columns.str.strip()
text_columns = [col for col in df.columns if df[col].dtype == 'object']
for col in text_columns:
    df[col] = df[col].str.lower().str.strip()

# Step 5: Convert Dt_Customer to dd-mm-yyyy format
df['Dt_Customer'] = pd.to_datetime(df['Dt_Customer'], errors='coerce')
df['Dt_Customer'] = df['Dt_Customer'].dt.strftime('%d-%m-%Y')

df['Dt_Customer'] = df['Dt_Customer'].fillna(df['Dt_Customer'].mode()[0])


# Step 6: Rename column headers to lowercase with underscores
df.columns = df.columns.str.lower().str.replace(" ", "_")

# Step 7: Ensure correct data types
if 'age' in df.columns:
    df['age'] = pd.to_numeric(df['age'], errors='coerce').fillna(0).astype(int)
if 'dt_customer' in df.columns:
    df['dt_customer'] = pd.to_datetime(df['dt_customer'], format='%d-%m-%Y', errors='coerce')

# Save the cleaned dataset
df.to_csv("marketing_campaign_fully_cleaned1.csv", index=False)
print("✅ Data cleaning and preprocessing completed.")
