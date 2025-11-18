# DML Scripts - MedFirst Diagnostic Center
## Data Manipulation Language for Sample Data - Production Ready

```sql
-- =====================================================
-- DML SCRIPT FOR MEDFIRST DIAGNOSTIC CENTER DATABASE
-- Author: Database Designers Inc.
-- Purpose: Insert sample data and retrieve records
-- Matches ERD and DDL perfectly
-- Date: 2024-11-18
-- =====================================================

-- Enable error handling
SET DEFINE OFF;
WHENEVER SQLERROR EXIT SQL.SQLCODE;

-- =====================================================
-- OPTIONAL: Clear existing data
-- WARNING: This will permanently delete all data!
-- Only uncomment if you need to reset the database.
-- =====================================================
/*
DELETE FROM TEST_RESULTS;
DELETE FROM DIAGNOSTIC_TESTS;
DELETE FROM ASSESSMENTS;
DELETE FROM APPOINTMENTS;
DELETE FROM DOCTORS;
DELETE FROM STAFF;
DELETE FROM PATIENTS;
DELETE FROM CLINIC_INFO;

-- Reset sequences to starting values
ALTER SEQUENCE clinic_seq RESTART START WITH 1;
ALTER SEQUENCE patient_seq RESTART START WITH 1000;
ALTER SEQUENCE staff_seq RESTART START WITH 100;
ALTER SEQUENCE appointment_seq RESTART START WITH 5000;
ALTER SEQUENCE assessment_seq RESTART START WITH 5000;
ALTER SEQUENCE test_seq RESTART START WITH 10000;
ALTER SEQUENCE result_seq RESTART START WITH 10000;

COMMIT;
*/

-- =====================================================
-- SECTION 1: INSERT STATEMENTS
-- Insert 5+ sample records into each table
-- Using triggers for auto-increment (no need for NEXTVAL)
-- =====================================================

-- =====================================================
-- INSERT INTO CLINIC_INFO (1 main clinic record)
-- =====================================================

INSERT INTO CLINIC_INFO (clinic_name, street_address, city, state, zip_code, 
                        phone, email, fax, operating_hours, emergency_contact)
VALUES ('MedFirst Diagnostic Center', '500 Medical Plaza Dr', 
        'Springfield', 'IL', '62701', '555-100-2000', 'info@medfirst.com', 
        '555-100-2001', 'Mon-Fri: 8AM-8PM, Sat: 9AM-5PM, Sun: 10AM-4PM', '555-100-9999');

-- =====================================================
-- INSERT INTO PATIENTS (5 records)
-- =====================================================

INSERT INTO PATIENTS (first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES ('John', 'Smith', DATE '1985-03-15', 'M', 
        '555-123-4567', 'john.smith@email.com', '123 Main St', 'Springfield', 'IL', '62701',
        'BlueCross BlueShield', 'BC123456789');

INSERT INTO PATIENTS (first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES ('Sarah', 'Johnson', DATE '1990-07-22', 'F', 
        '555-987-6543', 'sarah.j@email.com', '456 Oak Ave', 'Springfield', 'IL', '62702',
        'Aetna', 'AET987654321');

INSERT INTO PATIENTS (first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES ('Michael', 'Davis', DATE '1978-11-30', 'M', 
        '555-555-1234', 'mdavis@email.com', '789 Pine Rd', 'Chatham', 'IL', '62629',
        'United Healthcare', 'UHC456789123');

INSERT INTO PATIENTS (first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES ('Emily', 'Wilson', DATE '2005-04-18', 'F', 
        '555-333-9999', 'emily.wilson@email.com', '321 Elm St', 'Rochester', 'IL', '62563',
        'Cigna', 'CIG789456123');

INSERT INTO PATIENTS (first_name, last_name, date_of_birth, gender, 
                     phone, email, street_address, city, state, zip_code, 
                     insurance_provider, insurance_number)
VALUES ('Robert', 'Brown', DATE '1965-09-08', 'M', 
        '555-777-8888', 'rbrown@email.com', '654 Maple Dr', 'Sherman', 'IL', '62684',
        'Medicare', 'MED321654987');

-- =====================================================
-- INSERT INTO STAFF (8 records - 3 doctors, 2 nurses, 2 technicians, 1 admin)
-- Note: clinic_id will be fetched dynamically
-- =====================================================

-- Doctors (will also be in DOCTORS table)
INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('William', 'Chen', 'Doctor', 
        '555-200-1111', 'wchen@medfirst.com', DATE '2019-06-01', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('Lisa', 'Martinez', 'Doctor', 
        '555-200-2222', 'lmartinez@medfirst.com', DATE '2020-01-15', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('James', 'Thompson', 'Doctor', 
        '555-200-3333', 'jthompson@medfirst.com', DATE '2018-03-20', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

-- Nurses
INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('Jennifer', 'Taylor', 'Nurse', 
        '555-200-4444', 'jtaylor@medfirst.com', DATE '2021-03-10', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('Patricia', 'White', 'Nurse', 
        '555-200-5555', 'pwhite@medfirst.com', DATE '2022-05-15', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

-- Technicians
INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('Thomas', 'Anderson', 'Technician', 
        '555-200-6666', 'tanderson@medfirst.com', DATE '2019-11-20', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('Maria', 'Garcia', 'Technician', 
        '555-200-7777', 'mgarcia@medfirst.com', DATE '2022-02-28', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

-- Admin
INSERT INTO STAFF (first_name, last_name, role, phone, email, hire_date, clinic_id)
VALUES ('David', 'Miller', 'Admin', 
        '555-200-8888', 'dmiller@medfirst.com', DATE '2020-09-01', 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'));

-- =====================================================
-- INSERT INTO DOCTORS (3 records for the doctor staff members)
-- Uses subqueries to fetch actual staff_ids dynamically
-- =====================================================

INSERT INTO DOCTORS (doctor_id, specialty, license_number, board_certification, 
                    workplace, hospital_affiliation, years_experience)
VALUES ((SELECT staff_id FROM STAFF WHERE email = 'wchen@medfirst.com'), 
        'Internal Medicine', 'MD-IL-2019-1234', 'American Board of Internal Medicine', 
        'MedFirst Diagnostic Center', 'Springfield General Hospital', 12);

INSERT INTO DOCTORS (doctor_id, specialty, license_number, board_certification, 
                    workplace, hospital_affiliation, years_experience)
VALUES ((SELECT staff_id FROM STAFF WHERE email = 'lmartinez@medfirst.com'), 
        'Emergency Medicine', 'MD-IL-2018-5678', 'American Board of Emergency Medicine', 
        'MedFirst Diagnostic Center', 'St. Johns Medical Center', 8);

INSERT INTO DOCTORS (doctor_id, specialty, license_number, board_certification, 
                    workplace, hospital_affiliation, years_experience)
VALUES ((SELECT staff_id FROM STAFF WHERE email = 'jthompson@medfirst.com'), 
        'Family Medicine', 'MD-IL-2017-9012', 'American Board of Family Medicine', 
        'MedFirst Diagnostic Center', 'Memorial Hospital', 15);

-- =====================================================
-- INSERT INTO APPOINTMENTS (5 records)
-- Using current dates (adjust as needed for demo)
-- =====================================================

INSERT INTO APPOINTMENTS (patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES ((SELECT patient_id FROM PATIENTS WHERE email = 'john.smith@email.com'), 
        DATE '2025-11-15', '09:00', 'Scheduled', 'Completed', 
        (SELECT staff_id FROM STAFF WHERE email = 'wchen@medfirst.com'), 
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'), 
        'A101');

INSERT INTO APPOINTMENTS (patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES ((SELECT patient_id FROM PATIENTS WHERE email = 'sarah.j@email.com'), 
        DATE '2025-11-15', '10:30', 'Walk-in', 'Completed', 
        (SELECT staff_id FROM STAFF WHERE email = 'jtaylor@medfirst.com'),  -- Nurse
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'), 
        'A102');

INSERT INTO APPOINTMENTS (patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES ((SELECT patient_id FROM PATIENTS WHERE email = 'mdavis@email.com'), 
        DATE '2025-11-16', '14:00', 'Emergency', 'Completed', 
        (SELECT staff_id FROM STAFF WHERE email = 'lmartinez@medfirst.com'),  -- Doctor
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'), 
        'E101');

INSERT INTO APPOINTMENTS (patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES ((SELECT patient_id FROM PATIENTS WHERE email = 'emily.wilson@email.com'), 
        DATE '2025-11-17', '11:00', 'Scheduled', 'Completed', 
        (SELECT staff_id FROM STAFF WHERE email = 'jthompson@medfirst.com'),  -- Doctor
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'), 
        'A103');

INSERT INTO APPOINTMENTS (patient_id, scheduled_date, scheduled_time, 
                         appointment_type, status, attending_staff_id, clinic_id, room_number)
VALUES ((SELECT patient_id FROM PATIENTS WHERE email = 'rbrown@email.com'), 
        DATE '2025-11-18', '15:30', 'Walk-in', 'Scheduled', 
        (SELECT staff_id FROM STAFF WHERE email = 'pwhite@medfirst.com'),  -- Nurse
        (SELECT clinic_id FROM CLINIC_INFO WHERE clinic_name = 'MedFirst Diagnostic Center'), 
        'A104');

-- =====================================================
-- INSERT INTO ASSESSMENTS (5 records)
-- One for each completed appointment
-- =====================================================

INSERT INTO ASSESSMENTS (appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'john.smith@email.com')
         AND scheduled_date = DATE '2025-11-15'
         AND scheduled_time = '09:00'),
        'Persistent cough and fever for 3 days', 
        'BP: 130/85, Temp: 101.2F, Pulse: 88', '130/85', 101.2, 88,
        'Patient presents with upper respiratory symptoms. Lungs clear on auscultation. Throat erythematous.',
        'Upper Respiratory Infection', 2);

INSERT INTO ASSESSMENTS (appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'sarah.j@email.com')
         AND scheduled_date = DATE '2025-11-15'
         AND scheduled_time = '10:30'),
        'Severe headache and dizziness', 
        'BP: 145/95, Temp: 98.6F, Pulse: 92', '145/95', 98.6, 92,
        'Patient reports migraine-like symptoms. Photophobia present. No neurological deficits.',
        'Migraine Headache', 3);

INSERT INTO ASSESSMENTS (appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'mdavis@email.com')
         AND scheduled_date = DATE '2025-11-16'
         AND scheduled_time = '14:00'),
        'Chest pain and shortness of breath', 
        'BP: 160/100, Temp: 98.4F, Pulse: 110', '160/100', 98.4, 110,
        'Acute onset chest pain. ECG shows no acute changes. Troponin ordered.',
        'Chest Pain - Rule out MI', 4);

INSERT INTO ASSESSMENTS (appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'emily.wilson@email.com')
         AND scheduled_date = DATE '2025-11-17'
         AND scheduled_time = '11:00'),
        'Annual check-up', 
        'BP: 120/80, Temp: 98.2F, Pulse: 72', '120/80', 98.2, 72,
        'Routine physical exam. Patient healthy, no complaints. Labs ordered for screening.',
        'Annual Physical - Healthy', 1);

INSERT INTO ASSESSMENTS (appointment_id, chief_complaint, 
                        vital_signs, blood_pressure, temperature, pulse, 
                        assessment_notes, preliminary_diagnosis, severity_level)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'rbrown@email.com')
         AND scheduled_date = DATE '2025-11-18'
         AND scheduled_time = '15:30'),
        'Lower back pain after lifting', 
        'BP: 125/82, Temp: 98.5F, Pulse: 78', '125/82', 98.5, 78,
        'Acute lumbar strain. No radicular symptoms. Prescribed NSAIDs and rest.',
        'Lumbar Strain', 2);

-- =====================================================
-- INSERT INTO DIAGNOSTIC_TESTS (5 records)
-- Note: Only doctors can order tests (FK to DOCTORS table)
-- =====================================================

INSERT INTO DIAGNOSTIC_TESTS (appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'john.smith@email.com')
         AND scheduled_date = DATE '2025-11-15'
         AND scheduled_time = '09:00'),
        'Laboratory', 'Complete Blood Count', 
        DATE '2025-11-15', DATE '2025-11-15', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2019-1234'),
        (SELECT staff_id FROM STAFF WHERE email = 'mgarcia@medfirst.com'),
        'Completed', 'Routine');

INSERT INTO DIAGNOSTIC_TESTS (appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'sarah.j@email.com')
         AND scheduled_date = DATE '2025-11-15'
         AND scheduled_time = '10:30'),
        'Imaging', 'Head CT Scan', 
        DATE '2025-11-15', DATE '2025-11-15', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2018-5678'),
        (SELECT staff_id FROM STAFF WHERE email = 'tanderson@medfirst.com'),
        'Completed', 'Urgent');

INSERT INTO DIAGNOSTIC_TESTS (appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'mdavis@email.com')
         AND scheduled_date = DATE '2025-11-16'
         AND scheduled_time = '14:00'),
        'Cardiology', 'Electrocardiogram', 
        DATE '2025-11-16', DATE '2025-11-16', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2018-5678'),
        (SELECT staff_id FROM STAFF WHERE email = 'tanderson@medfirst.com'),
        'Completed', 'STAT');

INSERT INTO DIAGNOSTIC_TESTS (appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'mdavis@email.com')
         AND scheduled_date = DATE '2025-11-16'
         AND scheduled_time = '14:00'),
        'Laboratory', 'Troponin Level', 
        DATE '2025-11-16', DATE '2025-11-16', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2018-5678'),
        (SELECT staff_id FROM STAFF WHERE email = 'mgarcia@medfirst.com'),
        'Completed', 'STAT');

INSERT INTO DIAGNOSTIC_TESTS (appointment_id, test_type, test_name, 
                             ordered_date, performed_date, ordering_doctor_id, 
                             technician_id, status, priority)
VALUES ((SELECT appointment_id FROM APPOINTMENTS 
         WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'emily.wilson@email.com')
         AND scheduled_date = DATE '2025-11-17'
         AND scheduled_time = '11:00'),
        'Laboratory', 'Comprehensive Metabolic Panel', 
        DATE '2025-11-17', DATE '2025-11-17', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2017-9012'),
        (SELECT staff_id FROM STAFF WHERE email = 'mgarcia@medfirst.com'),
        'Completed', 'Routine');

-- =====================================================
-- INSERT INTO TEST_RESULTS (5 records)
-- Note: Only doctors can review results (FK to DOCTORS table)
-- =====================================================

INSERT INTO TEST_RESULTS (test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES ((SELECT test_id FROM DIAGNOSTIC_TESTS 
         WHERE test_name = 'Complete Blood Count' 
         AND appointment_id = (SELECT appointment_id FROM APPOINTMENTS 
                               WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'john.smith@email.com')
                               AND scheduled_date = DATE '2025-11-15'
                               AND scheduled_time = '09:00')),
        'WBC: 12.5, RBC: 4.8, Hgb: 14.2, Plt: 250', 
        DATE '2025-11-15', 'N', 'WBC: 4.5-11.0', 'Elevated WBC suggests infection', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2019-1234'));

INSERT INTO TEST_RESULTS (test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES ((SELECT test_id FROM DIAGNOSTIC_TESTS 
         WHERE test_name = 'Head CT Scan'
         AND appointment_id = (SELECT appointment_id FROM APPOINTMENTS 
                               WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'sarah.j@email.com')
                               AND scheduled_date = DATE '2025-11-15'
                               AND scheduled_time = '10:30')),
        'No acute intracranial abnormality', 
        DATE '2025-11-15', 'Y', 'Normal', 'No hemorrhage or mass effect', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2018-5678'));

INSERT INTO TEST_RESULTS (test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES ((SELECT test_id FROM DIAGNOSTIC_TESTS 
         WHERE test_name = 'Electrocardiogram'
         AND appointment_id = (SELECT appointment_id FROM APPOINTMENTS 
                               WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'mdavis@email.com')
                               AND scheduled_date = DATE '2025-11-16'
                               AND scheduled_time = '14:00')),
        'Normal sinus rhythm, rate 92 bpm', 
        DATE '2025-11-16', 'Y', 'Normal', 'No ST changes or arrhythmias', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2018-5678'));

INSERT INTO TEST_RESULTS (test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES ((SELECT test_id FROM DIAGNOSTIC_TESTS 
         WHERE test_name = 'Troponin Level'
         AND appointment_id = (SELECT appointment_id FROM APPOINTMENTS 
                               WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'mdavis@email.com')
                               AND scheduled_date = DATE '2025-11-16'
                               AND scheduled_time = '14:00')),
        'Troponin I: 0.02 ng/mL', 
        DATE '2025-11-16', 'Y', '<0.04 ng/mL', 'No evidence of myocardial injury', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2018-5678'));

INSERT INTO TEST_RESULTS (test_id, result_value, result_date, 
                         is_normal, reference_range, notes, reviewing_doctor_id)
VALUES ((SELECT test_id FROM DIAGNOSTIC_TESTS 
         WHERE test_name = 'Comprehensive Metabolic Panel'
         AND appointment_id = (SELECT appointment_id FROM APPOINTMENTS 
                               WHERE patient_id = (SELECT patient_id FROM PATIENTS WHERE email = 'emily.wilson@email.com')
                               AND scheduled_date = DATE '2025-11-17'
                               AND scheduled_time = '11:00')),
        'Glucose: 95, Creatinine: 0.9, K: 4.2, Na: 140', 
        DATE '2025-11-17', 'Y', 'All within normal limits', 'Healthy metabolic profile', 
        (SELECT doctor_id FROM DOCTORS WHERE license_number = 'MD-IL-2017-9012'));

-- Commit all changes
COMMIT;

-- =====================================================
-- SECTION 2: VERIFICATION QUERIES
-- Verify all data was inserted correctly
-- =====================================================

-- Verify record counts
SELECT 'Record Count Verification' AS verification_type FROM DUAL;

SELECT 
    'CLINIC_INFO' AS table_name, 
    COUNT(*) AS record_count,
    CASE WHEN COUNT(*) = 1 THEN 'PASS' ELSE 'FAIL' END AS status
FROM CLINIC_INFO
UNION ALL
SELECT 'PATIENTS', COUNT(*), CASE WHEN COUNT(*) = 5 THEN 'PASS' ELSE 'FAIL' END FROM PATIENTS
UNION ALL
SELECT 'STAFF', COUNT(*), CASE WHEN COUNT(*) = 8 THEN 'PASS' ELSE 'FAIL' END FROM STAFF
UNION ALL
SELECT 'DOCTORS', COUNT(*), CASE WHEN COUNT(*) = 3 THEN 'PASS' ELSE 'FAIL' END FROM DOCTORS
UNION ALL
SELECT 'APPOINTMENTS', COUNT(*), CASE WHEN COUNT(*) = 5 THEN 'PASS' ELSE 'FAIL' END FROM APPOINTMENTS
UNION ALL
SELECT 'ASSESSMENTS', COUNT(*), CASE WHEN COUNT(*) = 5 THEN 'PASS' ELSE 'FAIL' END FROM ASSESSMENTS
UNION ALL
SELECT 'DIAGNOSTIC_TESTS', COUNT(*), CASE WHEN COUNT(*) = 5 THEN 'PASS' ELSE 'FAIL' END FROM DIAGNOSTIC_TESTS
UNION ALL
SELECT 'TEST_RESULTS', COUNT(*), CASE WHEN COUNT(*) = 5 THEN 'PASS' ELSE 'FAIL' END FROM TEST_RESULTS
UNION ALL
SELECT 'TOTAL RECORDS', 
       (SELECT COUNT(*) FROM CLINIC_INFO) +
       (SELECT COUNT(*) FROM PATIENTS) +
       (SELECT COUNT(*) FROM STAFF) +
       (SELECT COUNT(*) FROM DOCTORS) +
       (SELECT COUNT(*) FROM APPOINTMENTS) +
       (SELECT COUNT(*) FROM ASSESSMENTS) +
       (SELECT COUNT(*) FROM DIAGNOSTIC_TESTS) +
       (SELECT COUNT(*) FROM TEST_RESULTS),
       CASE WHEN (SELECT COUNT(*) FROM CLINIC_INFO) +
                 (SELECT COUNT(*) FROM PATIENTS) +
                 (SELECT COUNT(*) FROM STAFF) +
                 (SELECT COUNT(*) FROM DOCTORS) +
                 (SELECT COUNT(*) FROM APPOINTMENTS) +
                 (SELECT COUNT(*) FROM ASSESSMENTS) +
                 (SELECT COUNT(*) FROM DIAGNOSTIC_TESTS) +
                 (SELECT COUNT(*) FROM TEST_RESULTS) = 37 THEN 'PASS' ELSE 'FAIL' END
FROM DUAL;

-- Verify business rule: Only doctors in DOCTORS table
SELECT 'Business Rule Verification: Only Doctors in DOCTORS table' AS verification_type FROM DUAL;

SELECT 
    s.staff_id,
    s.first_name || ' ' || s.last_name AS name,
    s.role,
    CASE 
        WHEN d.doctor_id IS NOT NULL AND s.role = 'Doctor' THEN 'PASS'
        WHEN d.doctor_id IS NULL AND s.role != 'Doctor' THEN 'PASS'
        ELSE 'FAIL'
    END AS status
FROM STAFF s
LEFT JOIN DOCTORS d ON s.staff_id = d.doctor_id
ORDER BY s.role, s.staff_id;

-- Verify foreign key integrity
SELECT 'Foreign Key Integrity Check' AS verification_type FROM DUAL;

SELECT 
    'STAFF.clinic_id' AS foreign_key,
    COUNT(*) AS valid_references,
    CASE WHEN COUNT(*) = (SELECT COUNT(*) FROM STAFF) THEN 'PASS' ELSE 'FAIL' END AS status
FROM STAFF s
WHERE EXISTS (SELECT 1 FROM CLINIC_INFO c WHERE c.clinic_id = s.clinic_id)
UNION ALL
SELECT 
    'DOCTORS.doctor_id',
    COUNT(*),
    CASE WHEN COUNT(*) = (SELECT COUNT(*) FROM DOCTORS) THEN 'PASS' ELSE 'FAIL' END
FROM DOCTORS d
WHERE EXISTS (SELECT 1 FROM STAFF s WHERE s.staff_id = d.doctor_id)
UNION ALL
SELECT 
    'APPOINTMENTS.patient_id',
    COUNT(*),
    CASE WHEN COUNT(*) = (SELECT COUNT(*) FROM APPOINTMENTS) THEN 'PASS' ELSE 'FAIL' END
FROM APPOINTMENTS a
WHERE EXISTS (SELECT 1 FROM PATIENTS p WHERE p.patient_id = a.patient_id)
UNION ALL
SELECT 
    'APPOINTMENTS.attending_staff_id',
    COUNT(*),
    CASE WHEN COUNT(*) = (SELECT COUNT(*) FROM APPOINTMENTS) THEN 'PASS' ELSE 'FAIL' END
FROM APPOINTMENTS a
WHERE EXISTS (SELECT 1 FROM STAFF s WHERE s.staff_id = a.attending_staff_id)
UNION ALL
SELECT 
    'DIAGNOSTIC_TESTS.ordering_doctor_id',
    COUNT(*),
    CASE WHEN COUNT(*) = (SELECT COUNT(*) FROM DIAGNOSTIC_TESTS) THEN 'PASS' ELSE 'FAIL' END
FROM DIAGNOSTIC_TESTS dt
WHERE EXISTS (SELECT 1 FROM DOCTORS d WHERE d.doctor_id = dt.ordering_doctor_id)
UNION ALL
SELECT 
    'TEST_RESULTS.reviewing_doctor_id',
    COUNT(*),
    CASE WHEN COUNT(*) = (SELECT COUNT(*) FROM TEST_RESULTS) THEN 'PASS' ELSE 'FAIL' END
FROM TEST_RESULTS tr
WHERE EXISTS (SELECT 1 FROM DOCTORS d WHERE d.doctor_id = tr.reviewing_doctor_id);

-- =====================================================
-- SECTION 3: SELECT STATEMENTS
-- Retrieve all rows and columns from each table
-- =====================================================

SELECT 'Data Retrieval: All Tables' AS section_title FROM DUAL;

-- Select CLINIC_INFO
SELECT 'CLINIC_INFO' AS table_name FROM DUAL;
SELECT * FROM CLINIC_INFO
ORDER BY clinic_id;

-- Select all PATIENTS
SELECT 'PATIENTS' AS table_name FROM DUAL;
SELECT * FROM PATIENTS
ORDER BY patient_id;

-- Select all STAFF
SELECT 'STAFF' AS table_name FROM DUAL;
SELECT * FROM STAFF
ORDER BY staff_id;

-- Select all DOCTORS with their staff info
SELECT 'DOCTORS (with STAFF info)' AS table_name FROM DUAL;
SELECT 
    s.staff_id,
    s.first_name,
    s.last_name,
    s.role,
    s.phone,
    s.email,
    s.hire_date,
    s.clinic_id,
    d.specialty,
    d.license_number,
    d.board_certification,
    d.workplace,
    d.hospital_affiliation,
    d.years_experience
FROM STAFF s
JOIN DOCTORS d ON s.staff_id = d.doctor_id
ORDER BY d.doctor_id;

-- Select all APPOINTMENTS
SELECT 'APPOINTMENTS' AS table_name FROM DUAL;
SELECT * FROM APPOINTMENTS
ORDER BY appointment_id;

-- Select all ASSESSMENTS
SELECT 'ASSESSMENTS' AS table_name FROM DUAL;
SELECT * FROM ASSESSMENTS
ORDER BY assessment_id;

-- Select all DIAGNOSTIC_TESTS
SELECT 'DIAGNOSTIC_TESTS' AS table_name FROM DUAL;
SELECT * FROM DIAGNOSTIC_TESTS
ORDER BY test_id;

-- Select all TEST_RESULTS
SELECT 'TEST_RESULTS' AS table_name FROM DUAL;
SELECT * FROM TEST_RESULTS
ORDER BY result_id;

-- =====================================================
-- SECTION 4: USEFUL QUERIES FOR DEMONSTRATION
-- Show relationships between tables and business rules
-- =====================================================

SELECT 'Demonstration Queries' AS section_title FROM DUAL;

-- Query 1: Patient appointment history with attending staff and clinic
SELECT 'Query 1: Patient Appointment History' AS query_name FROM DUAL;

SELECT 
    c.clinic_name,
    p.first_name || ' ' || p.last_name AS patient_name,
    TO_CHAR(a.scheduled_date, 'DD-MON-YYYY') AS appointment_date,
    a.scheduled_time,
    a.appointment_type,
    s.first_name || ' ' || s.last_name AS attending_staff,
    s.role AS staff_role,
    a.room_number,
    a.status
FROM APPOINTMENTS a
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.attending_staff_id = s.staff_id
JOIN CLINIC_INFO c ON a.clinic_id = c.clinic_id
ORDER BY a.scheduled_date DESC, a.scheduled_time;

-- Query 2: Tests ordered by doctors only (showing business rule)
SELECT 'Query 2: Diagnostic Tests (Doctors Only Can Order)' AS query_name FROM DUAL;

SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    dt.test_name,
    dt.test_type,
    TO_CHAR(dt.ordered_date, 'DD-MON-YYYY') AS order_date,
    ds.first_name || ' ' || ds.last_name AS ordering_doctor,
    doc.specialty AS doctor_specialty,
    doc.workplace,
    dt.priority,
    dt.status,
    ts.first_name || ' ' || ts.last_name AS technician
FROM DIAGNOSTIC_TESTS dt
JOIN APPOINTMENTS a ON dt.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN DOCTORS doc ON dt.ordering_doctor_id = doc.doctor_id
JOIN STAFF ds ON doc.doctor_id = ds.staff_id
LEFT JOIN STAFF ts ON dt.technician_id = ts.staff_id
ORDER BY dt.ordered_date DESC, p.last_name;

-- Query 3: Test results reviewed by doctors only
SELECT 'Query 3: Test Results (Doctors Only Can Review)' AS query_name FROM DUAL;

SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    dt.test_name,
    tr.result_value,
    tr.is_normal,
    TO_CHAR(tr.result_date, 'DD-MON-YYYY') AS result_date,
    ds.first_name || ' ' || ds.last_name AS reviewing_doctor,
    doc.specialty,
    doc.hospital_affiliation
FROM TEST_RESULTS tr
JOIN DIAGNOSTIC_TESTS dt ON tr.test_id = dt.test_id
JOIN APPOINTMENTS a ON dt.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN DOCTORS doc ON tr.reviewing_doctor_id = doc.doctor_id
JOIN STAFF ds ON doc.doctor_id = ds.staff_id
ORDER BY tr.result_date DESC, p.last_name;

-- Query 4: Doctor workload analysis (showing specialization)
SELECT 'Query 4: Doctor Workload Analysis' AS query_name FROM DUAL;

SELECT 
    s.first_name || ' ' || s.last_name AS doctor_name,
    d.specialty,
    d.workplace,
    d.hospital_affiliation,
    d.years_experience,
    COUNT(DISTINCT a.appointment_id) AS appointments_attended,
    COUNT(DISTINCT dt.test_id) AS tests_ordered,
    COUNT(DISTINCT tr.result_id) AS results_reviewed
FROM DOCTORS d
JOIN STAFF s ON d.doctor_id = s.staff_id
LEFT JOIN APPOINTMENTS a ON s.staff_id = a.attending_staff_id
LEFT JOIN DIAGNOSTIC_TESTS dt ON d.doctor_id = dt.ordering_doctor_id
LEFT JOIN TEST_RESULTS tr ON d.doctor_id = tr.reviewing_doctor_id
GROUP BY s.staff_id, s.first_name, s.last_name, d.specialty, d.workplace, d.hospital_affiliation, d.years_experience
ORDER BY tests_ordered DESC, appointments_attended DESC;

-- Query 5: Clinic daily schedule showing mix of staff types
SELECT 'Query 5: Clinic Daily Schedule (All Staff Types)' AS query_name FROM DUAL;

SELECT 
    c.clinic_name,
    TO_CHAR(a.scheduled_date, 'DD-MON-YYYY') AS appointment_date,
    a.scheduled_time,
    p.first_name || ' ' || p.last_name AS patient_name,
    a.appointment_type,
    s.first_name || ' ' || s.last_name AS attending_staff,
    s.role AS staff_type,
    a.room_number,
    a.status,
    ass.severity_level
FROM APPOINTMENTS a
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.attending_staff_id = s.staff_id
JOIN CLINIC_INFO c ON a.clinic_id = c.clinic_id
LEFT JOIN ASSESSMENTS ass ON a.appointment_id = ass.appointment_id
WHERE a.scheduled_date >= DATE '2025-11-01'  -- Current month
ORDER BY a.scheduled_date, a.scheduled_time;

-- Query 6: Assessment summary with severity levels and diagnoses
SELECT 'Query 6: Assessment Summary by Severity' AS query_name FROM DUAL;

SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    TO_CHAR(a.scheduled_date, 'DD-MON-YYYY') AS visit_date,
    ass.chief_complaint,
    ass.preliminary_diagnosis,
    ass.severity_level,
    CASE ass.severity_level
        WHEN 1 THEN 'Minimal'
        WHEN 2 THEN 'Mild'
        WHEN 3 THEN 'Moderate'
        WHEN 4 THEN 'Severe'
        WHEN 5 THEN 'Critical'
    END AS severity_description,
    s.first_name || ' ' || s.last_name AS attending_staff,
    s.role
FROM ASSESSMENTS ass
JOIN APPOINTMENTS a ON ass.appointment_id = a.appointment_id
JOIN PATIENTS p ON a.patient_id = p.patient_id
JOIN STAFF s ON a.attending_staff_id = s.staff_id
ORDER BY ass.severity_level DESC, a.scheduled_date DESC;

-- Query 7: Staff roster by clinic and role
SELECT 'Query 7: Staff Roster by Clinic and Role' AS query_name FROM DUAL;

SELECT 
    c.clinic_name,
    s.role,
    s.first_name || ' ' || s.last_name AS staff_name,
    s.email,
    s.phone,
    TO_CHAR(s.hire_date, 'DD-MON-YYYY') AS hire_date,
    CASE 
        WHEN d.doctor_id IS NOT NULL THEN d.specialty
        ELSE 'N/A'
    END AS specialty
FROM STAFF s
JOIN CLINIC_INFO c ON s.clinic_id = c.clinic_id
LEFT JOIN DOCTORS d ON s.staff_id = d.doctor_id
ORDER BY c.clinic_name, s.role, s.last_name;

-- Query 8: Patient medical history summary
SELECT 'Query 8: Patient Medical History Summary' AS query_name FROM DUAL;

SELECT 
    p.first_name || ' ' || p.last_name AS patient_name,
    p.date_of_birth,
    FLOOR(MONTHS_BETWEEN(SYSDATE, p.date_of_birth) / 12) AS age,
    p.gender,
    p.insurance_provider,
    COUNT(DISTINCT a.appointment_id) AS total_visits,
    COUNT(DISTINCT dt.test_id) AS total_tests,
    MAX(a.scheduled_date) AS last_visit
FROM PATIENTS p
LEFT JOIN APPOINTMENTS a ON p.patient_id = a.patient_id
LEFT JOIN DIAGNOSTIC_TESTS dt ON a.appointment_id = dt.appointment_id
GROUP BY p.patient_id, p.first_name, p.last_name, p.date_of_birth, p.gender, p.insurance_provider
ORDER BY p.last_name, p.first_name;

-- =====================================================
-- END OF DML SCRIPT
-- Total Records Inserted: 37
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

## Execution Instructions for DataGrip:

### **Step 1: Run DDL Script First**
Make sure your DDL script has been executed successfully and all tables are created.

### **Step 2: Execute DML Script**
1. Open this DML script in DataGrip
2. Select all (Ctrl+A)
3. Execute (Ctrl+Enter or click green arrow)
4. Watch for verification queries at the end
