# 2023CarMarket-SQL
**SQL** : Cleaning, Wrangling and Exploring the Data    

   
**Excel**: Created an Interactive Dynamic Dashboard with Slicers using Pivot Tables and Pivot Diagrams
## Short Description
- Data Cleaning in SQL  (Done)

Standardized the convention, Fixed Structural errors, Performed Type Conversions, Renamed Columns, Handled Missing Values, Dropped useless columns, Handled Duplications
- Data Wrangling in SQL  (Done)

Created new variables, Aggregated data, Filtered observations

- Data Exploration in SQL  (Done)

Performed Descriptive analysis( Found Mean, Median, Mode, Variability(Min/Max values), Distribution.

Used Window Functions, Aggregate functions, CTE's, subqueries, filtering, sorting, mathematical operations, statistical functions, row limiting.   

Found price outliers using IQR, found price outliers using Zscore, Calculated a 95% confidence interval, etc.   

- Interactive Dashboard in Excel  (Done)

    
Slicers for search by: City, State,UsedOrNew,Year of Manufacture, Body Type, Drive Type, Transmission, Number of Seats   

Summary Statistics: Number of Cars, Minimum Year of Manufacture, Max Year, Avg Kilometres, Avg Fuel Consumption,Avg. Engine Capacity,Avg. Price,Min Price, Max Price   

Diagrams for: Top 10 most popular brands and their average vehicle price, Fuel Type Segment Overview, Vehicle Distribution based of year of manufacture, Correlation analysis ,Fuel Consumption by Fuel type.   


## Detailed Description   
### Data Cleaning and Wrangling

![1](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/e18c26ed-fe14-4be2-bf91-45365a7d245f)

##### Standardize the Convention
In the raw dataset, both empty cell and '-' are used to represent the same value 'NULL'.Which can mess with the accuracy of our analysis.   

Fixing it:   
![2](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/4be7c4d9-2c34-4e8e-a6b8-338948ddd2ac)

#### Structural Errors
There are multiple structural 'errors' in the dataset.    

1.'Location' Column contains both the 'City' and 'State' Values.
If we split this 1 column into 2, we can group and filter data separately. For example, we will be able to look for:"The average price of a car X in respect to the City Y" and the "The average price of a car X in respect to the State Z" and compare the values. So on.

![3](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/483a884b-a088-4d01-bbc5-e0c7bab2e949)

2.'Engine', 'CylindersinEngine' have duplicated values in each row, because 'Engine' column contains both the Engine Capacity in litres and the number of Cylinders, even though a separate column for Cylinders exists. Also this values and 'FuelConsumption','Doors' and 'Seats' columns are very valuable in their numerical form, however they are stored with letters inside them like so:

![4](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/9e41f657-e73d-42c5-bf30-3fae58c211c1)
Let's fix it by scraping the numerical values from records using SUBSTRING function and CHARINDEX:
![5](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/8362adf0-e124-462e-a005-87674b92e7b6)

#### Type Conversion   

Those values also have a Data Type of NVARCHAR(255) which makes it impossible to perform calculations on them.Let's fix it:   

![6](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/bc697319-f6a2-41dd-9a5f-a1f662311724)

You can already see the change:   
![13](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/3bc1b2a6-3682-4bb1-80e3-f92049613ff2)


#### Rename Columns   

For readability and so that the header actually speaks for the data stored under it.
![7](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/e311202d-8cec-43a6-8d95-299fb31f0408)

#### Handle Missing Values   
A lot of records have FuelConsumption=0, even though these cars are not electric. Therefore their FuelConsumption should be equal to NULL and not 0.    
![8](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/37c8cda5-7d76-4cb9-b477-39cd8d3b1ee5)

   
This can mess with the quality of our data A LOT, for example, when calculating the correlation between the price and the fuel consumption.
So let's fix it and see the change:

![9](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/9a564715-7221-4e62-ad99-063c02d3bbfe)

#### Drop Irrelevant Columns   
![10](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/f1c2809c-195c-4bec-9558-1f7a63a483e1) 

Columns 'Car/Suv' and 'BodyType' seem to have a lot of duplicate records.
Let's explore.   
What kind of values does BodyType have? How many missing values does it have?
![11](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/355f7e14-16d9-4f78-a34b-e4d3b1fb919d)


Records seem to be relevant to the columns names and there is 282 missing records.Now,     
Let's look at Car/Suv Column:

![12](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/322f1469-42ab-4595-9f9e-46207ee33936)
The data stored seems to be vastly irrelevant to the column name, even though some of the records are duplicates with BodyType Records->Dropped the Column Car/Suv.

### Data Exploration   

Price Analysis:    

1.Calculated the **standard deviation** of prices to understand the dispersion of prices in the dataset.    

#### Discriptive statistics:

2.What is the **median price** of car for each city?     

![14](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/bf20ca2f-2283-4247-a99c-830aa82f480c)    

**What is the difference beween percentile_cont and percentile_cont?**    

When the number of records is even(for example,6) , there is no clear cut middle value, so percentile_cont will calculate the avg(3rd,4th) and display that as the median i.e., an interpolated value which is not present in the original dataset.In the same situation, percentile_disc is going to fetch a value within the table( in this example, 3rd value)    

![15](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/b0523f9d-36e0-462c-80c5-48ad35a252fd)    

3. What is the overall **median** price of the dataset? (Irrespective of the city)
What is the **Mean** Price of the dataset?

![16](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/d69dd876-521a-40f6-b3b2-af975ae936b2)    

Are Mean Price and Median price different?   

![17](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/e4f2e489-b66c-486a-b15b-b205c2c2ea5c)   

**Why are Mean Price and Median Price so different?** 

The Median is a better measure of the central tendency of the group as it is not skewed by exceptionally high or low characteristic values.    

5. **Find Price Outliers by using IQR**

![18](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/926b045e-65de-4dfa-81ba-a83d1adb8136)    

As a result of that query I got 931 records of price outliers:    

![19](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/ff8571da-cfd7-41b9-b6d6-13d47d26e4d2)    

However **using the IQR method can find more "outliers" than using the standard deviation (or zscore)**. It depends on the distribution of the data. Data that's peaked with long tails will have a comparatively low IQR, so the IQR method will find lots of outliers.    


7. **Find Price Outliers by using Zscore**

![20](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/59bc0c37-57d8-4ab3-8e16-b687535d090b)   

By using zscore we get way less outliers, 286 - to be specific:    

![21](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/9d86789f-5943-4ede-b4a1-aeccf0d6f3bd)    


9. Calculate **95 % confidence interval** for the price of cars

![22](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/e30fe7dd-e47f-4da0-a092-d036255742b1)     

With a 95 percent confidence interval, you have a 5 percent chance of being wrong:    

![23](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/3a9cf361-cb27-4dbe-94e6-a5248eb34bf7)    

11. Found Variability of Price column**(Found Min/Max value)**
  
13. Found TOP 10 car brands listed for sale in 2023 in Australia.

![image](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/20d80179-2cb4-407d-9a09-b6fbdeba5f5b)   

15. Found TOP 10 car Models.

   Found that Toyota Hilux is the most frequently listed car in the dataset.  

17. Calculated the percentage of the market overtaken by Toyota Hilux

11.Since Toyota is the most popular brand. **What is the distribution** of its Models on the market?**What is the Mean price for each such Model?** 

![image](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/0aecb7ff-bd29-4a0c-8740-417d8270c649)   

19. Calculated what Percentege of cars on sale were New? What percentage were Used?
    
21. Calculated the  count and percentage presence of each fuel type in the dataset
    
![24](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/4bc5db22-1c74-4c04-89cf-bb747046a061)   

23. What percentage of cars on the market have extremely high mileage?
    
![25](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/8ba0ee3f-29a0-4d0f-9c7e-ffd390cc76eb)   

    
### Dynamic Interactive Dashboard in Excel

Examples of Pivot tables Created:   

    
![pivotTables](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/8652e0a4-3ccc-413b-a3d1-1e387af19ced)

Overview of the Dashboard:    

![ExcelDashboard](https://github.com/orynbay21/2023CarMarket-SQL/assets/98757036/a7406581-c41f-4e09-9bb3-2f094bfb78bf)


### Link to the original dataset   

https://www.kaggle.com/datasets/nelgiriyewithana/australian-vehicle-prices
