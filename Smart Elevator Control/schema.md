title Smart Elevator Control
// corporate towers, malls, airports, hospitals and high-rise residential complexes

//Each building can contain multiple elevator shafts.

//Each shaft contains one elevator.

//Each elevator moves across a defined set of floors

buildings [icon: building, color: blue] {
building_id serial pk
company_name varchar(100) not null
building_no int
address text
created_at timestamp
updated_at timestamp
}

floors [icon: layers, color: green] {
floor_id serial pk
building_id fk
floor_no int not null
created_at timestamp
}

elevators [icon: share, color: orange] {
elevator_id serial pk
elevator_shaft_id fk
current_floor int not null
direction ('up', 'down', 'idle')

capacity int
created_at timestamp
updated_at timestamp
}

elevator_status [icon: file, color: black] {
elevator_stauts_id serial pk
elevator_id fk
status ('moving','idle', 'maintenance', 'out_of_service')
created_at timestamp
updated_at timestamp
}

elevators.elevator_id - elevator_status.elevator_id

elevator_shafts [icon: share-2] {
elevator_shaft_id serial pk
building_id fk
shaft_number int
created_at timestamp
updated_at timestamp
}

maintenance_check [icon: service, color: red] {
maintenance_check_id serial pk
elevator_id fk
issue_type varchar(100) not null
issue_description text
status ('pending', 'ongoing', 'completed')
scheduled_at timestamp
completed_date timestamp
created_at timestamp
updated_at timestamp
}

elevators.elevator_id < maintenance_check.elevator_id

floorRequest [icon: layers] {
floorRequest_id serial pk
floor_id fk
building_id fk
requested_at timestamp
status ('pending', 'assigned', 'completed')
current_floor int not null
direction ('up', 'down')
destination_floor int

}

floors.floor_id < floorRequest.floor_id

ride_assignments [icon: user-edit, color: blue] {
ride_assignments_id serial pk
elevator_id fk
floorRequest_id fk
start_time timestamp
end_time timestamp
floor_visited int
created_at timestamp
updated_at timestamp
}

floorRequest.floorRequest_id - ride_assignments.floorRequest_id

elevators.elevator_id < ride_assignments.elevator_id

ride_logs [icon: document, color: white] {
ride_logs_id serial pk
ride_assignments_id fk
elevator_id int fk
start_floor int
end_floor int
start_time timestamp
end_time timestamp
ride_status ('ongoing','completed','pending', 'need_maintenance')
created_at timestamp
}

ride_assignments.ride_assignments_id - ride_logs.ride_assignments_id

buildings.building_id < floors.building_id

buildings.building_id < elevator_shafts.building_id

elevators.elevator_shaft_id - elevator_shafts.elevator_shaft_id
