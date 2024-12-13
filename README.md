# SQL ADVENT CALENDAR 2024
Link to challenges - https://www.sqlcalendar.com/app/advent-calendar

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

#### Day 7
The owner of a winter market wants to know which vendors have generated the highest revenue overall. For each vendor, calculate the total revenue for all their items and return a list of the top 2 vendors by total revenue. Include the vendor_name and total_revenue in your results.
```
SELECT vendors.vendor_name, SUM(sales.price_per_unit * sales.quantity_sold) AS total_revenue
FROM vendors JOIN sales
ON vendors.vendor_id = sales.vendor_id
GROUP BY vendor_name
ORDER BY total_revenue DESC
LIMIT 2;
```

#### Day 8
You are managing inventory in Santa's workshop. Which gifts are meant for "good" recipients? List the gift name and its weight.
```
SELECT gift_name, weight_kg
FROM gifts
WHERE recipient_type = 'good';
```

#### Day 9
A community is hosting a series of festive feasts, and they want to ensure a balanced menu. Write a query to identify the top 3 most calorie-dense dishes (calories per gram) served for each event. Include the dish_name, event_name, and the calculated calorie density in your results.
```
WITH CTE AS (SELECT menu.dish_name, events.event_name, 
    (menu.calories/menu.weight_g) AS calorie_per_g,
    RANK () OVER(PARTITION BY events.event_name ORDER BY (menu.calories/menu.weight_g) DESC) AS calorie_density_rank
FROM  events 
JOIN menu
ON events.event_id = menu.event_id)
SELECT dish_name, event_name, calorie_per_g
FROM CTE
WHERE calorie_density_rank <= 3
ORDER BY event_name;
```

#### Day 10
You are tracking your friends' New Year’s resolution progress. Write a query to calculate the following for each friend: number of resolutions they made, number of resolutions they completed, and success percentage (% of resolutions completed) and a success category based on the success percentage:
- Green: If success percentage is greater than 75%.
- Yellow: If success percentage is between 50% and 75% (inclusive).
- Red: If success percentage is less than 50%.
```
SELECT friend_name, 
COUNT(resolution) AS resolution_count,
COUNT(CASE WHEN is_completed = 1 THEN 1 END) AS completed_resolutions_count,
COUNT(CASE WHEN is_completed = 1 THEN 1 END)/COUNT(resolution) * 100 AS success_percentage,
CASE 
    WHEN (COUNT(CASE WHEN is_completed = 1 THEN 1 END)/COUNT(resolution) * 100) > 75 THEN 'Green'
    WHEN (COUNT(CASE WHEN is_completed = 1 THEN 1 END)/COUNT(resolution) * 100) >=50 AND (COUNT(CASE WHEN is_completed = 1 THEN 1 END)/COUNT(resolution) * 100) <=75 THEN 'Yellow'
    ELSE 'Red'
END AS success_category
FROM resolutions
GROUP BY friend_name;
```

#### Day 11
You are preparing holiday gifts for your family. Who in the family_members table are celebrating their birthdays in December 2024? List their name and birthday.
```
SELECT name, birthday 
FROM family_members
WHERE birthday BETWEEN '2024-12-01' AND '2024-12-31';
```

#### Day 12
A collector wants to identify the top 3 snow globes with the highest number of figurines. Write a query to rank them and include their globe_name, number of figurines, and material.
```
SELECT snow_globes.globe_name,
COUNT(figurines.figurine_type) AS num_figurines,
snow_globes.material
FROM snow_globes 
JOIN figurines
ON snow_globes.globe_id = figurines.globe_id
GROUP BY globe_name
ORDER BY num_figurines DESC
LIMIT 3;
```
