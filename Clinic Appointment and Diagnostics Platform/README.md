# Clinic Appointment and Diagnostics Platform Database Design

![Clinic Appointment and Diagnostics Platform database diagram](Clinic%20Appointment%20and%20Diagnostics%20Platform.png)

This repository contains the database schema for a clinic platform that manages patients, doctors, appointments, consultations, diagnostic tests, reports, and payments.

The design supports a patient journey from booking an appointment, through consultation and prescribed testing, to receiving reports and making payments.

## Core Entities

### User

Stores shared profile information for both patients and doctors.

### Patient

Contains patient-specific details such as date of birth and past medical records.

### Doctor

Contains doctor-specific details such as department, specialization, availability, and clinic assignment.

### Clinic

Represents a physical clinic location where doctors practice.

### Appointment

Captures a scheduled visit between a patient and a doctor.

### Consultation

Stores the clinical outcome of an appointment, including diagnosis and prescription.

### Diagnostic Test

Stores tests prescribed during a consultation, along with their status and results.

### Report

Stores the final report linked to a patient and a diagnostic test.

### Payment

Tracks payments made by patients for clinic services.

## Table Summary

| Table           | Purpose                                      |
| --------------- | -------------------------------------------- |
| user            | Common profile for all platform users        |
| patient         | Patient-specific attributes                  |
| doctor          | Doctor-specific attributes                   |
| clinic          | Clinic information                           |
| appointment     | Booking between patient and doctor           |
| consultation    | Consultation details after an appointment    |
| diagnostic_test | Tests prescribed during a consultation       |
| report          | Diagnostic report linked to patient and test |
| payment         | Payment details for a patient                |

## Relationships

- A user can be associated with either a patient or a doctor profile.
- A doctor belongs to a clinic.
- A patient can book multiple appointments.
- A doctor can handle multiple appointments.
- Each appointment can lead to one consultation.
- A consultation can generate multiple diagnostic tests.
- A diagnostic test can generate a report.
- A patient can make multiple payments.

## Workflow

1. A patient registers in the system.
2. The patient books an appointment with a doctor.
3. The doctor conducts the consultation.
4. The doctor may prescribe one or more diagnostic tests.
5. Diagnostic tests are performed and results are recorded.
6. Reports are generated and linked back to the patient.
7. Payment records are stored for the services used.

## Notes

- The schema currently uses a shared user table for both patients and doctors.
- The design allows a patient to visit the clinic multiple times through repeated appointments.
- Some field names and enum values in the schema may need cleanup before implementation, such as spelling consistency and naming consistency.

## Suggested Improvements

- Standardize naming conventions across all tables and columns.
- Add stronger constraints for email, phone, and status fields.
- Define primary keys, foreign keys, and indexes explicitly in the SQL implementation.
- Consider adding audit fields such as created_by and updated_by if required.

## Scope

This document describes the database design only. It does not include application logic, APIs, or user interface details.
