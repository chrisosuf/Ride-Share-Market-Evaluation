# RideShare Market Evaluation Algorithm

### Author
Chris Osufsen

The goal of this project is to create an algorithm that will examine a market’s potential for ridesharing (consolidating multiple taxi trips into one). I then use this method to evaluate Manhattan’s potential for ridesharing using a full week’s worth of yellow cab (efficiency exploration notebook) and explore how things vary based on time and location (location and time exploration).
Code Files: Efficiency Exploration.ipynb and Location and Time Exploration.ipynb

## 1.	Assess Potential Efficiency

### Algorithm Summary: 
I define an efficiently aggregated ride as one that reduces the amount of drivers and completes the rides without hurting user experience and revenue. My method of determining if a location has potential for producing such aggregated rides is as follows:
1.	Simulate/aggregate rides that have the following points in common
a.	Going in similar direction (angle difference < 15 degrees)
b.	Pickup time difference < 5 minutes
c.	Similar Route distance
d.	Pickup location distance < .5 miles
2.	For all aggregated rides, I analyze:
a.	Total rides aggregated
b.	Reduction in driver costs based on driver hourly wage
c.	Average increase in trip duration for passengers
d.	Total and average revenue increase or decrease

### Assumptions: 
-	$7 flat fee for each rideshare passenger, original fare for single ride comparison
-	Trips to combine can be compared by straight line distance, not route/street distance
-	Pickup/drop off of new rider takes 2 extra min
-	The cost saved from removing a driver (aggregating 2 cars to one), is the duration of that driver's previous ride at their hourly wage ($20/hour)
-	Rides cannot be matched together with more than 4 passengers

### Goal: 
If a location is not losing revenue from the simulation of aggregated rides, and new customer trip times are not going up by more than 5-10 minutes on average, then the location likely has potential for producing efficiently aggregated rides.
Constraints and thresholds for matching rides to aggregate may be easily adjusted and would be valuable to examine in a sensitivity analysis as well.

## 2.	Apply to Manhattan
After simulating and aggregating rides on a subset of the data for the first full week of June, we achieve the following results on 6696 new rides (aggregated from many more)

<img width="409" alt="image" src="https://user-images.githubusercontent.com/22949201/171525049-6e607907-896e-4fc4-84ef-29f93dbeeaa8.png">

Overall we see that the reduced driver costs and flat fare of passengers outweigh the original fare of the non-aggregated ride, for most rides in Manhattan using this simulation. Increase in passenger ride duration remained acceptable as well. Based on this, Manhattan looks to be a good candidate for further exploration of potential. 

### Model Complexity: My algorithm for simulating rides and evaluating efficiency will not grow exponentially complex. I sort the data based on pickup time and feed it in batches to the algorithm to aggregate, group based on matches, and then output into the evaluation. 
So instead of comparing n to n rows, I am comparing each constant-sized subset to itself, n/x times, where n is the number of rides, and x is the batch size. 

### Example of 2 Aggregated Rides

<img width="713" alt="image" src="https://user-images.githubusercontent.com/22949201/171525089-6d0e42a9-ba4d-4686-92a1-ee087779db1d.png">

## 3.	Location and Time Analysis
### Location Analysis
Midtown and Upper Manhattan proved to be efficient areas to aggregate rides in, showing higher revenue post-aggregation and acceptable duration change. 
Lower Manhattan struggled to produce efficient rides even with our constraints.
Ride duration fluctuations were largely marginal based on location.
Rides with a higher duration relative to distance typically lost more money when they were aggregated, likely because the original cab fare exceeded the total flat fee of users. Negative profit rides had an average duration 12 minutes higher than profitable rides. 
Low profit neighborhoods often had high duration rides originally, which may give some insight as to why they underperformed: More maneuverable grid-like structure of Midtown vs lower Manhattan, for example.

<img width="388" alt="image" src="https://user-images.githubusercontent.com/22949201/171525131-9609b3e7-00a7-477e-910b-cd7c91ed5c57.png">

Avg revenue increase per ride in top performing neighborhoods (after aggregation)

<img width="400" alt="image" src="https://user-images.githubusercontent.com/22949201/171525176-014607d8-210a-4636-aa8b-0f6e33f3601d.png">

Avg revenue increase/decrease per ride in low performing neighborhoods (after aggregation)

<img width="415" alt="image" src="https://user-images.githubusercontent.com/22949201/171525189-edf62235-48e6-43fa-832f-ade920afe6ce.png">

### Time

<img width="459" alt="image" src="https://user-images.githubusercontent.com/22949201/171525202-bdfb546f-45bf-41c6-95d5-e84c47eb93d0.png">

<img width="454" alt="image" src="https://user-images.githubusercontent.com/22949201/171525211-ac782591-1eb9-49f6-8070-c0ea2c437408.png">

The algorithm aggregated more rides at night than during the day despite a balanced subset of data. My parameter constraints were more rigidly met during this time. In addition to this, aggregated rides were more profitable at night and on the weekend (particularly Saturday and Sunday). New duration changes were still marginal. Note: minimal data from 12am-6am

<img width="256" alt="image" src="https://user-images.githubusercontent.com/22949201/171525326-54743de8-7e7c-4c42-8286-e3ba922a35d4.png">
<img width="235" alt="image" src="https://user-images.githubusercontent.com/22949201/171525369-471eb416-6056-4ed0-8a5e-3cf47b8f3d17.png">

<img width="256" alt="image" src="https://user-images.githubusercontent.com/22949201/171525377-a01c9c85-4719-4acf-ae02-7513996ca5b1.png">
<img width="311" alt="image" src="https://user-images.githubusercontent.com/22949201/171525384-eba2e484-05c4-4950-9762-63dd5dcffd23.png">

### Thoughts and Other Business Implications 
One feature that stood out in our low-efficiency zones was the average duration of rides before aggregation. Longer rides proved to result in lower profit rides when aggregated. This is likely due to the original cab fare exceeding the total fixed flat fee structure in place in my algorithm. Perhaps the flat fee can be adjusted when expected ride duration/distance crosses a certain threshold to make sure we remain positive on our routes within these areas. 
Our most efficient rides tend to take place on weekend nights in mid-upper Manhattan. Isolating efficient aggregation areas by neighborhood and only offering the feature to our high value zones (or marketing it more heavily in these zones) may also be a way to increase our performance on one specific area. By doing this, we can even potentially add value to cities where aggregation is inefficient on average, but worth it when we are selective. 

