# 2023CarMarket-SQL
**SQL** : Cleaning, Wrangling and Exploring the Data
## Short Description
- Data Cleaning in SQL  (Done)

Standardized the convention,Fixed Structural errors,Performed Type Conversions,Renamed Columns,Handled Missing Values,Dropped useless columns, Handled Duplications
- Data Wrangling in SQL  (Done)
  
Created new variables,Aggregated data,Filtered observations
- Data Exploration in SQL  (Done)

Descriptive statistics analysis

## Detailed Description   
### Data Cleaning

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
