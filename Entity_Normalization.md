# Entity Normalization Process
## MedFirst Diagnostic Center Database

---

## Initial Unnormalized Entity List

### PATIENT_VISIT (Unnormalized)
```
Patient_ID, Patient_Name, Patient_DOB, Patient_Phone, Patient_Address, 
Patient_Email, Insurance_Company, Insurance_Number, 
Visit_Date, Visit_Time, Appointment_Type,
Doctor_ID, Doctor_Name, Doctor_Specialty, Doctor_Phone,
Nurse_ID, Nurse_Name,
Assessment_Notes, Vital_Signs, Chief_Complaint, Diagnosis,
Test1_Name, Test1_Date, Test1_Result, Test1_Technician,
Test2_Name, Test2_Date, Test2_Result, Test2_Technician,
Referral_To, Referral_Reason, Referral_Date
```

**Problems:**
- Repeating groups (multiple tests)
- Transitive dependencies
- Partial dependencies
- Data redundancy

---

## First Normal Form (1NF)
*Eliminate repeating groups*

### PATIENT_VISIT (1NF)
```
Patient_ID, Patient_Name, Patient_DOB, Patient_Phone, Patient_Address,
Patient_Email, Insurance_Company, Insurance_Number,
Visit_ID, Visit_Date, Visit_Time, Appointment_Type,
Doctor_ID, Doctor_Name, Doctor_Specialty, Doctor_Phone,
Nurse_ID, Nurse_Name,
Assessment_Notes, Vital_Signs, Chief_Complaint, Diagnosis,
Referral_To, Referral_Reason, Referral_Date
```

### DIAGNOSTIC_TEST (1NF) - *Separated repeating group*
```
Test_ID, Visit_ID, Test_Name, Test_Date, Test_Result, 
Technician_ID, Technician_Name
```

---

## Second Normal Form (2NF)
*Remove partial dependencies - all non-key attributes must depend on entire primary key*

### PATIENTS (2NF)
```
Patient_ID (PK), First_Name, Last_Name, DOB, Phone, 
Street_Address, City, State, Zip, Email, 
Insurance_Company, Insurance_Number
```

### VISITS (2NF)
```
Visit_ID (PK), Patient_ID (FK), Visit_Date, Visit_Time, 
Appointment_Type, Doctor_ID (FK), Nurse_ID (FK)
```

### STAFF (2NF)
```
Staff_ID (PK), First_Name, Last_Name, Role, Specialty, 
Phone, Email, License_Number
```

### ASSESSMENTS (2NF)
```
Assessment_ID (PK), Visit_ID (FK), Chief_Complaint, 
Vital_Signs, Assessment_Notes, Diagnosis, 
Referral_To, Referral_Reason
```

### DIAGNOSTIC_TESTS (2NF)
```
Test_ID (PK), Visit_ID (FK), Test_Name, Test_Date, 
Technician_ID (FK)
```

### TEST_RESULTS (2NF)
```
Result_ID (PK), Test_ID (FK), Result_Value, Result_Date, 
Notes, Is_Abnormal
```

---

## Third Normal Form (3NF)
*Remove transitive dependencies - non-key attributes should not depend on other non-key attributes*

### Final Normalized Entities with Data Types

### 1. PATIENTS
| Attribute | Data Type | Constraints |
|-----------|-----------|------------|
| patient_id | NUMBER(10) | PRIMARY KEY |
| first_name | VARCHAR2(50) | NOT NULL |
| last_name | VARCHAR2(50) | NOT NULL |
| date_of_birth | DATE | NOT NULL |
| gender | CHAR(1) | CHECK (gender IN ('M','F','O')) |
| phone | VARCHAR2(15) | NOT NULL |
| email | VARCHAR2(100) | UNIQUE |
| street_address | VARCHAR2(100) | |
| city | VARCHAR2(50) | |
| state | CHAR(2) | |
| zip_code | VARCHAR2(10) | |
| insurance_provider | VARCHAR2(100) | |
| insurance_number | VARCHAR2(50) | |

### 2. STAFF
| Attribute | Data Type | Constraints |
|-----------|-----------|------------|
| staff_id | NUMBER(10) | PRIMARY KEY |
| first_name | VARCHAR2(50) | NOT NULL |
| last_name | VARCHAR2(50) | NOT NULL |
| role | VARCHAR2(30) | CHECK (role IN ('Doctor','Nurse','Technician','Admin')) |
| specialty | VARCHAR2(50) | |
| phone | VARCHAR2(15) | NOT NULL |
| email | VARCHAR2(100) | UNIQUE, NOT NULL |
| license_number | VARCHAR2(30) | UNIQUE |
| hire_date | DATE | NOT NULL |

### 3. APPOINTMENTS
| Attribute | Data Type | Constraints |
|-----------|-----------|------------|
| appointment_id | NUMBER(10) | PRIMARY KEY |
| patient_id | NUMBER(10) | FOREIGN KEY → PATIENTS |
| scheduled_date | DATE | NOT NULL |
| scheduled_time | VARCHAR2(5) | NOT NULL |
| appointment_type | VARCHAR2(30) | CHECK (type IN ('Walk-in','Scheduled','Emergency')) |
| status | VARCHAR2(20) | CHECK (status IN ('Scheduled','Completed','Cancelled','No-Show')) |
| doctor_id | NUMBER(10) | FOREIGN KEY → STAFF |
| room_number | VARCHAR2(10) | |

### 4. ASSESSMENTS
| Attribute | Data Type | Constraints |
|-----------|-----------|------------|
| assessment_id | NUMBER(10) | PRIMARY KEY |
| appointment_id | NUMBER(10) | FOREIGN KEY → APPOINTMENTS |
| chief_complaint | VARCHAR2(500) | NOT NULL |
| vital_signs | VARCHAR2(200) | |
| blood_pressure | VARCHAR2(10) | |
| temperature | NUMBER(4,1) | CHECK (temperature BETWEEN 90 AND 110) |
| pulse | NUMBER(3) | CHECK (pulse BETWEEN 40 AND 200) |
| assessment_notes | CLOB | |
| preliminary_diagnosis | VARCHAR2(200) | |
| severity_level | NUMBER(1) | CHECK (level BETWEEN 1 AND 5) |

### 5. DIAGNOSTIC_TESTS
| Attribute | Data Type | Constraints |
|-----------|-----------|------------|
| test_id | NUMBER(10) | PRIMARY KEY |
| appointment_id | NUMBER(10) | FOREIGN KEY → APPOINTMENTS |
| test_type | VARCHAR2(50) | NOT NULL |
| test_name | VARCHAR2(100) | NOT NULL |
| ordered_date | DATE | NOT NULL |
| performed_date | DATE | |
| technician_id | NUMBER(10) | FOREIGN KEY → STAFF |
| status | VARCHAR2(20) | CHECK (status IN ('Ordered','In-Progress','Completed')) |
| priority | VARCHAR2(10) | CHECK (priority IN ('Routine','Urgent','STAT')) |

### 6. TEST_RESULTS
| Attribute | Data Type | Constraints |
|-----------|-----------|------------|
| result_id | NUMBER(10) | PRIMARY KEY |
| test_id | NUMBER(10) | FOREIGN KEY → DIAGNOSTIC_TESTS |
| result_value | VARCHAR2(500) | NOT NULL |
| result_date | DATE | NOT NULL |
| is_normal | CHAR(1) | CHECK (is_normal IN ('Y','N')) |
| reference_range | VARCHAR2(100) | |
| notes | VARCHAR2(1000) | |
| reviewed_by | NUMBER(10) | FOREIGN KEY → STAFF |

---

## Normalization Summary

### Benefits Achieved:
1. **Eliminated redundancy** - Patient info stored once
2. **Improved data integrity** - Foreign keys ensure valid relationships
3. **Flexibility** - Can add multiple tests without schema changes
4. **Scalability** - Efficient storage and retrieval
5. **Maintainability** - Updates affect single records

### Key Relationships:
- PATIENTS ← APPOINTMENTS (One-to-Many)
- APPOINTMENTS ← ASSESSMENTS (One-to-One)
- APPOINTMENTS ← DIAGNOSTIC_TESTS (One-to-Many)
- DIAGNOSTIC_TESTS ← TEST_RESULTS (One-to-One)
- STAFF → APPOINTMENTS, DIAGNOSTIC_TESTS, TEST_RESULTS (One-to-Many)
