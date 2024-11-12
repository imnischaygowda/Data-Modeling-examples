
Here is data miodel example for Hospital cocnisting of Paient, medical procedures, doctors and more. 

**1. Core Entities**
Common entities in healthcare databases include:

    •	Patients 
    •	Providers/Staff
    •	Encounters/Visits
    •	Diagnoses
    •	Procedures
    •	Medications
    •	Lab Results
    •	Insurance/Billing


**2. Attributes for each entity**
Defining attribiutes for each entites and thier data types. 

```sql
  -- Patients table
  CREATE TABLE Patients (
    patient_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    date_of_birth DATE NOT NULL,
    gender VARCHAR(10),
    address TEXT,
    phone VARCHAR(20),
    email VARCHAR(100),
    emergency_contact VARCHAR(100),
    emergency_contact_phone VARCHAR(20)
  );

  
  -- Providers/Staff table
  CREATE TABLE Providers (
    provider_id INT PRIMARY KEY,
    first_name VARCHAR(50) NOT NULL,
    last_name VARCHAR(50) NOT NULL,
    specialization VARCHAR(100),
    license_number VARCHAR(50),
    phone VARCHAR(20),
    email VARCHAR(100),
    hire_date DATE
  );
  
  -- Encounters/Visits table
  CREATE TABLE Encounters (
    encounter_id INT PRIMARY KEY,
    patient_id INT,
    provider_id INT,
    encounter_date DATETIME NOT NULL,
    encounter_type VARCHAR(50),
    reason_for_visit TEXT,
    notes TEXT,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (provider_id) REFERENCES Providers(provider_id)
  );
  
  -- Diagnoses table
  CREATE TABLE Diagnoses (
    diagnosis_id INT PRIMARY KEY,
    encounter_id INT,
    icd_code VARCHAR(20) NOT NULL,
    description TEXT,
    diagnosis_date DATE,
    FOREIGN KEY (encounter_id) REFERENCES Encounters(encounter_id)
  );
  
  -- Procedures table
  CREATE TABLE Procedures (
    procedure_id INT PRIMARY KEY,
    encounter_id INT,
    cpt_code VARCHAR(20) NOT NULL,
    description TEXT,
    procedure_date DATE,
    provider_id INT,
    FOREIGN KEY (encounter_id) REFERENCES Encounters(encounter_id),
    FOREIGN KEY (provider_id) REFERENCES Providers(provider_id)
  );
  
  -- Medications table
  CREATE TABLE Medications (
    medication_id INT PRIMARY KEY,
    patient_id INT,
    drug_name VARCHAR(100) NOT NULL,
    dosage VARCHAR(50),
    frequency VARCHAR(50),
    start_date DATE,
    end_date DATE,
    prescribing_provider_id INT,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (prescribing_provider_id) REFERENCES Providers(provider_id)
  );
  
  -- Lab Results table
  CREATE TABLE LabResults (
    lab_result_id INT PRIMARY KEY,
    patient_id INT,
    test_name VARCHAR(100) NOT NULL,
    result VARCHAR(100),
    unit VARCHAR(20),
    reference_range VARCHAR(50),
    test_date DATE,
    ordering_provider_id INT,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (ordering_provider_id) REFERENCES Providers(provider_id)
  );
  
  -- Insurance/Billing table
  CREATE TABLE Insurance (
    insurance_id INT PRIMARY KEY,
    patient_id INT,
    insurance_provider VARCHAR(100) NOT NULL,
    policy_number VARCHAR(50),
    group_number VARCHAR(50),
    coverage_start_date DATE,
    coverage_end_date DATE,
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id)
  );

  -- Insurance/Billing table
  CREATE TABLE Billing (
    billing_id INT PRIMARY KEY,
    encounter_id INT,
    patient_id INT,
    insurance_id INT,
    total_amount DECIMAL(10, 2),
    billed_date DATE,
    paid_amount DECIMAL(10, 2),
    payment_date DATE,
    status VARCHAR(20),
    FOREIGN KEY (encounter_id) REFERENCES Encounters(encounter_id),
    FOREIGN KEY (patient_id) REFERENCES Patients(patient_id),
    FOREIGN KEY (insurance_id) REFERENCES Insurance(insurance_id)
  );
  ```



  **3. Physichal Modeling** - now we dfine the relationship for the data access.

  Here are things to be considered.

  - Patients and Encounters - connect patient with the hospital.
    
      • A patient can have multiple encounters, but each encounter is associated with only one patient.
    
      •	This is represented by the **patient_id foreign key** in the Encounters table.
    
  - Providers and Encounters - hospital staff and encounter.
    
      •	A provider can be involved in multiple encounters, and an encounter can involve one primary provider.
    
      •	This is represented by the **provider_id foreign** key in the Encounters table.

  -  Encounters and Diagnoses:
    
       •	An encounter can have multiple diagnoses, but each diagnosis is associated with one encounter.
     
       • This is represented by the **encounter_id foreign key** in the Diagnoses table.
    
  - Encounters and Procedures:
    
      •	An encounter can involve multiple procedures, and each procedure is associated with one encounter.
    
      •	This is represented by the **encounter_id foreign key** in the Procedures table.

  - Patients and Medications:
    
      •	A patient can have multiple medications, but each medication record is associated with one patient.
    
      •	This is represented by the **patient_id foreign key** in the Medications table.
    
  -	Providers and Medications:
    
      •	A provider can prescribe multiple medications, and each medication is prescribed by one provider.
   	
      •	This is represented by the **prescribing_provider_id foreign key** in the Medications table.

  -	Patients and Lab Results:
    
      •	A patient can have multiple lab results, but each lab result is associated with one patient.
   	
      •	This is represented by the **patient_id foreign key** in the LabResults table.

  -	Providers and Lab Results:

      •	A provider can order multiple lab tests, and each lab result is ordered by one provider.
   	
      •	This is represented by the **ordering_provider_id foreign key** in the LabResults table.

  -	Patients and Insurance:
    
      •	A patient can have multiple insurance policies, but each insurance record is associated with one patient.
   	
      •	This is represented by the **patient_id foreign key** in the Insurance table.
  
  -	Encounters and Billing:
    
      •	Each encounter can have one billing record, and each billing record is associated with one encounter.
   	
      •	This is represented by the **encounter_id foreign key** in the Billing table.

  -	Insurance and Billing:
    
      •	An insurance policy can be associated with multiple billing records, but each billing record is typically associated with one insurance policy.
   	
      •	This is represented by the **insurance_id foreign key** in the Billing table.



  ```
  Patients
    ├── Encounters (1:N)
    │     ├── Diagnoses (1:N)
    │     ├── Procedures (1:N)
    │     └── Billing (1:1)
    ├── Medications (1:N)
    ├── LabResults (1:N)
    └── Insurance (1:N)
          └── Billing (1:N)
  
  Providers
    ├── Encounters (1:N)
    ├── Procedures (1:N)
    ├── Medications (1:N)
    └── LabResults (1:N)
  ```


