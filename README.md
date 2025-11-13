🚗 Vehicle Compliance Analysis & Real-Time Confidence Plotting

This project processes automatic number plate recognition (ANPR) output data, evaluates compliance status, identifies fake or invalid plates, and visualizes confidence scores with both static and real-time plotting.

1️⃣ Data Validation & Compliance Classification

It checks each record in vehicle_data.csv and determines:

Plate format validity using a regular expression

Fake plates using a predefined list

Registration, insurance, and pollution compliance

Generates:

status → COMPLIANT, NON-COMPLIANT, INVALID FORMAT, FAKE PLATE

final_flag → Green Flag or Reportable

2️⃣ Visualizations
✔ Histogram with KDE

A static histogram (histogram_confidence.png) of confidence scores.

✔ Simulated Real-Time Confidence Plot

A live-updating distribution plot (live_plot.png) that simulates incoming scores.

3️⃣ Final Compliance Report

A cleaned table summarizing plate details and compliance status.

Output saved to:

vehicle_report.csv

📁 Input Data Requirements

The script expects a CSV file named vehicle_data.csv with the following columns:

Column Name	Type	Description
plate_number	string	ANPR-detected number plate
confidence_score	float	Model confidence (0–100)
registration_valid	bool	Registration validity
insurance_valid	bool	Insurance validity
pollution_valid	bool	Pollution certificate validity
🧪 Plate Validation Logic
Fake plate detection
FAKE_PLATES = {"MH12FAKE1234", "DL8CAF9999"}

Format validation

Accepts formats like:

TN22AB1234
KA05XY4321
UP32ZZ9999


Regex used:

PLATE_REGEX = r'^[A-Z]{2}\d{2}[A-Z]{1,2}\d{4}$'

📊 Generated Outputs
File	Description
histogram_confidence.png	Static histogram of confidence scores
live_plot.png	Simulated real-time confidence visualization
vehicle_report.csv	Final vehicle compliance summary
Console Table	Readable Markdown-formatted report
▶️ How to Run

Ensure required Python packages are installed:

pip install pandas matplotlib seaborn


Place your vehicle_data.csv in the same directory.

Then run:

python your_script.py

🛠 Key Features in Code
Compute Compliance Status
def compute_status(row):
    if is_fake_plate(row["plate_number"]): return "FAKE PLATE"
    if not is_plate_format_valid(row["plate_number"]): return "INVALID FORMAT"
    if not (row["registration_valid"] and row["insurance_valid"] and row["pollution_valid"]):
        return "NON-COMPLIANT"
    return "COMPLIANT"

Real-Time Plotting Loop

Simulates incoming detection updates:

for score in df["confidence_score"]:
    conf_scores.append(score)
    ax.clear()
    sns.histplot(conf_scores, bins=5, kde=True, ...)

📌 Sample Output Table
| plate_number | plate_format_valid | confidence_score | registration_valid | insurance_valid | pollution_valid | status        | final_flag |
|--------------|--------------------|------------------|--------------------|-----------------|-----------------|---------------|------------|
| TN22AB1234   | True               | 95               | True               | True            | True            | COMPLIANT     | Green Flag |
| MH12FAKE1234 | False              | 90               | True               | True            | True            | FAKE PLATE    | Reportable |
...

🎯 Purpose

This tool is ideal for:

ANPR system validation

Compliance checking for traffic enforcement

Real-time monitoring dashboards

Model confidence evaluation
