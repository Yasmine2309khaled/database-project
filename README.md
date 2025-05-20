# 💊 Medicine Inventory Management System
This project is designed to model a **Medicine Management Management System** using an Entity-Relationship approach. It captures the relationships between doctors, patients, prescriptions, medicine, bills, and diseases.
## Repository Structure

 
├─ [database report.docx](https://github.com/user-attachments/files/20353806/database.report.docx) /report.docx        ← Your written report
 
  ├─ /presentation.pptx  ← Your slides

  ├─ /video_link.txt     ← Public YouTube/MS Stream URL

  /sql/

*create_tables.sql    ← DDL (Table definitions)*
```CREATE DATABASE IF NOT EXISTS medicine_inventory_management_system;
USE medicine_inventory_management_system;

CREATE TABLE IF NOT EXISTS Disease (
  disease_id INT PRIMARY KEY,
  disease_name VARCHAR(100),
  disease_type ENUM('infectious', 'deficiency', 'genetic hereditary', 'non-genetic hereditary')
);

CREATE TABLE IF NOT EXISTS Doctor (
  doctor_id INT PRIMARY KEY,
  doctor_name VARCHAR(100),
  qualification VARCHAR(100),
  specialization VARCHAR(100)
);

CREATE TABLE IF NOT EXISTS Medicine (
  medicine_id INT PRIMARY KEY,
  medicine_name VARCHAR(100),
  manufacture_date DATE,
  expiry_date DATE,
  price DECIMAL(10,2),
  dosage VARCHAR(100)
);

CREATE TABLE IF NOT EXISTS Medicine_Disease (
  medicine_id INT,
  disease_id INT,
  PRIMARY KEY (medicine_id, disease_id),
  FOREIGN KEY (medicine_id) REFERENCES Medicine(medicine_id),
  FOREIGN KEY (disease_id) REFERENCES Disease(disease_id)
);

CREATE TABLE IF NOT EXISTS Prescription (
  prescription_id INT PRIMARY KEY,
  doctor_id INT,
  patient_name VARCHAR(100),
  issue_date DATE,
  FOREIGN KEY (doctor_id) REFERENCES Doctor(doctor_id)
);

CREATE TABLE IF NOT EXISTS Prescription_Medicine (
  prescription_id INT,
  medicine_id INT,
  quantity INT,
  PRIMARY KEY (prescription_id, medicine_id),
  FOREIGN KEY (prescription_id) REFERENCES Prescription(prescription_id) ON DELETE CASCADE,
  FOREIGN KEY (medicine_id) REFERENCES Medicine(medicine_id)
);

CREATE TABLE IF NOT EXISTS Bill (
  bill_id INT PRIMARY KEY,
  prescription_id INT UNIQUE,
  tax DECIMAL(10,2),
  discount DECIMAL(10,2),
  total DECIMAL(10,2),
  FOREIGN KEY (prescription_id) REFERENCES Prescription(prescription_id) ON DELETE CASCADE
);
```
*load_data.sql        ← INSERT scripts (min 10 records/table)*
```INSERT INTO Doctor (doctor_id, doctor_name, qualification, specialization) VALUES 
(1, 'Dr. Ahmed Hisham', 'MBBS, MD', 'Cardiology'),
(2, 'Dr. Mariam El-Sayed', 'MBBS, PhD', 'Endocrinology'),
(3, 'Dr. Youssef Nabil', 'MBBS', 'General Medicine'),
(4, 'Dr. Salma Khaled', 'MBBS, MD', 'Pediatrics'),
(5, 'Dr. Hani Saad', 'MBBS, MS', 'Neurology'),
(6, 'Dr. Dalia Mostafa', 'MBBS, MD', 'Dermatology'),
(7, 'Dr. Nader Zaki', 'MBBS', 'Internal Medicine'),
(8, 'Dr. Rana Kamal', 'MBBS', 'Oncology'),
(9, 'Dr. Tarek El-Amin', 'MBBS, MD', 'Orthopedics'),
(10, 'Dr. Laila Samir', 'MBBS', 'Hematology');

INSERT INTO Disease (disease_id, disease_name, disease_type) VALUES 
(1, 'Anemia', 'deficiency'),
(2, 'Diabetes', 'non-genetic hereditary'),
(3, 'COVID-19', 'infectious'),
(4, 'Thalassemia', 'genetic hereditary'),
(5, 'Vitamin D Deficiency', 'deficiency'),
(6, 'Hepatitis B', 'infectious'),
(7, 'Cystic Fibrosis', 'genetic hereditary'),
(8, 'Tuberculosis', 'infectious'),
(9, 'Hemophilia', 'genetic hereditary'),
(10, 'Night Blindness', 'deficiency');

INSERT INTO Medicine (medicine_id, medicine_name, manufacture_date, expiry_date, price, dosage) VALUES 
(101, 'Panadol', '2023-01-10', '2026-01-10', 25.50, '500mg twice a day'),
(102, 'Cetamol', '2022-11-05', '2025-11-05', 20.00, '2 tablets per day'),
(103, 'Insulatard', '2023-05-15', '2025-05-15', 150.00, '1 injection daily'),
(104, 'Ferroglobin', '2023-07-01', '2026-07-01', 45.00, '1 capsule daily'),
(105, 'Neuroton', '2024-02-01', '2026-02-01', 60.00, '1 tablet twice a day'),
(106, 'Calcimax', '2023-09-12', '2025-09-12', 35.00, '2 tablets daily'),
(107, 'Dafalgan', '2022-12-20', '2025-12-20', 18.00, '500mg once a day'),
(108, 'Zithromax', '2023-03-25', '2026-03-25', 80.00, '500mg for 3 days'),
(109, 'Vitamin D3', '2023-10-01', '2026-10-01', 40.00, '1 drop daily'),
(110, 'Hemofer', '2023-06-14', '2025-06-14', 50.00, '1 capsule daily');

INSERT INTO Medicine_Disease (medicine_id, disease_id) VALUES 
(101, 3), (102, 3), (103, 2), (104, 1), (105, 7), (106, 5), 
(107, 6), (108, 6), (109, 10), (110, 1);

INSERT INTO Prescription (prescription_id, doctor_id, patient_name, issue_date) VALUES 
(1001, 1, 'Omar Mahmoud', '2024-12-20'),
(1002, 2, 'Layla Ahmed', '2025-01-05'),
(1003, 3, 'Hassan Farid', '2025-03-15'),
(1004, 4, 'Nour Samy', '2025-02-11'),
(1005, 5, 'Khaled Mostafa', '2025-01-25'),
(1006, 6, 'Yasmin Fathy', '2025-04-10'),
(1007, 7, 'Ehab El-Gendy', '2025-05-01'),
(1008, 8, 'Sara Magdy', '2025-04-22'),
(1009, 9, 'Mohamed Salah', '2025-05-10'),
(1010, 10, 'Alya Tarek', '2025-05-15');

INSERT INTO Prescription_Medicine (prescription_id, medicine_id, quantity) VALUES 
(1001, 101, 2), (1001, 102, 1), (1002, 103, 1), (1003, 104, 3), 
(1004, 106, 2), (1005, 105, 2), (1006, 109, 1), (1007, 107, 2), 
(1008, 108, 1), (1009, 110, 2), (1010, 104, 2);

INSERT INTO Bill (bill_id, prescription_id, tax, discount, total) VALUES 
(1001, 1001, 4.58, 2.29, 48.09), (1002, 1002, 15.00, 7.50, 157.50),
(1003, 1003, 13.50, 6.75, 141.75), (1004, 1004, 7.00, 3.50, 73.50),
(1005, 1005, 12.00, 6.00, 126.00), (1006, 1006, 4.00, 2.00, 42.00),
(1007, 1007, 3.60, 1.80, 37.80), (1008, 1008, 8.00, 4.00, 84.00),
(1009, 1009, 10.00, 5.00, 105.00), (1010, 1010, 9.00, 4.50, 94.50);
```

 
*queries.sql          ← Sample SELECT, UPDATE, DELETE queries*
```SELECT doctor_name FROM Doctor WHERE specialization = 'Cardiology';
SELECT disease_name FROM Disease WHERE disease_type = 'deficiency';
SELECT m.medicine_name
FROM Medicine m
JOIN Medicine_Disease md ON m.medicine_id = md.medicine_id
JOIN Disease d ON d.disease_id = md.disease_id
WHERE d.disease_name = 'Anemia';


UPDATE Doctor SET qualification = 'MBBS, MD, PhD' WHERE doctor_id = 2;


DELETE FROM Prescription_Medicine WHERE prescription_id = 1001 AND medicine_id = 101;
```

*triggers.sql         ← AFTER INSERT trigger for billing*
```DELIMITER $$

CREATE TRIGGER updated_bill
AFTER INSERT ON Prescription_Medicine
FOR EACH ROW
BEGIN
  DECLARE total_price DECIMAL(10,2);
  DECLARE tax_amt DECIMAL(10,2);
  DECLARE discount_amt DECIMAL(10,2);

  SELECT SUM(pm.quantity * m.price)
  INTO total_price
  FROM Prescription_Medicine pm
  JOIN Medicine m ON pm.medicine_id = m.medicine_id
  WHERE pm.prescription_id = NEW.prescription_id;

  IF total_price IS NULL THEN
    SET total_price = 0;
  END IF;

  SET tax_amt = total_price * 0.10;
  SET discount_amt = total_price * 0.05;

  IF EXISTS (SELECT 1 FROM Bill WHERE prescription_id = NEW.prescription_id) THEN
    UPDATE Bill
    SET tax = tax_amt,
        discount = discount_amt,
        total = total_price + tax_amt - discount_amt
    WHERE prescription_id = NEW.prescription_id;
  ELSE
    INSERT INTO Bill (bill_id, prescription_id, tax, discount, total)
    VALUES (
      NEW.prescription_id,
      NEW.prescription_id,
      tax_amt,
      discount_amt,
      total_price + tax_amt - discount_amt
    );
  END IF;
END$$

DELIMITER ;
```


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
3. Run `create_tables.sql` to create schema
4. Run `load_data.sql` to add data
5. Run `queries.sql` to test queries
6. Run `triggers.sql` to test the trigger
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
