# 💊 Medicine Inventory Management System
This project is designed to model a **Medicine Management Management System** using an Entity-Relationship approach. It captures the relationships between doctors, patients, prescriptions, medicine, bills, and diseases.

## Entities 

### 1. Prescription
Stores information about each issued prescription.

- `Prescription_ID` (PK)
- `Issuing_Doctor` (FK → Doctor)
- `Issue_Date`
- `Patient_Name` (FK → Patient)
- `Medicines` (Multiple entries is associated with`Quantity`)

### 2. Medicine
States the medicines available in the system.

- `Medicine_ID` (PK)
- `Medicine_Name`
- `Manufacture_Date`
- `Expiry_Date`
- `Price`
- `Dosage`
- `Used_For` (FK → Disease)

### 3. Bill
States the billing associated with each prescription.

- `Bill_ID` (PK)
- `Related_Prescription` (FK → Prescription)
- `Tax`
- `Discount`
- `Total`

### 4. Disease
Stores data about diseases a medicine is used for.

- `Disease_ID` (PK)
- `Disease_Name`
- `Disease_Type`  
  *(Types: Infectious, Deficiency, Genetic Hereditary, Non-Genetic Hereditary)*

### 5. Doctor
Contains details about medical professionals.

- `Doctor_ID` (PK)
- `Doctor_Name`
- `Qualification`
- `Specialization`


## Database Design Features
- Normalized Database Design with clear entity relationships.

- Junction Tables for many-to-many mappings (e.g: medicine-disease, prescription-medicine).

- Trigger for dynamic billing updates upon new prescription entries.

- Sample Data for doctors, diseases, medicines, prescriptions, and bills.

## How to run the code
1. Install MYSQL.
2. Run the SQL script from the file or paste from the report.
3. Make sure the tables are created, records are inserted, triggers are set and then you can run the code.
### Sample queries
- Get all doctors specializing in Cardiology
 
`SELECT doctor_name FROM Doctor WHERE specialization = 'Cardiology';`

- List of deficiency diseases

`SELECT disease_name FROM Disease WHERE disease_type = 'deficiency';`

- Most sold medicine in 2025

`SELECT m.medicine_name, SUM(pm.quantity) AS total_sold
FROM Prescription_Medicine pm
JOIN Prescription p ON pm.prescription_id = p.prescription_id
JOIN Medicine m ON pm.medicine_id = m.medicine_id
WHERE YEAR(p.issue_date) = 2025
GROUP BY m.medicine_id, m.medicine_name
ORDER BY total_sold DESC
LIMIT 1;`
# 👥 Contributors
- Yasmine Khaled - 231000809
- Hana Hikal - 231000300
