<<<<<<< HEAD

# MICROSOFT MOVIE STUDIO ANALYSIS


# Business Understanding

# %% [markdown]
# Microsoft is looking into starting a new movie studio and since they do not have any experience with movie studios, this project is aimed at analysing which films are currently doing the best at the box office to come up with recommendations for Microsoft as it starts its first movie studio.

# %% [markdown]
# ## Data Source and Exploration

# %% [markdown]
# This data comes from various data files : [bom.movie_gross.csv.gz](dsc-phase-1-project-v2-4/zippedData/bom.movie_gross.csv.gz) and [tmdb.csv](dsc-phase-1-project-v2-4/zippedData/tmdb.movies.csv.gz) as well as [im.db](dsc-phase-1-project-v2-4/zippedData/im.db.zip)
# In order to carry out the data analysis, the first step is choosing the data we want to work with depending on the information we want to analyse. In this case what we need includes
# - Film release dates
# - genre
# - budget
# - movie ratings
# - box office gross(domestic and international)

# %%
# import of my modules
import pandas as pd
import csv
import matplotlib.pyplot as plt
%matplotlib inline

# %% [markdown]
# The next step will be to explore the data sets that we have, we will read from the file bom.movie_gross.csv.gv and check out the dataFrame that we are working with, our DataFrame will be df

# %%
df = df = pd.read_csv('bom.movie_gross.csv')

df.head(10)

# %% [markdown]
# ### Data Cleaning
# 

# %% [markdown]
# - The next step is to clean the data to ensure it does not have missing values and drop any if need be.
# In this case we will check how the data looks using df.info() then in case of missing values, we will do data cleaning

# %%
df.info()

# %%
# Check for missing values in all columns
print(df.isnull().sum())

# %%
# Explore the 'foreign_gross' column specifically
print(df['foreign_gross'].unique())  # View unique values to understand inconsistencies

# %% [markdown]
# - We will work with domestic_gross since the missing values are not many, hence they will not alter our analysis.
# We will drop the missing values NaN 

# %%
#Dropping the missing values in the domestic_gross column 
df.dropna(subset=['domestic_gross'], inplace=True) #inplace = True to maintain the initial dataFrame
df['domestic_gross'].isnull().sum()

# %% [markdown]
# # Exploratory Data Analysis

# %% [markdown]
# ### Descriptive Statistics

# %% [markdown]
# In this section, we analyse the mean, median and other measures of central tendencies
# - We will calculate the mean and median to find out the average gross earned from the movies

# %%
#mean of the domestic_gross
mean_domestic_gross_earning = df['domestic_gross'].mean()
print(f'mean_domestic_gross_earning: {mean_domestic_gross_earning}')
#median of the domestic_gross
median_domestic_gross_earning = df['domestic_gross'].median()
print(f'median_domestic_gross_earning: {median_domestic_gross_earning}')

# %% [markdown]
# - Visualization of the data on a graph 

# %%
# Sort together by domestic gross (optional)
sorted_df = df.sort_values(by='domestic_gross', ascending=False)  # Sort high to low gross

# Select top 10
top_10_df = sorted_df.head(10)  # Get the first 10 rows (top 10 grossing)

# Create the bar graph
plt.figure(figsize=(10, 6))  # Set figure size
plt.bar(top_10_df['title'], top_10_df['domestic_gross'], color='skyblue')  # Plot bars with titles on x-axis
plt.xlabel('Movie Titles')  # Label for x-axis
plt.ylabel('Domestic Gross')  # Label for y-axis
plt.title('Top 10 Grossing Movies (Domestic)')  # Title for the graph

plt.xticks(rotation=45, ha='right')  # Rotate if titles are long



=======

>>>>>>> c12b6901cf44001db580043b051ca611ecfb4fba
