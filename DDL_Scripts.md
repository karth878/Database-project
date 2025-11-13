# DDL Scripts - MedFirst Diagnostic Center
## Data Definition Language for Oracle Database

```sql
-- =====================================================
-- DDL SCRIPT FOR MEDIRST DIAGNOSTIC CENTER DATABASE
-- Author: Database Designers Inc.
-- Date: 2024
-- Database: Oracle 19c or higher
-- =====================================================

-- Drop existing tables if they exist (for clean rebuild)
-- Note: Comment these out for first-time creation
/*
DROP TABLE TEST_RESULTS CASCADE CONSTRAINTS;
DROP TABLE DIAGNOSTIC_TESTS CASCADE CONSTRAINTS;
DROP TABLE ASSESSMENTS CASCADE CONSTRAINTS;
DROP TABLE APPOINTMENTS CASCADE CONSTRAINTS;
DROP TABLE STAFF CASCADE CONSTRAINTS;
DROP TABLE PATIENTS CASCADE CONSTRAINTS;
*/

-- =====================================================
-- CREATE SEQUENCE OBJECTS FOR AUTO-INCREMENT IDs
-- =====================================================

CREATE SEQUENCE patient_seq START WITH 1000 INCREMENT BY 1;
CREATE SEQUENCE staff_seq START WITH 100 INCREMENT BY 1;
CREATE SEQUENCE appointment_seq START WITH 5000 INCREMENT BY 1;
CREATE SEQUENCE assessment_seq START WITH 5000 INCREMENT BY 1;
CREATE SEQUENCE test_seq START WITH 10000 INCREMENT BY 1;
CREATE SEQUENCE result_seq START WITH 10000 INCREMENT BY 1;

-- =====================================================
-- TABLE 1: PATIENTS
-- Stores patient demographic and insurance information
-- =====================================================

CREATE TABLE PATIENTS (
    patient_id         NUMBER(10)      NOT NULL,
    first_name        VARCHAR2(50)    NOT NULL,
    last_name         VARCHAR2(50)    NOT NULL,
    date_of_birth     DATE            NOT NULL,
    gender            CHAR(1)         DEFAULT 'O',
    phone             VARCHAR2(15)    NOT NULL,
    email             VARCHAR2(100),
    street_address    VARCHAR2(100),
    city              VARCHAR2(50),
    state             CHAR(2),
    zip_code          VARCHAR2(10),
    insurance_provider VARCHAR2(100),
    insurance_number  VARCHAR2(50),
    -- Constraints
    CONSTRAINT pk_patients PRIMARY KEY (patient_id),
    CONSTRAINT uq_patient_email UNIQUE (email),
    CONSTRAINT chk_gender CHECK (gender IN ('M', 'F', 'O')),
    CONSTRAINT chk_state CHECK (REGEXP_LIKE(state, '^[A-Z]{2}$')),
    CONSTRAINT chk_phone CHECK (REGEXP_LIKE(phone, '^[0-9-() ]+$'))
);

-- Create indexes for PATIENTS
CREATE INDEX idx_patient_name ON PATIENTS(last_name, first_name);
CREATE INDEX idx_patient_dob ON PATIENTS(date_of_birth);

-- =====================================================
-- TABLE 2: STAFF
-- Stores all staff members (doctors, nurses, technicians)
-- =====================================================

CREATE TABLE STAFF (
    staff_id          NUMBER(10)      NOT NULL,
    first_name        VARCHAR2(50)    NOT NULL,
    last_name         VARCHAR2(50)    NOT NULL,
    role              VARCHAR2(30)    NOT NULL,
    specialty         VARCHAR2(50),
    phone             VARCHAR2(15)    NOT NULL,
    email             VARCHAR2(100)   NOT NULL,
    license_number    VARCHAR2(30),
    hire_date         DATE            DEFAULT SYSDATE NOT NULL,
    -- Constraints
    CONSTRAINT pk_staff PRIMARY KEY (staff_id),
    CONSTRAINT uq_staff_email UNIQUE (email),
    CONSTRAINT uq_license UNIQUE (license_number),
    CONSTRAINT chk_role CHECK (role IN ('Doctor', 'Nurse', 'Technician', 'Admin')),
    CONSTRAINT chk_staff_phone CHECK (REGEXP_LIKE(phone, '^[0-9-() ]+$'))
);

-- Create indexes for STAFF
CREATE INDEX idx_staff_name ON STAFF(last_name, first_name);
CREATE INDEX idx_staff_role ON STAFF(role);

-- =====================================================
-- TABLE 3: APPOINTMENTS
-- Tracks all patient appointments and visits
-- =====================================================

CREATE TABLE APPOINTMENTS (
    appointment_id    NUMBER(10)      NOT NULL,
    patient_id        NUMBER(10)      NOT NULL,
    scheduled_date    DATE            NOT NULL,
    scheduled_time    VARCHAR2(5)     NOT NULL,
    appointment_type  VARCHAR2(30)    DEFAULT 'Scheduled',
    status            VARCHAR2(20)    DEFAULT 'Scheduled',
    doctor_id         NUMBER(10)      NOT NULL,
    room_number       VARCHAR2(10),
    -- Constraints
    CONSTRAINT pk_appointments PRIMARY KEY (appointment_id),
    CONSTRAINT fk_appt_patient FOREIGN KEY (patient_id) 
        REFERENCES PATIENTS(patient_id),
    CONSTRAINT fk_appt_doctor FOREIGN KEY (doctor_id) 
        REFERENCES STAFF(staff_id),
    CONSTRAINT chk_appt_type CHECK (appointment_type IN ('Walk-in', 'Scheduled', 'Emergency')),
    CONSTRAINT chk_appt_status CHECK (status IN ('Scheduled', 'Completed', 'Cancelled', 'No-Show')),
    CONSTRAINT chk_time_format CHECK (REGEXP_LIKE(scheduled_time, '^([01][0-9]|2[0-3]):[0-5][0-9]$'))
);

-- Create indexes for APPOINTMENTS
CREATE INDEX idx_appt_patient ON APPOINTMENTS(patient_id);
CREATE INDEX idx_appt_date ON APPOINTMENTS(scheduled_date, scheduled_time);
CREATE INDEX idx_appt_doctor ON APPOINTMENTS(doctor_id);

-- =====================================================
-- TABLE 4: ASSESSMENTS
-- Medical assessment details for each appointment
-- =====================================================

CREATE TABLE ASSESSMENTS (
    assessment_id         NUMBER(10)      NOT NULL,
    appointment_id        NUMBER(10)      NOT NULL,
    chief_complaint       VARCHAR2(500)   NOT NULL,
    vital_signs          VARCHAR2(200),
    blood_pressure       VARCHAR2(10),
    temperature          NUMBER(4,1),
    pulse                NUMBER(3),
    assessment_notes     CLOB,
    preliminary_diagnosis VARCHAR2(200),
    severity_level       NUMBER(1)        DEFAULT 3,
    -- Constraints
    CONSTRAINT pk_assessments PRIMARY KEY (assessment_id),
    CONSTRAINT fk_assess_appt FOREIGN KEY (appointment_id) 
        REFERENCES APPOINTMENTS(appointment_id),
    CONSTRAINT uq_assess_appt UNIQUE (appointment_id),
    CONSTRAINT chk_temperature CHECK (temperature BETWEEN 90.0 AND 110.0),
    CONSTRAINT chk_pulse CHECK (pulse BETWEEN 40 AND 200),
    CONSTRAINT chk_severity CHECK (severity_level BETWEEN 1 AND 5),
    CONSTRAINT chk_bp_format CHECK (REGEXP_LIKE(blood_pressure, '^[0-9]{2,3}/[0-9]{2,3}$') OR blood_pressure IS NULL)
);

-- Create index for ASSESSMENTS
CREATE INDEX idx_assess_appt ON ASSESSMENTS(appointment_id);

-- =====================================================
-- TABLE 5: DIAGNOSTIC_TESTS
-- Tracks all diagnostic tests ordered
-- =====================================================

CREATE TABLE DIAGNOSTIC_TESTS (
    test_id           NUMBER(10)      NOT NULL,
    appointment_id    NUMBER(10)      NOT NULL,
    test_type         VARCHAR2(50)    NOT NULL,
    test_name         VARCHAR2(100)   NOT NULL,
    ordered_date      DATE            DEFAULT SYSDATE NOT NULL,
    performed_date    DATE,
    technician_id     NUMBER(10),
    status            VARCHAR2(20)    DEFAULT 'Ordered',
    priority          VARCHAR2(10)    DEFAULT 'Routine',
    -- Constraints
    CONSTRAINT pk_tests PRIMARY KEY (test_id),
    CONSTRAINT fk_test_appt FOREIGN KEY (appointment_id) 
        REFERENCES APPOINTMENTS(appointment_id),
    CONSTRAINT fk_test_tech FOREIGN KEY (technician_id) 
        REFERENCES STAFF(staff_id),
    CONSTRAINT chk_test_status CHECK (status IN ('Ordered', 'In-Progress', 'Completed', 'Cancelled')),
    CONSTRAINT chk_priority CHECK (priority IN ('Routine', 'Urgent', 'STAT')),
    CONSTRAINT chk_test_dates CHECK (performed_date >= ordered_date OR performed_date IS NULL)
);

-- Create indexes for DIAGNOSTIC_TESTS
CREATE INDEX idx_test_appt ON DIAGNOSTIC_TESTS(appointment_id);
CREATE INDEX idx_test_tech ON DIAGNOSTIC_TESTS(technician_id);
CREATE INDEX idx_test_status ON DIAGNOSTIC_TESTS(status);

-- =====================================================
-- TABLE 6: TEST_RESULTS
-- Stores results from diagnostic tests
-- =====================================================

CREATE TABLE TEST_RESULTS (
    result_id         NUMBER(10)      NOT NULL,
    test_id           NUMBER(10)      NOT NULL,
    result_value      VARCHAR2(500)   NOT NULL,
    result_date       DATE            DEFAULT SYSDATE NOT NULL,
    is_normal         CHAR(1)         DEFAULT 'Y',
    reference_range   VARCHAR2(100),
    notes             VARCHAR2(1000),
    reviewed_by       NUMBER(10)      NOT NULL,
    -- Constraints
    CONSTRAINT pk_results PRIMARY KEY (result_id),
    CONSTRAINT fk_result_test FOREIGN KEY (test_id) 
        REFERENCES DIAGNOSTIC_TESTS(test_id),
    CONSTRAINT fk_result_reviewer FOREIGN KEY (reviewed_by) 
        REFERENCES STAFF(staff_id),
    CONSTRAINT uq_test_result UNIQUE (test_id),
    CONSTRAINT chk_is_normal CHECK (is_normal IN ('Y', 'N'))
);

-- Create indexes for TEST_RESULTS
CREATE INDEX idx_result_test ON TEST_RESULTS(test_id);
CREATE INDEX idx_result_reviewer ON TEST_RESULTS(reviewed_by);
CREATE INDEX idx_result_abnormal ON TEST_RESULTS(is_normal);

-- =====================================================
-- CREATE TRIGGERS FOR AUTO-INCREMENT (Optional)
-- Use if not using sequences manually in INSERT
-- =====================================================

CREATE OR REPLACE TRIGGER patient_id_trigger
BEFORE INSERT ON PATIENTS
FOR EACH ROW
BEGIN
    IF :NEW.patient_id IS NULL THEN
        SELECT patient_seq.NEXTVAL INTO :NEW.patient_id FROM dual;
    END IF;
END;
/

CREATE OR REPLACE TRIGGER staff_id_trigger
BEFORE INSERT ON STAFF
FOR EACH ROW
BEGIN
    IF :NEW.staff_id IS NULL THEN
        SELECT staff_seq.NEXTVAL INTO :NEW.staff_id FROM dual;
    END IF;
END;
/

CREATE OR REPLACE TRIGGER appointment_id_trigger
BEFORE INSERT ON APPOINTMENTS
FOR EACH ROW
BEGIN
    IF :NEW.appointment_id IS NULL THEN
        SELECT appointment_seq.NEXTVAL INTO :NEW.appointment_id FROM dual;
    END IF;
END;
/

CREATE OR REPLACE TRIGGER assessment_id_trigger
BEFORE INSERT ON ASSESSMENTS
FOR EACH ROW
BEGIN
    IF :NEW.assessment_id IS NULL THEN
        SELECT assessment_seq.NEXTVAL INTO :NEW.assessment_id FROM dual;
    END IF;
END;
/

CREATE OR REPLACE TRIGGER test_id_trigger
BEFORE INSERT ON DIAGNOSTIC_TESTS
FOR EACH ROW
BEGIN
    IF :NEW.test_id IS NULL THEN
        SELECT test_seq.NEXTVAL INTO :NEW.test_id FROM dual;
    END IF;
END;
/

CREATE OR REPLACE TRIGGER result_id_trigger
BEFORE INSERT ON TEST_RESULTS
FOR EACH ROW
BEGIN
    IF :NEW.result_id IS NULL THEN
        SELECT result_seq.NEXTVAL INTO :NEW.result_id FROM dual;
    END IF;
END;
/

-- =====================================================
-- GRANT PERMISSIONS (Adjust based on user roles)
-- =====================================================

-- Example permissions for application user
-- GRANT SELECT, INSERT, UPDATE ON PATIENTS TO app_user;
-- GRANT SELECT, INSERT, UPDATE ON APPOINTMENTS TO app_user;
-- GRANT SELECT ON STAFF TO app_user;

-- =====================================================
-- END OF DDL SCRIPT
-- Total Tables Created: 6
-- Total Indexes Created: 14
-- Total Sequences Created: 6
-- Total Triggers Created: 6
-- =====================================================

-- Verify tables were created successfully
SELECT table_name FROM user_tables 
WHERE table_name IN ('PATIENTS','STAFF','APPOINTMENTS',
                     'ASSESSMENTS','DIAGNOSTIC_TESTS','TEST_RESULTS');

-- Verify constraints
SELECT constraint_name, constraint_type, table_name 
FROM user_constraints 
WHERE table_name IN ('PATIENTS','STAFF','APPOINTMENTS',
                     'ASSESSMENTS','DIAGNOSTIC_TESTS','TEST_RESULTS')
ORDER BY table_name, constraint_type;
```

---

## Notes for DataGrip Usage:

1. **Copy and paste** the entire script into DataGrip's SQL editor
2. **Execute** the script (Ctrl+Enter or click the green arrow)
3. **Check for errors** in the console output
4. If you get "table already exists" errors, uncomment the DROP statements at the beginning
5. The script includes triggers for auto-increment - you can insert without specifying IDs
6. The verification queries at the end confirm successful creation

## Key Features of This DDL:

- **Sequences** for auto-incrementing primary keys
- **Check constraints** for data validation (gender, phone format, etc.)
- **Foreign key constraints** to maintain referential integrity
- **Unique constraints** on emails and license numbers
- **Indexes** on commonly queried columns for performance
- **Triggers** for automatic ID assignment
- **Regular expressions** for format validation (phone, time, blood pressure)
