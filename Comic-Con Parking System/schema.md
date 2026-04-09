//anime screeings, cosplay competition, gaming showcase,creator meetups, merchandise zone, panel discussions

//bikes, cars, SUVs, cabs, Ev vechiles

//multiple zones and levels

// cosplayers with props, exhibitors, creators, VIP guests, staff members and EV charging vehicles. - reserved parking

Vechicles [icon: car, color: black] {
vehicle_id serial pk
vehicle_number varchar(20) unique
category ('bikes', 'cars', 'SUVs', 'cabs', 'EV', 'other')
created_at timestamp
updated_at timestamp
}

parkingSpots [icon: parking, color: green] {
parkingSpot_id serial pk
status ('available', 'occupied', 'reserved')
zone_id int
level_no int
created_at timestamp
updated_at timestamp
}

reserved_parking [icon: parking, color: red] {
reserved_parking_id serial pk
parkingSpot_id fk
category ('exhibitors', 'creators', 'VIP guests', 'staff members' ,'EV_charging_vehicles')
created_at timestamp
updated_at timestamp

}

parkingTickets [icon: ticket, color: yellow] {
parkingTicket_id int pk
parkingSpot_id fk
vehicle_id fk
created_at timestamp
updated_at timestamp
}

parkingSessions [icon: calendar, color: orange] {
parkingSession_id int pk
parkingTicket_id fk
entryTime timestamp
exitTime timestamp
created_at timestamp
updated_at timestamp

}

paymentRecords [icon: payment, color: purple ] {
payment_id int pk
parkingTicket_id fk
payment_mode ('netbanking', 'upi', 'cash')
status ('pending', 'failed', 'completed')
Amount ('exitTime entryTime') decimal (10, 2)
created_at timestamp
updated_at timestamp
}

parkingTickets.parkingSpot_id > parkingSpots.parkingSpot_id

parkingSpots.parkingSpot_id - reserved_parking.parkingSpot_id

Vechicles.vehicle_id - parkingTickets.vehicle_id

parkingTickets.parkingTicket_id - parkingSessions.parkingTicket_id

parkingTickets.parkingTicket_id - paymentRecords.parkingTicket_id
