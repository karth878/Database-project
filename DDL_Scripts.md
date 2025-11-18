# DDL Scripts - MedFirst Diagnostic Center

```sql
-- =====================================================
-- DDL SCRIPT FOR MEDFIRST DIAGNOSTIC CENTER DATABASE
-- =====================================================

-- Drop existing tables if they exist (for clean rebuild)
/*
DROP TABLE TEST_RESULTS CASCADE CONSTRAINTS;
DROP TABLE DIAGNOSTIC_TESTS CASCADE CONSTRAINTS;
DROP TABLE ASSESSMENTS CASCADE CONSTRAINTS;
DROP TABLE APPOINTMENTS CASCADE CONSTRAINTS;
DROP TABLE DOCTORS CASCADE CONSTRAINTS;
DROP TABLE STAFF CASCADE CONSTRAINTS;
DROP TABLE PATIENTS CASCADE CONSTRAINTS;
DROP TABLE CLINIC_INFO CASCADE CONSTRAINTS;
*/

-- =====================================================
-- SEQUENCES
-- =====================================================

CREATE SEQUENCE clinic_seq START WITH 1 INCREMENT BY 1;
CREATE SEQUENCE patient_seq START WITH 1000 INCREMENT BY 1;
CREATE SEQUENCE staff_seq START WITH 100 INCREMENT BY 1;
CREATE SEQUENCE appointment_seq START WITH 5000 INCREMENT BY 1;
CREATE SEQUENCE assessment_seq START WITH 5000 INCREMENT BY 1;
CREATE SEQUENCE test_seq START WITH 10000 INCREMENT BY 1;
CREATE SEQUENCE result_seq START WITH 10000 INCREMENT BY 1;

-- =====================================================
-- CLINIC_INFO
-- =====================================================

CREATE TABLE CLINIC_INFO (
    clinic_id          NUMBER(10)      NOT NULL,
    clinic_name        VARCHAR2(100)   NOT NULL,
    street_address     VARCHAR2(100)   NOT NULL,
    city               VARCHAR2(50)    NOT NULL,
    state              CHAR(2)         NOT NULL,
    zip_code           VARCHAR2(10)    NOT NULL,
    phone              VARCHAR2(15)    NOT NULL,
    email              VARCHAR2(100)   NOT NULL,
    fax                VARCHAR2(15),
    operating_hours    VARCHAR2(100)   DEFAULT 'Mon-Fri: 8AM-8PM, Sat: 9AM-5PM',
    emergency_contact  VARCHAR2(15),
    -- Constraints
    CONSTRAINT pk_clinic PRIMARY KEY (clinic_id),
    CONSTRAINT uq_clinic_email UNIQUE (email),
    CONSTRAINT chk_clinic_state CHECK (REGEXP_LIKE(state, '^[A-Z]{2}$')),
    CONSTRAINT chk_clinic_phone CHECK (REGEXP_LIKE(phone, '^[0-9-() ]+$'))
);

-- =====================================================
-- PATIENTS
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
    CONSTRAINT chk_patient_state CHECK (REGEXP_LIKE(state, '^[A-Z]{2}$') OR state IS NULL),
    CONSTRAINT chk_patient_phone CHECK (REGEXP_LIKE(phone, '^[0-9-() ]+$'))
);

CREATE INDEX idx_patient_name ON PATIENTS(last_name, first_name);
CREATE INDEX idx_patient_dob ON PATIENTS(date_of_birth);

-- =====================================================
-- STAFF
-- =====================================================

CREATE TABLE STAFF (
    staff_id          NUMBER(10)      NOT NULL,
    first_name        VARCHAR2(50)    NOT NULL,
    last_name         VARCHAR2(50)    NOT NULL,
    role              VARCHAR2(30)    NOT NULL,
    phone             VARCHAR2(15)    NOT NULL,
    email             VARCHAR2(100)   NOT NULL,
    hire_date         DATE            DEFAULT SYSDATE NOT NULL,
    clinic_id         NUMBER(10)      NOT NULL,
    -- Constraints
    CONSTRAINT pk_staff PRIMARY KEY (staff_id),
    CONSTRAINT fk_staff_clinic FOREIGN KEY (clinic_id)
        REFERENCES CLINIC_INFO(clinic_id),
    CONSTRAINT uq_staff_email UNIQUE (email),
    CONSTRAINT chk_role CHECK (role IN ('Doctor', 'Nurse', 'Technician', 'Admin')),
    CONSTRAINT chk_staff_phone CHECK (REGEXP_LIKE(phone, '^[0-9-() ]+$'))
);

CREATE INDEX idx_staff_name ON STAFF(last_name, first_name);
CREATE INDEX idx_staff_role ON STAFF(role);
CREATE INDEX idx_staff_clinic ON STAFF(clinic_id, role);

-- =====================================================
-- DOCTORS
-- =====================================================

CREATE TABLE DOCTORS (
    doctor_id             NUMBER(10)      NOT NULL,
    specialty             VARCHAR2(50)    NOT NULL,
    license_number        VARCHAR2(30)    NOT NULL,
    board_certification   VARCHAR2(100),
    workplace             VARCHAR2(100)   DEFAULT 'MedFirst Diagnostic Center',
    hospital_affiliation  VARCHAR2(100),
    years_experience      NUMBER(3)       DEFAULT 0,
    -- Constraints
    CONSTRAINT pk_doctors PRIMARY KEY (doctor_id),
    CONSTRAINT fk_doctor_staff FOREIGN KEY (doctor_id)
        REFERENCES STAFF(staff_id),
    CONSTRAINT uq_license UNIQUE (license_number),
    CONSTRAINT chk_experience CHECK (years_experience >= 0 AND years_experience <= 100)
);

CREATE INDEX idx_doctor_specialty ON DOCTORS(specialty);
CREATE INDEX idx_doctor_workplace ON DOCTORS(workplace);

-- =====================================================
-- APPOINTMENTS
-- =====================================================

CREATE TABLE APPOINTMENTS (
    appointment_id       NUMBER(10)      NOT NULL,
    patient_id           NUMBER(10)      NOT NULL,
    scheduled_date       DATE            NOT NULL,
    scheduled_time       VARCHAR2(5)     NOT NULL,
    appointment_type     VARCHAR2(30)    DEFAULT 'Scheduled',
    status               VARCHAR2(20)    DEFAULT 'Scheduled',
    attending_staff_id   NUMBER(10)      NOT NULL,
    clinic_id            NUMBER(10)      NOT NULL,
    room_number          VARCHAR2(10),
    -- Constraints
    CONSTRAINT pk_appointments PRIMARY KEY (appointment_id),
    CONSTRAINT fk_appt_patient FOREIGN KEY (patient_id) 
        REFERENCES PATIENTS(patient_id),
    CONSTRAINT fk_appt_staff FOREIGN KEY (attending_staff_id) 
        REFERENCES STAFF(staff_id),
    CONSTRAINT fk_appt_clinic FOREIGN KEY (clinic_id)
        REFERENCES CLINIC_INFO(clinic_id),
    CONSTRAINT chk_appt_type CHECK (appointment_type IN ('Walk-in', 'Scheduled', 'Emergency')),
    CONSTRAINT chk_appt_status CHECK (status IN ('Scheduled', 'Completed', 'Cancelled', 'No-Show')),
    CONSTRAINT chk_time_format CHECK (REGEXP_LIKE(scheduled_time, '^([01][0-9]|2[0-3]):[0-5][0-9]$'))
);

CREATE INDEX idx_appt_patient ON APPOINTMENTS(patient_id);
CREATE INDEX idx_appt_date ON APPOINTMENTS(scheduled_date, scheduled_time);
CREATE INDEX idx_appt_staff ON APPOINTMENTS(attending_staff_id);
CREATE INDEX idx_appt_clinic ON APPOINTMENTS(clinic_id, scheduled_date);

-- =====================================================
-- ASSESSMENTS
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
    CONSTRAINT chk_temperature CHECK (temperature BETWEEN 90.0 AND 110.0 OR temperature IS NULL),
    CONSTRAINT chk_pulse CHECK (pulse BETWEEN 40 AND 200 OR pulse IS NULL),
    CONSTRAINT chk_severity CHECK (severity_level BETWEEN 1 AND 5),
    CONSTRAINT chk_bp_format CHECK (REGEXP_LIKE(blood_pressure, '^[0-9]{2,3}/[0-9]{2,3}$') OR blood_pressure IS NULL)
);

CREATE INDEX idx_assess_appt ON ASSESSMENTS(appointment_id);

-- =====================================================
-- DIAGNOSTIC_TESTS
-- =====================================================

CREATE TABLE DIAGNOSTIC_TESTS (
    test_id              NUMBER(10)      NOT NULL,
    appointment_id       NUMBER(10)      NOT NULL,
    test_type            VARCHAR2(50)    NOT NULL,
    test_name            VARCHAR2(100)   NOT NULL,
    ordered_date         DATE            DEFAULT SYSDATE NOT NULL,
    performed_date       DATE,
    ordering_doctor_id   NUMBER(10)      NOT NULL, -- Must be a doctor
    technician_id        NUMBER(10),     -- Can be any staff member
    status               VARCHAR2(20)    DEFAULT 'Ordered',
    priority             VARCHAR2(10)    DEFAULT 'Routine',
    -- Constraints
    CONSTRAINT pk_tests PRIMARY KEY (test_id),
    CONSTRAINT fk_test_appt FOREIGN KEY (appointment_id) 
        REFERENCES APPOINTMENTS(appointment_id),
    CONSTRAINT fk_test_doctor FOREIGN KEY (ordering_doctor_id) 
        REFERENCES DOCTORS(doctor_id),  -- Only doctors can order
    CONSTRAINT fk_test_tech FOREIGN KEY (technician_id) 
        REFERENCES STAFF(staff_id),     -- Any staff can perform
    CONSTRAINT chk_test_status CHECK (status IN ('Ordered', 'In-Progress', 'Completed', 'Cancelled')),
    CONSTRAINT chk_priority CHECK (priority IN ('Routine', 'Urgent', 'STAT')),
    CONSTRAINT chk_test_dates CHECK (performed_date >= ordered_date OR performed_date IS NULL)
);

CREATE INDEX idx_test_appt ON DIAGNOSTIC_TESTS(appointment_id);
CREATE INDEX idx_test_doctor ON DIAGNOSTIC_TESTS(ordering_doctor_id);
CREATE INDEX idx_test_tech ON DIAGNOSTIC_TESTS(technician_id);
CREATE INDEX idx_test_status ON DIAGNOSTIC_TESTS(status);

-- =====================================================
-- TEST_RESULTS
-- =====================================================

CREATE TABLE TEST_RESULTS (
    result_id            NUMBER(10)      NOT NULL,
    test_id              NUMBER(10)      NOT NULL,
    result_value         VARCHAR2(500)   NOT NULL,
    result_date          DATE            DEFAULT SYSDATE NOT NULL,
    is_normal            CHAR(1)         DEFAULT 'Y',
    reference_range      VARCHAR2(100),
    notes                VARCHAR2(1000),
    reviewing_doctor_id  NUMBER(10)      NOT NULL, -- Must be a doctor
    -- Constraints
    CONSTRAINT pk_results PRIMARY KEY (result_id),
    CONSTRAINT fk_result_test FOREIGN KEY (test_id) 
        REFERENCES DIAGNOSTIC_TESTS(test_id),
    CONSTRAINT fk_result_doctor FOREIGN KEY (reviewing_doctor_id) 
        REFERENCES DOCTORS(doctor_id),  -- Only doctors can review
    CONSTRAINT uq_test_result UNIQUE (test_id),
    CONSTRAINT chk_is_normal CHECK (is_normal IN ('Y', 'N'))
);

CREATE INDEX idx_result_test ON TEST_RESULTS(test_id);
CREATE INDEX idx_result_doctor ON TEST_RESULTS(reviewing_doctor_id);
CREATE INDEX idx_result_abnormal ON TEST_RESULTS(is_normal);

-- =====================================================
-- TRIGGERS
-- =====================================================

CREATE OR REPLACE TRIGGER clinic_id_trigger
BEFORE INSERT ON CLINIC_INFO
FOR EACH ROW
BEGIN
    IF :NEW.clinic_id IS NULL THEN
        SELECT clinic_seq.NEXTVAL INTO :NEW.clinic_id FROM dual;
    END IF;
END;
/

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
-- BUSINESS RULE TRIGGERS
-- =====================================================

CREATE OR REPLACE TRIGGER doctor_role_check
BEFORE INSERT OR UPDATE ON DOCTORS
FOR EACH ROW
DECLARE
    v_role VARCHAR2(30);
BEGIN
    SELECT role INTO v_role
    FROM STAFF
    WHERE staff_id = :NEW.doctor_id;
    
    IF v_role != 'Doctor' THEN
        RAISE_APPLICATION_ERROR(-20001, 
            'Only staff members with role=Doctor can be in DOCTORS table');
    END IF;
END;
/

-- =====================================================
-- VERIFICATION
-- =====================================================

SELECT table_name FROM user_tables 
WHERE table_name IN ('CLINIC_INFO','PATIENTS','STAFF','DOCTORS',
                     'APPOINTMENTS','ASSESSMENTS','DIAGNOSTIC_TESTS','TEST_RESULTS');

SELECT constraint_name, constraint_type, table_name 
FROM user_constraints 
WHERE table_name IN ('CLINIC_INFO','PATIENTS','STAFF','DOCTORS',
                     'APPOINTMENTS','ASSESSMENTS','DIAGNOSTIC_TESTS','TEST_RESULTS')
ORDER BY table_name, constraint_type;
```

