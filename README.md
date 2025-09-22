# Medical Appointment No-Shows Dataset – Data Cleaning and Preprocessing

This repository contains the **Medical Appointment No-Shows Dataset** from Kaggle, along with the Python and Excel scripts used for **data cleaning and preprocessing**.

---

## Dataset

- Original dataset: [Kaggle – Medical Appointment No-Shows](https://www.kaggle.com/datasets/joniarroba/noshowappointments)  
- Renamed dataset for this project: `task1.csv` (CSV format) and `task1.xlsx` (Excel format)

---

### Dataset Summary

- **Number of rows:** ~110,000 (appointments)  
- **Number of columns:** 14  
- **Key columns:**
  - `patient_id` – Unique identifier for each patient  
  - `gender` – Patient gender (`Male` / `Female`)  
  - `scheduled_date` – Date when the appointment was scheduled  
  - `scheduled_time` – Time when the appointment was scheduled  
  - `appointment_date` – Date of the appointment  
  - `age` – Age of the patient  
  - `no_show` – Indicates if the patient showed up (`Yes` / `No`)  
  - Other columns: `neighbourhood`, `hipertension`, `diabetes`, `alcoholism`, `handicap`, `sms_received`  

- **Purpose:**  
  The dataset tracks whether patients showed up for their scheduled medical appointments and includes demographic and medical attributes to analyze trends and predict no-shows.  

---

## Data Cleaning and Preprocessing

Data cleaning was performed using **two methods**:

**Steps performed:**
### 1. Microsoft Excel
1. **Convert CSV to XLSX** to use Excel tools.  
2. **Review & Identify Issues:**  
   - Mixed and spaced column names  
   - Gender represented as `M` and `F`  
   - `Scheduled Day` and `Appointment Day` included both date and time  
   - Invalid age value `-1`  
3. **Handle Missing Values:** Applied filters to detect missing values.  
4. **Remove Duplicates:** Used the **Remove Duplicates** tool.  
5. **Standardize Column Names:** Converted to lowercase and replaced spaces with underscores.  
   Example:  
Patient ID → patient_id
Appointment Date → appointment_date
6. **Format Date & Time Columns:**  
- Split date and time using **Text-to-Columns** tool.  
- Reformatted date from `YYYY-MM-DD` → `DD-MM-YYYY`.  
- Deleted unnecessary `appointment_time` column.  
7. **Correct Age Values:** Replaced invalid `-1` with `1`.  
8. **Update Gender Values:** Replaced `F` → `Female` and `M` → `Male`.  

---

### 2. Python (Jupyter Notebook)

**Steps performed:**

1. **Load Dataset:**
```python
import pandas as pd
df = pd.read_csv("task1.csv")
2. **Handling missing values:**
print("Missing values:\n", df.isnull().sum())
3. **Remove Duplicates:**
df.drop_duplicates(inplace=True)
4. **Standardize Text Columns:**
for col in df.select_dtypes(include='object').columns:
    df[col] = df[col].str.strip().str.lower()

df['gender'] = df['gender'].str.strip().str.upper().map({'F': 'female', 'M': 'male'})
5. **Format Date & Time Columns:**
if 'scheduledday' in df.columns:
    df['scheduledday'] = pd.to_datetime(df['scheduledday'], errors='coerce')
    df['scheduled_date'] = df['scheduledday'].dt.strftime('%d-%m-%Y')
    df['scheduled_time'] = df['scheduledday'].dt.strftime('%H:%M:%S')
    df.drop(columns=['scheduledday'], inplace=True)

if 'appointmentday' in df.columns:
    df['appointmentday'] = pd.to_datetime(df['appointmentday'], errors='coerce')
    df['appointment_date'] = df['appointmentday'].dt.strftime('%d-%m-%Y')
    df['appointment_time'] = df['appointmentday'].dt.strftime('%H:%M:%S')
    df.drop(columns=['appointmentday'], inplace=True)
6. **Rename Columns and Drop Unnecessary Columns:**
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')
df.drop(columns=['appointment_time'], inplace=True)
7. **Fix Data Types:**
if 'age' in df.columns:
    df['age'] = df['age'].astype(int, errors='ignore')
8. **Export Cleaned Data:**
df.to_csv("cleaned_data.csv", index=False)
print("Data cleaning completed! Cleaned file saved as 'cleaned_data.csv'")
```
---

### Summary

> - Excel was used for **manual/visual cleaning**.
> - Python was used for **automated and reproducible cleaning**, including handling missing values, duplicates, text standardization, date-time formatting, and exporting the cleaned dataset.
> - The final cleaned dataset is saved as `cleaned_data.csv` ready for analysis.
