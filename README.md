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

#### Day 13
We need to make sure Santa's sleigh is properly balanced. Find the total weight of gifts for each recipient.
```
SELECT recipient, SUM(weight_kg) 
FROM gifts
GROUP BY recipient;
```

#### Day 14
Which ski resorts had snowfall greater than 50 inches?
```
SELECT * 
FROM snowfall
WHERE snowfall_inches > 50;
```

#### Day 15
A family reunion is being planned, and the organizer wants to identify the three family members with the most children. Write a query to calculate the total number of children for each parent and rank them. Include the parent’s name and their total number of children in the result.
```
SELECT name, COUNT(child_id) AS total_children
FROM family_members
JOIN parent_child_relationships 
ON member_id = parent_id
GROUP BY name
ORDER BY total_children DESC
LIMIT 3;
```

#### Day 16
As the owner of a candy store, you want to understand which of your products are selling best. Write a query to calculate the total revenue generated from each candy category.
```
SELECT category, SUM(quantity_sold * price_per_unit) AS total_revenue
FROM candy_sales
GROUP BY category;
```

#### Day 17
The Grinch is planning out his pranks for this holiday season. Which pranks have a difficulty level of “Advanced” or “Expert"? List the prank name and location (both in descending order).
```
SELECT prank_name, location
FROM grinch_pranks
WHERE difficulty IN ('Advanced', 'Expert')
ORDER BY prank_name DESC, location DESC;
```

#### Day 18
A travel agency is promoting activities for a "Summer Christmas" party. They want to identify the top 2 activities based on the average rating. Write a query to rank the activities by average rating.
```
SELECT activity_name, AVG(rating) AS avg_rating
FROM activities
JOIN activity_ratings
ON activities.activity_id = activity_ratings.activity_id
GROUP BY activity_name
ORDER BY avg_rating DESC LIMIT 2;
```

#### Day 19
Scientists are studying the diets of polar bears. Write a query to find the maximum amount of food (in kilograms) consumed by each polar bear in a single meal December 2024. Include the bear_name and biggest_meal_kg, and sort the results in descending order of largest meal consumed.
```
SELECT bear_name, MAX(food_weight_kg) AS biggest_meal_kg
FROM polar_bears
JOIN meal_log
ON polar_bears.bear_id = meal_log.bear_id
WHERE "date" BETWEEN '2024-12-01' AND '2024-12-31'
GROUP BY bear_name
ORDER BY biggest_meal_kg DESC;
```

#### Day 20
We are looking for cheap gifts at the market. Which vendors are selling items priced below $10? List the unique (i.e. remove duplicates) vendor names.
```
SELECT DISTINCT vendor_name
FROM vendors
JOIN item_prices
ON vendors.vendor_id = item_prices.vendor_id
WHERE price_usd < 10;
```
