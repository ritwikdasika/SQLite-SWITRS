# Analyzing Collisions in the State of California using SQLite

Using SQLite queries, we're going to analyze a curated database provided by SWITRS, which is maintained by the CHP, to answer the questions about the database below:

1. We want a table of the total pedestrian collisions listed by the day of the week. Make it happen.
2. We want a table of the DUIs listed by day of the week. This is similar to question 1.
3. What is the spread of collisions since the start of the COVID-19 pandemic? Use a graph if needed.
4. What is the average age of the party (one at fault)? What is the average age of the party who is under the influence of alcohol?

A link to the sqlite database can be found here: https://www.kaggle.com/alexgude/california-traffic-collision-data-from-switrs

---
## On what days of the week are pedestrians involved in collisions?

Here is the code to complete question 1:

![Pedestrians](https://i.ibb.co/FnnTKMB/Weekday-Pedestrians.png)

We create a table called WeekdayPedestrian and take the fields case_id and collision_date from the original collisions table. However, this gives us the date of the collision as year-month-day (2020-03-16, for example). We want the day of the week, for example, if the collision happened on March 16th, 2020, we want to know what day of the week that was. Therefore, we cast it as an integer and use the strftime() function to assign it a weekday value from 0 to 6 (0 starting on Sunday and 6 ending on Saturday). We also want to make sure we ignore any rows with NULL values.

When we first print the table, using the SELECT statement and the * Wildcard, we get 277,383 rows of collision cases, however, the day of the week is in integer form from 0 to 6. 

![](https://i.ibb.co/vDxkCnm/Weekday-Output1.png)

We use the ALTER TABLE statement to add a column to the WeekdayPedestrian table and assign character values to the day of the week. 

Now we need to drop the old column with integer values for the day of the week. To do so, we rename WeekdayPedestrian to WpTemp, and create a new table called WeekdayPedestrian. Then we insert the case_id and Weekday columns from WpTemp into WeekdayPedestrian.

Finally, we have 277,383 rows of collision cases involving pedestrians with their respective day of the week.

![](https://i.ibb.co/Y719jVr/Weekday-Output2.png)

---
## Print a table of the DUIs listed by day of the week.

Here is the code to print a table of DUIs listed by day of the week. As you can see, it is similar to part 1, so I will not explain in such detail:

![](https://i.ibb.co/5csqr9w/dui.png)

The output in SQLiteStudio is:

![](https://i.ibb.co/DrKpMqc/DUIWeekday.png)

---
## What is the spread of collisions since the start of the COVID-19 pandemic?

We would need to print a table of all collisions since the start of the COVID-19 pandemic, which was approximately around March 16th of 2020.

Here is the code that uses the WHERE clause to filter the dataset to retrieve rows of the collisions table from specified columns where the date is on or after 2020-03-16.

![](https://i.ibb.co/60Ts3sN/covidcollisions.png)

We can export the table as a CSV and print a chart with the total number of collisions per month in the state of California so far during the COVID-19 pandemic.

![](https://i.ibb.co/thyFCzX/Screen-Shot-2021-05-14-at-12-58-29-PM.png)

---
## What is the average age of the party (one at fault)? What is the average age of the party who is under the influence of alcohol?

Using the parties table in the database, we want to figure out the average age of the party and then the average age of the party who is driving under the influence.

The following code provides a table that has this information:

![](https://i.ibb.co/CKZGwfs/party.png)

To get the average age, we use this line:

![](https://i.ibb.co/0QjY3pZ/partyavgage.png)

The average age of a party in a collision in California is approximately 38 years and 6 months old.

To get the average age DUI, we use this line:

![](https://i.ibb.co/gFDDLm5/partyavgagedui.png)

party_sobriety is a field where 'A' means not under the influence and 'B' means that the person is under the influence and impaired by alcohol.

The average age of the party that is DUI in California is approximately 34 years old.