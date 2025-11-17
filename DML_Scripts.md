# DML Scripts - MedFirst Diagnostic Center
## Data Manipulation Language for Sample Data

```sql
-- =====================================================
-- DML SCRIPT FOR URGENT CARE DIAGNOSTIC CENTER DATABASE
-- Author: John Perrotti
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
DELETE FROM STAFF;
DELETE FROM PATIENTS;
*/

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
-- INSERT INTO STAFF (5 records - 2 doctors, 2 technicians, 1 nurse)
-- =====================================================

INSERT INTO STAFF (staff_id, first_name, last_name, role, specialty, 
                  phone, email, license_number, hire_date)
VALUES (staff_seq.NEXTVAL, 'Dr. William', 'Chen', 'Doctor', 'Internal Medicine', 
        '555-200-1111', 'wchen@medfirst.com', 'MD-IL-2019-1234', DATE '2019-06-01');

INSERT INTO STAFF (staff_id, first_name, last_name, role, specialty, 
                  phone, email, license_number, hire_date)
VALUES (staff_seq.NEXTVAL, 'Dr. Lisa', 'Martinez', 'Doctor', 'Emergency Medicine', 
        '555-200-2222', 'lmartinez@medfirst.com', 'MD-IL-2018-5678', DATE '2020-01-15');

INSERT INTO STAFF (staff_id, first_name, last_name, role, specialty, 
                  phone, email, license_number, hire_date)
VALUES (staff_seq.NEXTVAL, 'Jennifer', 'Taylor', 'Nurse', 'Triage', 
        '555-200-3333', 'jtaylor@medfirst.com', 'RN-IL-2020-9876', DATE '2021-03-10');

INSERT INTO STAFF (staff_id, first_name, last_name, role, specialty, 
                  phone, email, license_number, hire_date)
VALUES (staff_seq.NEXTVAL, 'Thomas', 'Anderson', 'Technician', 'Radiology', 
        '555-200-4444', 'tanderson@medfirst.com', 'RT-IL-2019-5432', DATE '2019-11-20');

INSERT INTO STAFF (staff_id, first_name, last_name, role, specialty, 
                  phone, email, license_number, hire_date)
VALUES (staff_seq.NEXTVAL, 'Maria', 'Garcia', 'Technician', 'Laboratory', 
        '555-200-5555', 'mgarcia@medfirst.com', 'MLT-IL-2021-1357', DATE '2022-02-28');

-- =====================================================
-- INSERT INTO APPOINTMENTS (5 records)
-- Note: Using specific IDs to match with staff and patients
-- =====================================================

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, doctor_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1000, DATE '2024-11-15', '09:00', 
        'Scheduled', 'Completed', 100, 'A101');

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, doctor_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1001, DATE '2024-11-15', '10:30', 
        'Walk-in', 'Completed', 101, 'A102');

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, doctor_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1002, DATE '2024-11-16', '14:00', 
        'Emergency', 'Completed', 101, 'E101');

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, doctor_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1003, DATE '2024-11-17', '11:00', 
        'Scheduled', 'Completed', 100, 'A103');

INSERT INTO APPOINTMENTS (appointment_id, patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, doctor_id, room_number)
VALUES (appointment_seq.NEXTVAL, 1004, DATE '2024-11-18', '15:30', 
        'Walk-in', 'Scheduled', 101, 'A104');

-- =====================================================
-- INSERT INTO ASSESSMENTS (5 records)
-- One for each completed appointment
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
-- Various tests ordered for different appointments
-- =====================================================

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5000, 'Laboratory', 'Complete Blood Count', 
        DATE '2024-11-15', DATE '2024-11-15', 104, 'Completed', 'Routine');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5001, 'Imaging', 'Head CT Scan', 
        DATE '2024-11-15', DATE '2024-11-15', 103, 'Completed', 'Urgent');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5002, 'Cardiology', 'Electrocardiogram', 
        DATE '2024-11-16', DATE '2024-11-16', 103, 'Completed', 'STAT');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5002, 'Laboratory', 'Troponin Level', 
        DATE '2024-11-16', DATE '2024-11-16', 104, 'Completed', 'STAT');

INSERT INTO DIAGNOSTIC_TESTS (test_id, appointment_id, test_type, test_name, 
                             ordered_date, performed_date, technician_id, status, priority)
VALUES (test_seq.NEXTVAL, 5003, 'Laboratory', 'Comprehensive Metabolic Panel', 
        DATE '2024-11-17', DATE '2024-11-17', 104, 'Completed', 'Routine');

-- =====================================================
-- INSERT INTO TEST_RESULTS (5 records)
-- Results for the completed tests
-- =====================================================

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewed_by)
VALUES (result_seq.NEXTVAL, 10000, 'WBC: 12.5, RBC: 4.8, Hgb: 14.2, Plt: 250', 
        DATE '2024-11-15', 'N', 'WBC: 4.5-11.0', 'Elevated WBC suggests infection', 100);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewed_by)
VALUES (result_seq.NEXTVAL, 10001, 'No acute intracranial abnormality', 
        DATE '2024-11-15', 'Y', 'Normal', 'No hemorrhage or mass effect', 101);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewed_by)
VALUES (result_seq.NEXTVAL, 10002, 'Normal sinus rhythm, rate 92 bpm', 
        DATE '2024-11-16', 'Y', 'Normal', 'No ST changes or arrhythmias', 101);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewed_by)
VALUES (result_seq.NEXTVAL, 10003, 'Troponin I: 0.02 ng/mL', 
        DATE '2024-11-16', 'Y', '<0.04 ng/mL', 'No evidence of myocardial injury', 101);

INSERT INTO TEST_RESULTS (result_id, test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewed_by)
VALUES (result_seq.NEXTVAL, 10004, 'Glucose: 95, Creatinine: 0.9, K: 4.2, Na: 140', 
        DATE '2024-11-17', 'Y', 'All within normal limits', 'Healthy metabolic profile', 100);

-- Commit all changes
COMMIT;

-- =====================================================
-- SECTION 2: SELECT STATEMENTS
-- Retrieve all rows and columns from each table
-- =====================================================

-- Select all PATIENTS
SELECT * FROM PATIENTS
ORDER BY patient_id;

-- Select all STAFF
SELECT * FROM STAFF
ORDER BY staff_id;

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
-- Show relationships between tables
-- =====================================================

-- Query 1: Patient appointment history with doctor names
SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    a.scheduled_date,
    a.scheduled_time,
    a.appointment_type,
    s.first_name || ' ' || s.last_name AS doctor_name,
    a.status
FROM APPOINTMENTS a
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.doctor_id = s.staff_id
ORDER BY a.scheduled_date DESC;

-- Query 2: Test results for each patient
SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    dt.test_name,
    dt.test_type,
    tr.result_value,
    tr.is_normal,
    tr.result_date
FROM PATIENTS p
JOIN APPOINTMENTS a ON p.patient_id = a.patient_id
JOIN DIAGNOSTIC_TESTS dt ON a.appointment_id = dt.appointment_id
JOIN TEST_RESULTS tr ON dt.test_id = tr.test_id
ORDER BY p.patient_id, tr.result_date;

-- Query 3: Assessment summary with severity levels
SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    ass.chief_complaint,
    ass.preliminary_diagnosis,
    ass.severity_level,
    a.scheduled_date
FROM ASSESSMENTS ass
JOIN APPOINTMENTS a ON ass.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
ORDER BY ass.severity_level DESC, a.scheduled_date DESC;

-- Query 4: Staff workload analysis
SELECT 
    s.role,
    s.first_name || ' ' || s.last_name AS staff_name,
    COUNT(DISTINCT a.appointment_id) AS total_appointments,
    COUNT(DISTINCT dt.test_id) AS tests_performed
FROM STAFF s
LEFT JOIN APPOINTMENTS a ON s.staff_id = a.doctor_id
LEFT JOIN DIAGNOSTIC_TESTS dt ON s.staff_id = dt.technician_id
GROUP BY s.staff_id, s.role, s.first_name, s.last_name
ORDER BY s.role, total_appointments DESC;

-- Query 5: Daily appointment schedule
SELECT 
    TO_CHAR(a.scheduled_date, 'DD-MON-YYYY') AS appointment_date,
    a.scheduled_time,
    p.first_name || ' ' || p.last_name AS patient_name,
    a.appointment_type,
    s.first_name || ' ' || s.last_name AS doctor_name,
    a.room_number,
    a.status
FROM APPOINTMENTS a
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.doctor_id = s.staff_id
WHERE a.scheduled_date >= SYSDATE - 30  -- Last 30 days
ORDER BY a.scheduled_date, a.scheduled_time;

-- =====================================================
-- END OF DML SCRIPT
-- Total Records Inserted: 30 (5 per table)
-- =====================================================
```

---

## Testing Instructions for DataGrip:

1. **Run DDL script first** to create all tables
2. **Execute INSERT statements** one section at a time
3. **Run SELECT queries** to verify data insertion
4. **Test the relationship queries** to demonstrate foreign key connections

#

## Notes:

- The IDs are hardcoded in some places to ensure foreign key relationships work
- You can modify the sample data to better match your presentation needs
- The "useful queries" section shows the database relationships in action
- All dates are recent for demo relevance
