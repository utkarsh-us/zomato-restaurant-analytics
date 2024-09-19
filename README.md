# Zomato Data Analysis Project

## Project Overview

This project aims to analyze Zomato restaurant data to uncover insights into customer preferences, restaurant types, ratings, and spending patterns using Python.

## Table of Contents

- [Project Overview](#project-overview)
- [Dataset](#dataset)
- [Key Features](#key-features)
- [Python Code and Analysis](#python-code-and-analysis)
  - [Data Cleaning](#data-cleaning)
  - [Data Exploration](#data-exploration)
  - [Key Business Questions](#key-business-questions)
- [Tools Used](#tools-used)
- [How to Run the Project](#how-to-run-the-project)
- [Conclusion](#conclusion)
- [Future Scope](#future-scope)
- [License](#license)

## Dataset

The dataset includes:

- **Restaurant Name**: Name of the restaurant.
- **Location**: Location of the restaurant.
- **Online Order**: Whether online ordering is available (Yes/No).
- **Book Table**: Whether booking a table is available (Yes/No).
- **Votes**: Number of votes received.
- **Rating**: Average rating of the restaurant.
- **Cuisines**: Types of cuisines offered by the restaurant.
- **Cost for Two**: Approximate cost for two people.

## Key Features

- Data Cleaning and Processing.
- Data Visualization using Python libraries.
- Restaurant type, rating, and cost analysis.
- Insights into customer preferences.

---

## Python Code and Analysis

### Data Cleaning

- **Checking for Missing Values**:

    ```python
    missing_values = df.isnull().sum()
    print("Missing values:\n", missing_values)
    ```

- **Cleaning the 'rate' column**:

    ```python
    df["rate"] = df["rate"].str.replace("/5", "").astype(float)
    ```

### Data Exploration

- **Distribution of Restaurant Types**:

    ```python
    sns.countplot(x=df['listed_in(type)'])
    plt.xlabel("Type of Restaurant")
    plt.title("Type of Restaurants")
    plt.show()
    ```

- **Total Votes per Restaurant Type**:

    ```python
    grouped_data = df.groupby('listed_in(type)')['votes'].sum()
    plt.plot(grouped_data, marker="o", color='green')
    plt.title("Total Votes per Restaurant Type")
    plt.show()
    ```

### Key Business Questions

1. **What type of restaurant do customers prefer?**

    ```python
    sns.countplot(x=df['listed_in(type)'])
    plt.show()
    ```

2. **How many votes has each type of restaurant received?**

    ```python
    grouped_data = df.groupby('listed_in(type)')['votes'].sum()
    plt.plot(grouped_data, marker="o", color='green')
    plt.show()
    ```

3. **What ratings have the majority of restaurants received?**

    ```python
    plt.hist(df["rate"], bins=3, color='skyblue')
    plt.title('Ratings Distribution')
    plt.show()
    ```

4. **What is the average order spending by couples?**

    ```python
    sns.countplot(x=df["approx_cost(for two people)"], palette="Blues")
    plt.title('Cost for Two People')
    plt.show()
    ```

5. **Which mode (online/offline) has received the maximum ratings?**

    ```python
    max_rating = df.groupby('online_order')['rate'].sum()
    plt.plot(max_rating, marker='o', color='green')
    plt.title('Mode with Maximum Ratings')
    plt.show()
    ```

6. **Which type of restaurant received more offline orders?**

    ```python
    grouped_data = df.groupby(['listed_in(type)', 'online_order'])['rate'].count()
    print(grouped_data[grouped_data['online_order'] == 'Offline'])
    ```

7. **Heatmap - Restaurant type vs. Order mode**:

    ```python
    pivot_table = df.pivot_table(index='listed_in(type)', columns='online_order', aggfunc='size', fill_value=0)
    sns.heatmap(pivot_table, annot=True, fmt='d', cmap="coolwarm")
    plt.title("Heatmap of Restaurant Type vs Order Mode")
    plt.show()
    ```

---

## Tools Used

- **Python**: For data analysis.
- **pandas**: Data manipulation.
- **numpy**: Numerical operations.
- **matplotlib**: Data visualization.
- **seaborn**: Statistical visualization.

---

## How to Run the Project

1. Clone this repository:

    ```bash
    git clone https://github.com/utkarsh-us/zomato-data-analysis.git
    ```

2. Install required libraries:

    ```bash
    pip install pandas numpy matplotlib seaborn
    ```

3. Run the Python code to analyze the dataset.

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

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
