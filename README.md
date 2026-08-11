Student Data Cleaning Pipeline

An end-to-end data cleaning pipeline built in Python (Pandas) to transform a deliberately messy, real-world-style student records dataset into an analysis-ready format — handling inconsistent strings, malformed numbers/dates, invalid contact info, nested JSON, and free-text addresses.

Problem

The raw dataset (super_dirty_students.csv, 1,000 rows / 20+ columns) simulated common real-world data quality issues:

Inconsistent string formatting (extra whitespace, mixed case, placeholder nulls like "NaN", "none", "null")
Numeric fields stored as text with symbols, commas, and stray characters (age, GPA, score, attendance, money spent)
Mixed date formats, including Unix timestamps stored as strings
Invalid or inconsistently formatted emails and phone numbers
Nested JSON stored as text in a single column (skills, hobbies, family income)
Unstructured free-text addresses containing city, district, and postal code
Duplicate records and inconsistent category labels (e.g., gender entered as "m", "Male", "мужской", "erkak")
Approach

The pipeline was built in stages, each isolating one class of data quality issue:

String cleaning — trimmed whitespace, standardized placeholder nulls to NaN across all text columns
Numeric & date parsing — stripped non-numeric characters and converted to proper numeric/datetime types, including handling mixed date and Unix-timestamp formats
Contact validation — validated email format with regex; parsed, reformatted, and mapped phone numbers to mobile operator using Uzbek numbering rules
JSON parsing — safely parsed nested JSON/dict-like text into structured columns (technical skills, soft skills, family income)
Address parsing — extracted postal code, city, and district from unstructured free-text addresses using pattern matching and reference lists
Deduplication — identified and removed exact and email-based duplicate records
Missing value handling — imputed numeric fields with median/mean and categorical fields with mode, based on column type
Category normalization — mapped inconsistent spellings/languages (English, Russian, Uzbek) for gender, course, and status into standardized categories
Type finalization & export — enforced final data types and exported the cleaned dataset to CSV
Automated QA checks — built a reusable function to compare original vs. cleaned row counts, check remaining missing values, and flag numeric values outside expected ranges (e.g., GPA outside 0–4.0, attendance outside 0–100%)
Results
Reduced dataset from 1,000 to 568 rows after removing 432 duplicate/invalid records
Eliminated all missing values in email and phone fields
Standardized gender, course, and status fields from 10+ inconsistent raw variants down to clean, consistent categories
Built a reusable QA function that automatically flags data quality issues (e.g., out-of-range GPA/attendance values) for review before downstream use
Known Limitations

The QA step currently flags out-of-range values (e.g., GPA outside 0–4.0, attendance outside 0–100%) rather than correcting them — this is intentional, so quality issues are surfaced for review rather than silently altered. A future iteration would add explicit correction or removal logic for these flagged rows.

Tools

Python, Pandas, NumPy, regex (re), json / ast for nested data parsing

Files
notebooks/project4.ipynb — full cleaning pipeline
data/super_dirty_students.csv — raw input data
data/super_dirty_students_cleaned.csv — cleaned output# student-data-cleaning-pipeline
