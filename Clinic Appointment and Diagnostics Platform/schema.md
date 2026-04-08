// doctors, patients, consultations, diagnostic tests, reports, payments

// patient - visit doctors, book appointments, undergo tests if prescribed, and receive reports later.

//doctors- departments or specialties, prescription of tests

// reports can be linkde to pateint and doctor

//patient - can visit clinic multiple time

user [icon: user, color: yellow] {
user_id int pk
user_name varchar(50) not null
age int not null
email varchar(322) not null
phone char(15) not null
role ('pateint', 'doctor')
created_at timestamp
updated_at timestamp
}

patient [icon: user-plus, color: green] {
patient_id int pk
user_id fk
dob date
past_records text

}

doctor [icon: doctor, color: blue] {
doctor_id int pk
user_id fk
clinic_id fk
department varchar(100) not null
specialization varchar(100) not null
isAvailable boolean default true
experienece int (in years)
}

clinic [icon: building, color: black ] {
clinic_id int pk
clinic_name varchar(100) not null
address text not null
number_exployees int

}

appointment [icon: document, color: orange] {
appointment_id serial pk
patient_id fk
doctor_id fk
status ('pending', 'fulfilled')
time timestamp
created_at timestamp
updated_at timestamp
}

consultation [icon: user-check, color: green] {
consultation_id serial pk
appointment_id fk
prescription text
diagnosis text
created_at timestamp
updated_at timestamp

}

diagnostic_test [icon: test-tube, color: red] {
diagnosis_tests_id serial pk
consultation_id fk
description text
result text
status ('pending', 'completed')
}

report [icon: report, color: green] {
report_id serial pk
patient_id fk
diagnosis_tests_id fk
file_url string
result_summary text
created_at timestamp
}

payment [icon: payment, color: purple] {
payment_id int pk
patient_id fk
Amount decimal (10, 2)
payment_mode ('netbanking', 'upi', 'cash')
status ('pending', 'failed', 'completed')
created_at timestamp
}

user.user_id - doctor.user_id
user.user_id - patient.user_id

doctor.clinic_id - clinic.clinic_id

patient.patient_id < appointment.patient_id
patient.patient_id < report.patient_id

doctor.doctor_id < appointment.doctor_id

appointment.appointment_id - consultation.appointment_id

consultation.consultation_id < diagnostic_test.consultation_id

patient.patient_id - payment.patient_id

diagnostic_test.diagnosis_tests_id - report.diagnosis_tests_id
