# Smart Elevator Control - Database Design

## Design Objective

Relational database design for a multi-building elevator control system with support for request tracking, elevator assignment, ride history, status logs, and maintenance operations.

## Design Scope

- multiple buildings
- multiple elevators per building
- floor-level request capture
- request-to-elevator allocation history
- ride lifecycle storage
- elevator status telemetry storage
- maintenance workflow storage

## ER Entities

### 1. buildings

- `building_id` bigint PK
- `building_code` varchar(30) unique not null
- `name` varchar(120) not null
- `address` text
- `city` varchar(80)
- `state` varchar(80)
- `created_at` timestamp not null
- `updated_at` timestamp not null

### 2. floors

- `floor_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `floor_no` int not null
- `floor_label` varchar(20)
- `created_at` timestamp not null

Constraint:

- unique (`building_id`, `floor_no`)

### 3. elevator_shafts

- `shaft_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `shaft_number` int not null
- `zone_name` varchar(50)
- `created_at` timestamp not null
- `updated_at` timestamp not null

Constraint:

- unique (`building_id`, `shaft_number`)

### 4. elevators

- `elevator_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `shaft_id` bigint FK -> elevator_shafts.shaft_id
- `elevator_code` varchar(20) not null
- `capacity_persons` int
- `capacity_kg` int
- `status` varchar(20) not null
- `created_at` timestamp not null
- `updated_at` timestamp not null

Constraints:

- unique (`building_id`, `elevator_code`)
- unique (`shaft_id`) for 1:1 shaft to elevator mapping

### 5. elevator_floor_access

- `elevator_id` bigint FK -> elevators.elevator_id
- `floor_id` bigint FK -> floors.floor_id
- `is_enabled` boolean not null default true
- `created_at` timestamp not null

Primary key:

- (`elevator_id`, `floor_id`)

### 6. floor_requests

- `request_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `source_floor_id` bigint FK -> floors.floor_id
- `direction` varchar(10) not null
- `request_time` timestamp not null
- `status` varchar(20) not null
- `created_at` timestamp not null
- `updated_at` timestamp not null

### 7. request_allocations

- `allocation_id` bigint PK
- `request_id` bigint FK -> floor_requests.request_id
- `elevator_id` bigint FK -> elevators.elevator_id
- `assigned_at` timestamp not null
- `allocation_status` varchar(20) not null
- `strategy` varchar(40)

### 8. rides

- `ride_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `elevator_id` bigint FK -> elevators.elevator_id
- `request_id` bigint FK -> floor_requests.request_id
- `start_floor_id` bigint FK -> floors.floor_id
- `end_floor_id` bigint FK -> floors.floor_id
- `pickup_time` timestamp
- `drop_time` timestamp
- `ride_status` varchar(20) not null

### 9. elevator_status_logs

- `status_log_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `elevator_id` bigint FK -> elevators.elevator_id
- `current_floor_id` bigint FK -> floors.floor_id
- `motion_state` varchar(20) not null
- `door_state` varchar(20) not null
- `fault_code` varchar(30)
- `recorded_at` timestamp not null

### 10. maintenance_records

- `maintenance_id` bigint PK
- `building_id` bigint FK -> buildings.building_id
- `elevator_id` bigint FK -> elevators.elevator_id
- `maintenance_type` varchar(20) not null
- `severity` varchar(20) not null
- `status` varchar(20) not null
- `issue_description` text
- `scheduled_at` timestamp
- `started_at` timestamp
- `completed_at` timestamp
- `created_at` timestamp not null
- `updated_at` timestamp not null

## Cardinality

- buildings 1:N floors
- buildings 1:N elevator_shafts
- elevator_shafts 1:1 elevators
- buildings 1:N elevators
- elevators N:M floors via elevator_floor_access
- floors 1:N floor_requests
- floor_requests 1:N request_allocations
- elevators 1:N request_allocations
- elevators 1:N rides
- elevators 1:N elevator_status_logs
- elevators 1:N maintenance_records

## Integrity Rules

- `floor_requests.source_floor_id` must belong to `floor_requests.building_id`.
- `elevators.building_id` must match associated `elevator_shafts.building_id`.
- Allocated elevator must have floor access to the request floor.
- `rides.drop_time >= rides.pickup_time` when both are not null.
- Only valid domain values allowed for status/direction/type columns (use `CHECK` or enums).

## Indexing Plan

- `floors (building_id, floor_no)` unique
- `elevators (building_id, elevator_code)` unique
- `floor_requests (building_id, status, request_time desc)`
- `request_allocations (request_id, assigned_at desc)`
- `rides (elevator_id, pickup_time desc)`
- `elevator_status_logs (elevator_id, recorded_at desc)`
- `maintenance_records (elevator_id, status, severity)`

## Normalization Notes

- Design is normalized to approximately 3NF.
- N:M relation between elevators and floors is resolved through `elevator_floor_access`.
- Historical events are separated into append-only log tables (`request_allocations`, `elevator_status_logs`).

## Deliverables

- ER diagram: [schema.md](schema.md)
- Reference image: [Smart Elevator Control.png](Smart%20Elevator%20Control.png)
