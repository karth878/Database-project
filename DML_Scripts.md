# DML Scripts - MedFirst Diagnostic Center
## Data Manipulation Language for Sample Data

```sql
-- =====================================================
-- DML SCRIPT FOR MEDIRST DIAGNOSTIC CENTER DATABASE
-- Author: Database Designers Inc.
-- Purpose: Insert sample data and retrieve records
-- =====================================================

-- =====================================================
-- SECTION 1: INSERT STATEMENTS
-- Insert 5 sample records into each table
-- =====================================================

-- Clear existing data (optional - be careful!)
/*
DELETE FROM TEST_RESULTS;
DELETE FROM DIAGNOSTIC_TESTS;
DELETE FROM ASSESSMENTS;
DELETE FROM APPOINTMENTS;
DELETE FROM DOCTORS;
DELETE FROM STAFF;
DELETE FROM PATIENTS;
DELETE FROM CLINIC_INFO;
*/

-- =====================================================
-- INSERT INTO CLINIC_INFO (1 main clinic record)
-- =====================================================

INSERT INTO CLINIC_INFO (clinic_id, clinic_name, street_address, city, state, zip_code, 
                        phone, email, fax, operating_hours, emergency_contact)
VALUES (clinic_seq.NEXTVAL, 'MedFirst Diagnostic Center', '500 Medical Plaza Dr', 
        'Springfield', 'IL', '62701', '555-100-2000', 'info@medfirst.com', 
        '555-100-2001', 'Mon-Fri: 8AM-8PM, Sat: 9AM-5PM, Sun: 10AM-4PM', '555-100-9999');

-- =====================================================
-- INSERT INTO PATIENTS (5 records)
-- =====================================================

INSERT INTO PATIENTS (patient_id, first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES (patient_seq.NEXTVAL, 'John', 'Smith', DATE '1985-03-15', 'M', 
        '555-123-4567', 'john.smith@email.com', '123 Main St', 'Springfield', 'IL', '62701',
        'BlueCross BlueShield', 'BC123456789');

INSERT INTO PATIENTS (patient_id, first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES (patient_seq.NEXTVAL, 'Sarah', 'Johnson', DATE '1990-07-22', 'F', 
        '555-987-6543', 'sarah.j@email.com', '456 Oak Ave', 'Springfield', 'IL', '62702',
        'Aetna', 'AET987654321');

INSERT INTO PATIENTS (patient_id, first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES (patient_seq.NEXTVAL, 'Michael', 'Davis', DATE '1978-11-30', 'M', 
        '555-555-1234', 'mdavis@email.com', '789 Pine Rd', 'Chatham', 'IL', '62629',
        'United Healthcare', 'UHC456789123');

INSERT INTO PATIENTS (patient_id, first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES (patient_seq.NEXTVAL, 'Emily', 'Wilson', DATE '2005-04-18', 'F', 
        '555-333-9999', 'emily.wilson@email.com', '321 Elm St', 'Rochester', 'IL', '62563',
        'Cigna', 'CIG789456123');

INSERT INTO PATIENTS (patient_id, first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES (patient_seq.NEXTVAL, 'Robert', 'Brown', DATE '1965-09-08', 'M', 
        '555-777-8888', 'rbrown@email.com', '654 Maple Dr', 'Sherman', 'IL', '62684',
        'Medicare', 'MED321654987');

-- =====================================================
-- INSERT INTO STAFF (8 records - 3 doctors, 2 nurses, 2 technicians, 1 admin)
-- Note: Using clinic_id = 1 for all staff
-- =====================================================

-- Doctors (will also be in DOCTORS table)
INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'William', 'Chen', 'Doctor', 
        '555-200-1111', 'wchen@medfirst.com', DATE '2019-06-01', 1);

INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'Lisa', 'Martinez', 'Doctor', 
        '555-200-2222', 'lmartinez@medfirst.com', DATE '2020-01-15', 1);

INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'James', 'Thompson', 'Doctor', 
        '555-200-3333', 'jthompson@medfirst.com', DATE '2018-03-20', 1);

-- Nurses
INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'Jennifer', 'Taylor', 'Nurse', 
        '555-200-4444', 'jtaylor@medfirst.com', DATE '2021-03-10', 1);

INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'Patricia', 'White', 'Nurse', 
        '555-200-5555', 'pwhite@medfirst.com', DATE '2022-05-15', 1);

-- Technicians
INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'Thomas', 'Anderson', 'Technician', 
        '555-200-6666', 'tanderson@medfirst.com', DATE '2019-11-20', 1);

INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'Maria', 'Garcia', 'Technician', 
        '555-200-7777', 'mgarcia@medfirst.com', DATE '2022-02-28', 1);

-- Admin
INSERT INTO STAFF (staff_id, first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES (staff_seq.NEXTVAL, 'David', 'Miller', 'Admin', 
        '555-200-8888', 'dmiller@medfirst.com', DATE '2020-09-01', 1);

-- =====================================================
-- INSERT INTO DOCTORS (3 records for the doctor staff members)
-- Assuming doctor IDs are 100, 101, 102
-- =====================================================

INSERT INTO DOCTORS (doctor_id, specialty, license_number, board_certification, 
                    workplace, hospital_affiliation, years_experience)
VALUES (100, 'Internal Medicine', 'MD-IL-2019-1234', 'American Board of Internal Medicine', 
        'MedFirst Diagnostic Center', 'Springfield General Hospital', 12);

INSERT INTO DOCTORS (doctor_id, specialty, license_number, board_certification, 
                    workplace, hospital_affiliation, years_experience)
VALUES (101, 'Emergency Medicine', 'MD-IL-2018-5678', 'American Board of Emergency Medicine', 
        'MedFirst Diagnostic Center', 'St. Johns Medical Center', 8);

INSERT INTO DOCTORS (doctor_id, specialty, license_number, board_certification, 
                    workplace, hospital_affiliation, years_experience)
VALUES (102, 'Family Medicine', 'MD-IL-2017-9012', 'American Board of Family Medicine', 
        'MedFirst Diagnostic Center', 'Memorial Hospital', 15);

-- =====================================================
-- INSERT INTO APPOINTMENTS (5 records)
-- Note: clinic_id = 1, using both doctors and nurses as attending staff
-- =====================================================

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1000, DATE '2024-11-15', '09:00', 
        'Scheduled', 'Completed', 100, 1, 'A101');  -- Doctor attending

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1001, DATE '2024-11-15', '10:30', 
        'Walk-in', 'Completed', 103, 1, 'A102');  -- Nurse attending initially

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1002, DATE '2024-11-16', '14:00', 
        'Emergency', 'Completed', 101, 1, 'E101');  -- Doctor attending

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1003, DATE '2024-11-17', '11:00', 
        'Scheduled', 'Completed', 102, 1, 'A103');  -- Doctor attending

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1004, DATE '2024-11-18', '15:30', 
        'Walk-in', 'Scheduled', 104, 1, 'A104');  -- Nurse attending

-- =====================================================
-- INSERT INTO ASSESSMENTS (5 records)
-- One for each appointment
-- =====================================================

INSERT INTO ASSESSMENTS (assessment_id, appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES (assessment_seq.NEXTVAL, 5000, 'Persistent cough and fever for 3 days', 
        'BP: 130/85, Temp: 101.2F, Pulse: 88', '130/85', 101.2, 88,
        'Patient presents with upper respiratory symptoms. Lungs clear on auscultation. Throat erythematous.',
        'Upper Respiratory Infection', 2);

INSERT INTO ASSESSMENTS (assessment_id, appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES (assessment_seq.NEXTVAL, 5001, 'Severe headache and dizziness', 
        'BP: 145/95, Temp: 98.6F, Pulse: 92', '145/95', 98.6, 92,
        'Patient reports migraine-like symptoms. Photophobia present. No neurological deficits.',
        'Migraine Headache', 3);

INSERT INTO ASSESSMENTS (assessment_id, appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES (assessment_seq.NEXTVAL, 5002, 'Chest pain and shortness of breath', 
        'BP: 160/100, Temp: 98.4F, Pulse: 110', '160/100', 98.4, 110,
        'Acute onset chest pain. ECG shows no acute changes. Troponin ordered.',
        'Chest Pain - Rule out MI', 4);

INSERT INTO ASSESSMENTS (assessment_id, appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES (assessment_seq.NEXTVAL, 5003, 'Annual check-up', 
        'BP: 120/80, Temp: 98.2F, Pulse: 72', '120/80', 98.2, 72,
        'Routine physical exam. Patient healthy, no complaints. Labs ordered for screening.',
        'Annual Physical - Healthy', 1);

INSERT INTO ASSESSMENTS (assessment_id, appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES (assessment_seq.NEXTVAL, 5004, 'Lower back pain after lifting', 
        'BP: 125/82, Temp: 98.5F, Pulse: 78', '125/82', 98.5, 78,
        'Acute lumbar strain. No radicular symptoms. Prescribed NSAIDs and rest.',
        'Lumbar Strain', 2);

-- =====================================================
-- INSERT INTO DIAGNOSTIC_TESTS (5 records)
-- Note: Only doctors can order tests
-- =====================================================

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5000, 'Laboratory', 'Complete Blood Count', 
        DATE '2024-11-15', DATE '2024-11-15', 100, 106, 'Completed', 'Routine');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5001, 'Imaging', 'Head CT Scan', 
        DATE '2024-11-15', DATE '2024-11-15', 101, 105, 'Completed', 'Urgent');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5002, 'Cardiology', 'Electrocardiogram', 
        DATE '2024-11-16', DATE '2024-11-16', 101, 105, 'Completed', 'STAT');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5002, 'Laboratory', 'Troponin Level', 
        DATE '2024-11-16', DATE '2024-11-16', 101, 106, 'Completed', 'STAT');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5003, 'Laboratory', 'Comprehensive Metabolic Panel', 
        DATE '2024-11-17', DATE '2024-11-17', 102, 106, 'Completed', 'Routine');

-- =====================================================
-- INSERT INTO TEST_RESULTS (5 records)
-- Note: Only doctors can review results
-- =====================================================

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES (result_seq.NEXTVAL, 10000, 'WBC: 12.5, RBC: 4.8, Hgb: 14.2, Plt: 250', 
        DATE '2024-11-15', 'N', 'WBC: 4.5-11.0', 'Elevated WBC suggests infection', 100);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES (result_seq.NEXTVAL, 10001, 'No acute intracranial abnormality', 
        DATE '2024-11-15', 'Y', 'Normal', 'No hemorrhage or mass effect', 101);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES (result_seq.NEXTVAL, 10002, 'Normal sinus rhythm, rate 92 bpm', 
        DATE '2024-11-16', 'Y', 'Normal', 'No ST changes or arrhythmias', 101);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES (result_seq.NEXTVAL, 10003, 'Troponin I: 0.02 ng/mL', 
        DATE '2024-11-16', 'Y', '<0.04 ng/mL', 'No evidence of myocardial injury', 101);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES (result_seq.NEXTVAL, 10004, 'Glucose: 95, Creatinine: 0.9, K: 4.2, Na: 140', 
        DATE '2024-11-17', 'Y', 'All within normal limits', 'Healthy metabolic profile', 102);

-- Commit all changes
COMMIT;

-- =====================================================
-- SECTION 2: SELECT STATEMENTS
-- Retrieve all rows and columns from each table
-- =====================================================

-- Select CLINIC_INFO
SELECT * FROM CLINIC_INFO
ORDER BY clinic_id;

-- Select all PATIENTS
SELECT * FROM PATIENTS
ORDER BY patient_id;

-- Select all STAFF
SELECT * FROM STAFF
ORDER BY staff_id;

-- Select all DOCTORS with their staff info
SELECT s.*, d.*
FROM STAFF s
JOIN DOCTORS d ON s.staff_id = d.doctor_id
ORDER BY d.doctor_id;

-- Select all APPOINTMENTS
SELECT * FROM APPOINTMENTS
ORDER BY appointment_id;

-- Select all ASSESSMENTS
SELECT * FROM ASSESSMENTS
ORDER BY assessment_id;

-- Select all DIAGNOSTIC_TESTS
SELECT * FROM DIAGNOSTIC_TESTS
ORDER BY test_id;

-- Select all TEST_RESULTS
SELECT * FROM TEST_RESULTS
ORDER BY result_id;

-- =====================================================
-- SECTION 3: USEFUL QUERIES FOR DEMONSTRATION
-- Show relationships between tables and business rules
-- =====================================================

-- Query 1: Patient appointment history with attending staff and clinic
SELECT 
    c.clinic_name,
    p.first_name || ' ' || p.last_name AS patient_name,
    a.scheduled_date,
    a.scheduled_time,
    a.appointment_type,
    s.first_name || ' ' || s.last_name AS attending_staff,
    s.role AS staff_role,
    a.status
FROM APPOINTMENTS a
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.attending_staff_id = s.staff_id
JOIN CLINIC_INFO c ON a.clinic_id = c.clinic_id
ORDER BY a.scheduled_date DESC;

-- Query 2: Tests ordered by doctors only (showing business rule)
SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    dt.test_name,
    dt.test_type,
    d.first_name || ' ' || d.last_name AS ordering_doctor,
    doc.specialty AS doctor_specialty,
    doc.workplace,
    dt.priority,
    dt.status
FROM DIAGNOSTIC_TESTS dt
JOIN APPOINTMENTS a ON dt.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF d ON dt.ordering_doctor_id = d.staff_id
JOIN DOCTORS doc ON d.staff_id = doc.doctor_id
ORDER BY dt.ordered_date DESC;

-- Query 3: Test results reviewed by doctors only
SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    dt.test_name,
    tr.result_value,
    tr.is_normal,
    d.first_name || ' ' || d.last_name AS reviewing_doctor,
    doc.specialty,
    doc.hospital_affiliation
FROM TEST_RESULTS tr
JOIN DIAGNOSTIC_TESTS dt ON tr.test_id = dt.test_id
JOIN APPOINTMENTS a ON dt.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF d ON tr.reviewing_doctor_id = d.staff_id
JOIN DOCTORS doc ON d.staff_id = doc.doctor_id
ORDER BY tr.result_date DESC;

-- Query 4: Doctor workload analysis (showing specialization)
SELECT 
    s.first_name || ' ' || s.last_name AS doctor_name,
    d.specialty,
    d.workplace,
    d.hospital_affiliation,
    COUNT(DISTINCT a.appointment_id) AS appointments_attended,
    COUNT(DISTINCT dt.test_id) AS tests_ordered
FROM DOCTORS d
JOIN STAFF s ON d.doctor_id = s.staff_id
LEFT JOIN APPOINTMENTS a ON s.staff_id = a.attending_staff_id
LEFT JOIN DIAGNOSTIC_TESTS dt ON d.doctor_id = dt.ordering_doctor_id
GROUP BY s.staff_id, s.first_name, s.last_name, d.specialty, d.workplace, d.hospital_affiliation
ORDER BY tests_ordered DESC;

-- Query 5: Clinic daily schedule showing mix of staff types
SELECT 
    c.clinic_name,
    TO_CHAR(a.scheduled_date, 'DD-MON-YYYY') AS appointment_date,
    a.scheduled_time,
    p.first_name || ' ' || p.last_name AS patient_name,
    a.appointment_type,
    s.first_name || ' ' || s.last_name AS attending_staff,
    s.role AS staff_type,
    a.room_number,
    a.status
FROM APPOINTMENTS a
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.attending_staff_id = s.staff_id
JOIN CLINIC_INFO c ON a.clinic_id = c.clinic_id
WHERE a.scheduled_date >= SYSDATE - 30
ORDER BY a.scheduled_date, a.scheduled_time;

-- Query 6: Verify business rule - only doctors in DOCTORS table
SELECT 
    s.staff_id,
    s.first_name || ' ' || s.last_name AS name,
    s.role,
    CASE 
        WHEN d.doctor_id IS NOT NULL THEN 'Yes'
        ELSE 'No'
    END AS is_in_doctors_table
FROM STAFF s
LEFT JOIN DOCTORS d ON s.staff_id = d.doctor_id
ORDER BY s.role, s.staff_id;

-- Query 7: Assessment summary with severity levels and diagnoses
SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    ass.chief_complaint,
    ass.preliminary_diagnosis,
    ass.severity_level,
    s.first_name || ' ' || s.last_name AS attending_staff,
    s.role,
    a.scheduled_date
FROM ASSESSMENTS ass
JOIN APPOINTMENTS a ON ass.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.attending_staff_id = s.staff_id
ORDER BY ass.severity_level DESC, a.scheduled_date DESC;

-- =====================================================
-- END OF DML SCRIPT
-- Total Records Inserted: 38
-- - CLINIC_INFO: 1
-- - PATIENTS: 5
-- - STAFF: 8
-- - DOCTORS: 3
-- - APPOINTMENTS: 5
-- - ASSESSMENTS: 5
-- - DIAGNOSTIC_TESTS: 5
-- - TEST_RESULTS: 5
-- =====================================================
```

---

## Testing Instructions for DataGrip:

1. **Run DDL script first** to create all tables
2. **Execute INSERT statements** one section at a time
3. **Run SELECT queries** to verify data insertion
4. **Test the relationship queries** to demonstrate foreign key connections

## Key Features:

- **CLINIC_INFO** establishes the facility context
- **DOCTORS as subtype** shows IS-A relationship with STAFF
- **Role-based constraints** enforced - only doctors order tests/review results
- **Mixed staff attendance** - nurses and doctors can attend appointments
- **Workplace tracking** - doctors show both clinic and hospital affiliations
- **Realistic medical data** with proper relationships
- **Complete data integrity** with all foreign keys satisfied

## Important Notes:

- Staff IDs 100-102 are doctors (also in DOCTORS table)
- Staff IDs 103-104 are nurses
- Staff IDs 105-106 are technicians
- Staff ID 107 is admin
- All staff belong to clinic_id = 1
- Only doctors can order tests (enforced by FK to DOCTORS)
- Only doctors can review results (enforced by FK to DOCTORS)
- Any staff member can attend appointments