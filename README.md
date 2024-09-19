# Zomato Data Analysis Project

## Project Overview

This project aims to analyze Zomato restaurant data to uncover insights into customer preferences, restaurant types, ratings, and spending patterns. We use Python libraries for data analysis and visualization.

## Libraries Used

- **pandas**: For data manipulation and analysis.
- **numpy**: For numerical operations.
- **matplotlib**: For data visualization.
- **seaborn**: For statistical data visualization.

## Python Code

```python
# Step 1: Importing Libraries
# pandas for data manipulation, numpy for numerical operations,
# matplotlib and seaborn for data visualization.

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Step 2: Loading the Dataset
# Loading the Zomato dataset into a pandas DataFrame to begin the analysis.

df = pd.read_csv("dataset/zomato_data.csv")

# Step 3: Inspecting the Dataset
# Checking the structure, shape, and summary of the data.

print("DataFrame shape:", df.shape)  # Prints number of rows and columns
print(df.head(1))  # Displays the first row of the dataset
df.info()  # Prints summary of the dataset, including non-null counts and data types

# Step 4: Checking for Missing Values
# This step checks for missing values in each column and calculates the percentage of missing data.

missing_values = df.isnull().sum()  # Total missing values per column
print("Missing values:\n", missing_values)
missing_percentage = (df.isnull().sum() / df.shape[0]) * 100  # Percentage of missing data
print("Missing data percentage:\n", missing_percentage)

# Dropping rows with missing values to clean the data
df.dropna(inplace=True)

# Step 5: Cleaning the 'rate' column
# The 'rate' column contains ratings like '4.1/5'. We'll remove the '/5' part and convert the column to a float.

df["rate"] = df["rate"].str.replace("/5", "")  # Removing '/5' from ratings
df['rate'] = df['rate'].astype(float)  # Converting the 'rate' column to float for analysis

# Step 6: Analysis 1 - What type of restaurant do customers prefer?
# We visualize the count of different restaurant types using a bar plot.

sns.countplot(x=df['listed_in(type)'])
plt.xlabel("Type of Restaurant")
plt.title("Type of Restaurants")
plt.show()

# Conclusion: Dining restaurants have the maximum customer orders.

# Step 7: Analysis 2 - How many votes has each type of restaurant received?
# We group the data by restaurant type and sum the votes to see which type has received the most votes.

grouped_data = df.groupby('listed_in(type)')['votes'].sum()  # Grouping by restaurant type and summing votes
result = pd.DataFrame({'votes': grouped_data})  # Creating a new DataFrame for visualization

plt.plot(result, color='green', marker="o")  # Plotting total votes by restaurant type
plt.xlabel("Type of Restaurant", color='red', size=20)
plt.ylabel("Votes", color='red', size=20)
plt.title("Total Votes per Restaurant Type")
plt.show()

# Conclusion: Dining restaurants have received the maximum votes from customers.

# Step 8: Analysis 3 - What ratings have the majority of restaurants received?
# We use a histogram to visualize the distribution of restaurant ratings.

plt.hist(df["rate"], bins=3, color='skyblue')
plt.title('Ratings Distribution')
plt.xlabel('Ratings')
plt.ylabel('Count')
plt.show()

# Conclusion: The majority of restaurants have ratings between 3.25 and 4.00.

# Step 9: Analysis 4 - Average order spending by couples
# We analyze the 'approx_cost(for two people)' column to understand the spending preferences of couples.

couple_data = df["approx_cost(for two people)"]  # Extracting cost data for two people
sns.countplot(x=couple_data, palette="Blues")  # Plotting cost distribution
plt.title('Cost for Two People')
plt.xlabel('Approx Cost (₹)')
plt.ylabel('Count')
plt.show()

# Conclusion: The majority of couples prefer restaurants with an approximate cost of ₹300.

# Step 10: Analysis 5 - Which mode (online/offline) has received the maximum ratings?
# We replace the values in the 'online_order' column with 'Online' and 'Offline' and analyze the ratings.

mode = {'No': 'Offline', 'Yes': 'Online'}
df['online_order'] = df['online_order'].replace(mode)  # Replacing values in the 'online_order' column

max_rating = df.groupby('online_order')['rate'].sum()  # Grouping by order mode and summing ratings
result = pd.DataFrame({'rate': max_rating})  # Creating a DataFrame for visualization

plt.plot(result, color='green', marker='o')
plt.title('Mode with Maximum Ratings')
plt.xlabel('Online/Offline')
plt.ylabel('Total Ratings')
plt.show()

# Conclusion: Offline mode has received the maximum ratings.

# Step 11: Analysis 6 - Which type of restaurant received more offline orders?
# We analyze restaurant type and order mode to determine which types receive more offline orders.

grouped_data = df.groupby(['listed_in(type)', 'online_order'])['rate'].count()  # Grouping by restaurant type and order mode
result = pd.DataFrame({'rate': grouped_data}).reset_index()

# Filtering for offline orders
filtered_data = result[result["online_order"] == 'Offline']
print(filtered_data)

# Step 12: Heatmap Visualization - Restaurant type vs. order mode
# Creating a heatmap to visualize the distribution of restaurant types and their preferred order modes.

pivot_table = df.pivot_table(index='listed_in(type)', columns='online_order', aggfunc='size', fill_value=0)
sns.heatmap(pivot_table, annot=True, fmt='d', cmap="coolwarm")
plt.title("Heatmap of Restaurant Type vs Order Mode")
plt.xlabel("Online/Offline Mode")
plt.ylabel("Restaurant Type")
plt.show()

# Conclusion: Dining restaurants have received the most offline orders, indicating that Zomato can offer discounts or promotions to offline dining customers.

```
---

## Conclusion

- **Dining restaurants** are the most popular among customers and receive the highest votes.
- The majority of restaurants have **ratings between 3.25 and 4.00**.
- **Offline mode** has received the maximum ratings, indicating higher customer satisfaction with offline dining experiences.
- **Couples** prefer restaurants with an approximate cost of **₹300** for two people.

---

## Future Scope

- **Sentiment Analysis**: Use customer reviews to analyze customer sentiment toward restaurants.
- **Time-based Analysis**: Investigate how ratings and orders fluctuate during different times of the day or week.
- **Machine Learning**: Build predictive models to estimate restaurant success based on factors like location, cuisine, and order modes.


## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
