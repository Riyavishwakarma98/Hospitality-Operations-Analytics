CREATE DATABASE IF NOT EXISTS hospitality_project;

use hospitality_project;

CREATE TABLE IF NOT EXISTS fact_bookings (
    booking_id VARCHAR(50) PRIMARY KEY,
    property_id INT,
    booking_date VARCHAR(20),
    check_in_date VARCHAR(20),
    checkout_date VARCHAR(20),
    no_guests INT,
    room_category VARCHAR(10),
    booking_platform VARCHAR(50),
    ratings_given INT NULL,
    booking_status VARCHAR(50),
    revenue_generated INT,
    revenue_realized INT,
    customer_id INT,
    payment_method VARCHAR(50),
    stay_duration INT,
    cancellation_reason VARCHAR(255) NULL,
    is_loyalty_member VARCHAR(10),
    country VARCHAR(50),
    customer_age INT,
    special_requests VARCHAR(10),
    discount_applied DECIMAL(10, 2),
    booking_channel VARCHAR(50)
    );

SET GLOBAL local_infile = 1;

LOAD DATA LOCAL INFILE 'C:/Users/Riya/OneDrive/Desktop/DA_P1303 Dataset/Data/hospitality_cleaned_data.csv'
INTO TABLE fact_bookings
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\r\n'
IGNORE 1 LINES;

CREATE TABLE IF NOT EXISTS dim_hotels (
    property_id INT PRIMARY KEY,
    property_name VARCHAR(100),
    category VARCHAR(50),
    city VARCHAR(50)
);

LOAD DATA LOCAL INFILE "C:/Users/Riya/OneDrive/Desktop/DA_P1303 Dataset/Data/dim_hotels.csv" 
INTO TABLE dim_hotels 
FIELDS TERMINATED BY ','  
ENCLOSED BY '"' 
LINES TERMINATED BY '\r\n' 
IGNORE 1 LINES;

CREATE TABLE IF NOT EXISTS dim_rooms (
    room_id VARCHAR(10) PRIMARY KEY,
    room_class VARCHAR(50)
);

LOAD DATA LOCAL INFILE "C:/Users/Riya/OneDrive/Desktop/DA_P1303 Dataset/Data/dim_rooms.csv" 
INTO TABLE dim_rooms 
FIELDS TERMINATED BY ','  
ENCLOSED BY '"' 
LINES TERMINATED BY '\r\n' 
IGNORE 1 LINES;

CREATE TABLE IF NOT EXISTS dim_rooms (
    room_id VARCHAR(10) PRIMARY KEY,
    room_class VARCHAR(50)
);

LOAD DATA LOCAL INFILE "C:/Users/Riya/OneDrive/Desktop/DA_P1303 Dataset/Data/dim_rooms.csv" 
INTO TABLE dim_rooms 
FIELDS TERMINATED BY ','  
ENCLOSED BY '"' 
LINES TERMINATED BY '\r\n' 
IGNORE 1 LINES;

CREATE TABLE IF NOT EXISTS dim_date (
    date VARCHAR(20) PRIMARY KEY,
    mmm_yy VARCHAR(20),
    week_no VARCHAR(10),
    day_type VARCHAR(20)
);

LOAD DATA LOCAL INFILE "C:/Users/Riya/OneDrive/Desktop/DA_P1303 Dataset/Data/dim_date.csv" 
INTO TABLE dim_date 
FIELDS TERMINATED BY ','  
ENCLOSED BY '"' 
LINES TERMINATED BY '\r\n' 
IGNORE 1 LINES;

CREATE TABLE IF NOT EXISTS fact_aggregated_bookings (
    property_id INT,
    check_in_date VARCHAR(20),
    room_category VARCHAR(10),
    successful_bookings INT,
    capacity INT,
    PRIMARY KEY (property_id, check_in_date, room_category)
);

LOAD DATA LOCAL INFILE "C:/Users/Riya/OneDrive/Desktop/DA_P1303 Dataset/Data/fact_aggregated_bookings.csv" 
INTO TABLE fact_aggregated_bookings 
FIELDS TERMINATED BY ','  
ENCLOSED BY '"' 
LINES TERMINATED BY '\r\n' 
IGNORE 1 LINES;

SELECT 'fact_bookings' AS table_name, COUNT(*) FROM fact_bookings
UNION ALL
SELECT 'fact_aggregated_bookings', COUNT(*) FROM fact_aggregated_bookings
UNION ALL
SELECT 'dim_hotels', COUNT(*) FROM dim_hotels
UNION ALL
SELECT 'dim_rooms', COUNT(*) FROM dim_rooms
UNION ALL
SELECT 'dim_date', COUNT(*) FROM dim_date;