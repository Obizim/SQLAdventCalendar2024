# SQL ADVENT CALENDAR 2024

#### Day 1
A ski resort company want to know which customers rented ski equipment for more than one type of activity (e.g., skiing and snowboarding). List the customer names and the number of distinct activities they rented equipment for.
```
SELECT customer_name,
 COUNT(DISTINCT activity) as activity_count
FROM rentals
GROUP BY customer_name
HAVING activity_count > 1;
```

#### Day 2
Santa wants to know which gifts weigh more than 1 kg. Can you list them?
```
SELECT gift_name
FROM gifts
WHERE weight_kg > 1;
```

#### Day 3
You’re trying to identify the most calorie-packed candies to avoid during your holiday binge. Write a query to rank candies based on their calorie count within each category. Include the candy name, category, calories, and rank (rank_in_category) within the category.
```
SELECT candy_name, 
  candy_category,
  calories,
  RANK() OVER(PARTITION BY candy_category ORDER BY calories) AS rank_in_category
FROM candy_nutrition;
```

#### Day 4
You’re planning your next ski vacation and want to find the best regions with heavy snowfall. Given the tables resorts and snowfall, find the average snowfall for each region and sort the regions in descending order of average snowfall. Return the columns region and average_snowfall.
```
SELECT ski_resorts.region,
    AVG(snowfall.snowfall_inches) AS average_snowfall
FROM ski_resorts 
INNER JOIN snowfall 
ON ski_resorts.resort_id = snowfall.resort_id
GROUP BY region
ORDER BY average_snowfall DESC;
```

#### Day 5
This year, we're celebrating Christmas in the Southern Hemisphere! Which beaches are expected to have temperatures above 30°C on Christmas Day?
```
SELECT beach_name 
FROM beach_temperature_predictions 
WHERE expected_temperature_c > 30 AND date = '2024-12-25';
```

#### Day 6
Scientists are tracking polar bears across the Arctic to monitor their migration patterns and caloric intake. Write a query to find the top 3 polar bears that have traveled the longest total distance in December 2024. Include their bear_id, bear_name, and total_distance_traveled in the results.
```
SELECT polar_bears.bear_id, polar_bears.bear_name, SUM(tracking.distance_km) AS total_distance_traveled
FROM polar_bears
JOIN tracking
ON polar_bears.bear_id = tracking.bear_id
WHERE tracking.date BETWEEN '2024-12-01' AND '2024-12-31'
GROUP BY polar_bears.bear_id
ORDER BY total_distance_traveled DESC
LIMIT 3;
```
