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

elevators [icon: share] {
elevator_id serial pk
building_id fk
capacity int
created_at timestamp
updated_at timestamp
}

elevator shafts [icon: share-2] {
elevator_shaft_id serial pk
building_id fk
shaft_number int
created_at timestamp
updated_at timestamp
}

floorRequest [icon: layers] {
floorRequest_id serial pk
building_id fk
requested_at timestamp
status ('pending', 'assigned', 'completed')

}

buildings.building_id < floors.building_id

floors.floor_id < elevator shafts.building_id
