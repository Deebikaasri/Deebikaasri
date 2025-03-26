import pandas as pd

# Load the dataset
file_path = r"C:\Users\kamal\Downloads\loan.csv"
df = pd.read_csv(file_path)

# Clean column names
df.columns = df.columns.str.strip()

# Remove rows with missing income values
df = df.dropna(subset=["income_annum", "loan_status"])

# Convert income_annum to numeric if necessary
df["income_annum"] = pd.to_numeric(df["income_annum"], errors='coerce')

# Check unique values in loan_status
print("Unique loan_status values:", df["loan_status"].unique())

# Define income threshold for approval analysis
threshold = df["income_annum"].median()
df["income_category"] = df["income_annum"].apply(lambda x: "High" if x >= threshold else "Low")

# Ensure loan_status has the correct values
loan_status_values = df["loan_status"].unique()

# Calculate approval rate based on income category
approval_rate = df.groupby(["income_category", "loan_status"]).size().unstack().fillna(0)

# Check correct column names
approved_col = next((col for col in approval_rate.columns if "approved" in col.lower()), None)
rejected_col = next((col for col in approval_rate.columns if "rejected" in col.lower()), None)

if approved_col and rejected_col:
    approval_rate["Approval_Rate"] = approval_rate[approved_col] / (approval_rate[approved_col] + approval_rate[rejected_col])
else:
    print("Error: Approved or Rejected column not found in loan_status")
print(approval_rate)

