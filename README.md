# MICROSOFT MOVIE STUDIO ANALYSIS

## Business Understanding

Microsoft is looking into starting a new movie studio and since they do not have any experience with movie studios, this project is aimed at analysing which films are currently doing the best at the box office to come up with recommendations for Microsoft as it starts its first movie studio.

## Data Source and Exploration

This data comes from various data files : [bom.movie_gross.csv.gz and tmdb.csv](file:///tmp/dsc-phase-1-project-v2-4/zippedData/bom.movie_gross.csv.gz)[ as well ](file:///tmp/dsc-phase-1-project-v2-4/zippedData/tmdb.movies.csv.gz)as im.db In [order to ](file:///tmp/dsc-phase-1-project-v2-4/zippedData/im.db.zip)carry out the data analysis, the first step is choosing the data we want to work with depending on the information we want to analyse. In this case what we need includes

- Film release dates

- genre

- budget

- movie ratings

- box office gross(domestic and international)

### import your modules

**import** pandas **as** pd 

**import** csv 

**import** matplotlib.pyplot **as** plt **%matplotlib** inline

The next step will be to explore the data sets that we have, we will read from the file bom.movie and check out the dataFrame that we are working with, our DataFrame will be df

 df  =  pd. read\_csv ( 'bom.movie\_gross.csv' ) 

df.head(10) 

 **title studio domestic\_gross foreign\_gross year**



|**0**|Toy Story 3|BV|415000000\.0|652000000|2010|
| - | - | - | - | - | - |
|**1**|Alice in Wonderland (2010)|BV|334200000\.0|691300000|2010|
|**2**|Harry Potter and the Deathly Hallows Part 1|WB|296000000\.0|664300000|2010|
|**3**|Inception|WB|292600000\.0|535700000|2010|
|**4**|Shrek Forever After|P/DW|238700000\.0|513900000|2010|
|**5**|The Twilight Saga: Eclipse|Sum.|300500000\.0|398000000|2010|
|**6**|Iron Man 2|Par.|312400000\.0|311500000|2010|
|**7**|Tangled|BV|200800000\.0|391000000|2010|
|**8**|Despicable Me|Uni.|251500000\.0|291600000|2010|
|**9**|How to Train Your Dragon|P/DW|217600000\.0|277300000|2010|

## Data Cleaning

- The next step is to clean the data to ensure it does not have missing values and drop any if need be. In this case we will check how the data looks using df.info() then in case of missing values, we will do data

 df . info () 

From the above result, the columns do not all have 3387 entries, which shows we have some missing data,let's check the number of entries in our columns with missing data

# Check for missing values in all columns![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.005.png)*

print(df.isnull().sum()) 

# Explore the 'foreign\_gross' column specifically![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.006.png)*

print(df['foreign\_gross'].unique()) 

 The answer is 1205

From the above code, we have 1205 entries to work with, this is an okay number to use as a sample for

investigation and analysis, we can therefore go ahead and fill in the missing values in order to conduct analysis on the data

# filling in the missing values with 0

df['foreign\_gross'] = df['foreign\_gross'].fillna(0) df['foreign\_gross'].isnull().sum() 
The output should be 0

 df . dropna ( subset =[ 'foreign\_gross' ],  inplace =True )

 **import**  numpy  **as**  np 

df['foreign\_gross'] = pd.to\_numeric(df['foreign\_gross'],errors='coerce') print(df['foreign\_gross'].dtype) 

float64

As seen above, we have successfully drpped any entry that had text and we can now perform our operations.

# finding the mean and median of foreign\_gross
The average foreign gross is 45 Million, let us do the same for domestic gross in order to see where we get more earnings.

The next step is to focus on the domestic\_gross since it has few missing values: we will drop the missing values

- We will work with domestic\_gross and see if it is doing better than foreign\_gross
- We will drop the missing values NaN
  The average domestic gross earnings are 28.7 Million, we can therefore conclude that foreign gross earnings are higher.

1st deduction from this data: What is made from foreign gross is higher that what is made from domestic gross, therefore some conclusions that can be made is to:

- Focus on International Appeal:Consider stories and themes that can resonate with a global audience, transcending cultural barriers.This could involve universal themes like love, family, adventure, or stories with minimal dialogue.
- Co-productions: Partner with international production companies to leverage their expertise in foreign markets and potentially gain access to financing and distribution channels i.e we can check another data set to see which studio is doing well and how is their foreign gross in order to consider partnerships.
- Domestic Market Value: Don't neglect the domestic market entirely. A strong domestic performance can still be valuable and contribute to a film's overall success.

## Exploratory Data Analysis

### Descriptive Statistics

In this section, we analyse the mean, median and other measures of central tendencies

- We will calculate the mean and median to find out the average gross earned from the movies

This shows the average of foreign gross earnings is 45 million while that of domestic gross earning from a movie is 28.7 million, let us put the individual earnings of the top earning movies to have a picture of the movies that are doing well so as to understand which structure in terns of theme etc. , will give better results.

-We will start with visualization of the domestic gross to see which movie had the highest domestic gross earning

- Visualization of the data on a graph
- 
## Create the bar graph
Star Wars: The Force Awakens is the best perfoeming in terms of domestic gross, it is a space opera infused with action-adventure, fantasy, and coming-of-age elements. It explores a central theme of good versus evil, along with themes of family, hope, rebellion, and the consequences of violence.

-Let us now visualization foreign gross to see which movie had the highest foreign gross earning

# Create the bar graph

Harry Potter and the Deathly Hallows Part 2 is the best performing in terms of foreign gross, while classified as fantasy, Harry Potter and the Deathly Hallows incorporates elements of coming-of-age, dark fantasy, and explores themes that resonate with readers beyond the fantastical setting.

## Comparison between foreign\_gross and domestic gross

We can use a stacked graph to see the difference in domestic and gross earnings for a sample of the movies

 **import**  seaborn  **as**  sns 
crerate a bargraph,
-From the above bargraph,a higher percentage of the gross earnings for most of these movies come from foreign markets compared to domestic markets

  ## Exploring another data set

  We want to have more comparison for accurate recommendations, we therefore chose another data set under [tmdb.movies.csv](file:///tmp/tmdb.movies.csv)

data = pd.read\_csv('tmdb.movies.csv') 
check the dataframe using:
data.head() 

Check which columns in bom.movie have similar columns with tmdb.csv for merging, in order to be able to analyse the dataframe

Next let us merge the two dataframes on the basis that they have the 'title' column in both 'df' and 'data' Dataframes

joined\_tmdb = data.merge(df,on='title',how='left') 

Check for any missing values fill with 0

### Let's now check on popularity of the movies The mean of the popularity column will be:

The popularity we should aim for is the highest which is at around 80, let's visualize this in order to see which movie did well in terms of popularity

 Avengers:Infinity War did well in terms of popularity, what can be borrowed from Avengers: Infinity War's success in terms of popularity:Focus on established and popular genres: Superhero films are a proven box-office draw, but other established genres like action, animation, or sci-fi with strong narratives can also be successful.

### Let's now compare popularity of the movie to the foreign gross since we have seen more earnings come from foreign gross

This visualization helps in understanding the relative performance of movies in terms of their foreign gross earnings and popularity, normalized to a common scale for easier comparison.

- Let's check whether an increase in popularity affects the foreign gross using correlation

  ### Calculate the Pearson correlation coefficient*

correlation = top\_10[['foreign\_gross', 'popularity']].corr() print(correlation) 
 Plot a scatter plot
 
It shows a weak relationship between foreign gross and popularity: The scatter plot suggests that popularity may not be a strong predictor of foreign gross. Other factors could be more significant in determining foreign gross.

Let us now compare popularity in terms of studio to see which studio has more popular movies, in case any co-productions, which studios should be considered.

Create the bar graph

### BV studio is doing well in terms of popularity, therefore can be the first consideration in case of any co-productions

Another comparison on popularity can be in terms of the season when is it best to release the movie. Let us compare release dates to popularit
Create the bar graph*

The highest popularity values are concentrated in specific periods, such as late April 2018 and late October 2014. The pattern might suggest that movies released during certain times of the year (e.g., spring or fall) could have higher popularity, potentially due to factors like holidays, school vacations, or award season considerations.

## Exploring another data set

We want to have more comparison, we therefore chose another data set under [tn.movie_budgets.csv](file:///tmp/tn.movie_budgets.csv)

df\_budget = pd.read\_csv('tn.movie\_budgets.csv') 

df\_budget.head() 

Next thing is to compare the production budget and the worldwide gross to know whether there is any return on investment.

To calculate the Return on Investment (ROI) for movies, you can use the following formula: ROI = (Worldwide Gross − Production Budget)/Production Budget

- Positive ROI: Movies with positive ROI values (e.g., Avatar, Pirates of the Caribbean...) generated more revenue than their production costs. The higher the ROI, the greater the return on investment.
- Negative ROI: Movies with negative ROI values (e.g., Dark Phoenix) had production costs exceeding their worldwide gross, resulting in a financial loss. The more negative the ROI, the larger the financial loss relative to the investment.

Let's do a visualization of the ROI against production budget to see for instance, high-budget movies might have a higher risk of financial loss even with a positive ROI due to a larger initial investment.
  X-axis: The x-axis labels represent the movie titles in the top 10 (based on production budget). The labels are rotated 90 degrees for better readability.

  Y-axes: There are two y-axes in this chart:

- Left Y-axis (blue): This axis represents the Production Budget for each movie. The scale is likely in millions or billions of dollars, depending on your data. The blue bars show the production budget for each movie.
- Right Y-axis (yellow): This axis represents the ROI for each movie. Since the ROI values are normalized between 0 and 1, the scale doesn't have specific units but allows for easy comparison between movies on the same scale as the production budget. The yellow bars show the normalized ROI for each movie.

From the graph: The ROI values (yellow bars) vary across the movies. Some movies with high production budgets also have high normalized ROI (e.g., Avatar), indicating they generated a significant return on investment compared to their production costs. Other movies might have high production budgets but lower ROI (e.g., Dark Phoenix), suggesting they may not have recouped their costs as effectively.

Let's have a representation with bar graphs and a line graph for the ROI for better visualization


## Recommendations and actionable insights

From the above calculations and visualization, the following conclusions can be drawn

- The earnings made from foreign gross are higher than what is made on domestic gross, which means more investment can be made towards (since the foreign gross is 45 Million and the domestic gross is 28.7M)
- The movie that had the highest domestic gross earning was Star Wars: The Force Awakens is the best perfoeming in terms of domestic gross, it is a space opera infused with action-adventure, fantasy, and coming-of-age elements. It explores a central theme of good versus evil, along with themes of family, hope, rebellion, and the consequences of violence.
- The movie that had the highest domestic gross earning was Harry Potter and the Deathly Hallows Part 2 is the best performing in terms of foreign gross, while classified as fantasy, Harry Potter and the Deathly Hallows incorporates elements of coming-of-age, dark fantasy, and explores themes that resonate with readers beyond the fantastical setting.
- Comparison between foreign\_gross and domestic gross shows a higher percentage of the gross earnings for most of these movies come from foreign markets compared to domestic markets.
- The popularity of movies we should aim for is the highest which is at around 80, Avengers:Infinity War did well in terms of popularity, what can be borrowed from Avengers: Infinity War's success in terms of popularity:Focus on established and popular genres: Superhero films are a proven box-office draw, but other established genres like action, animation, or sci-fi with strong narratives can also be successful.
- Checking on whether popularity is affected by foreign gross, shows a weak relationship between foreign gross and popularity: The scatter plot suggests that popularity may not be a strong predictor of foreign gross. Other factors could be more significant in determining foreign gross.
- Comparing popularity in terms of studio to see which studio has more popular movies, in case any co- productions, which studios should be considered, we see that BV studio is doing well in terms of popularity, therefore can be the first consideration in case of any co-productions
- Another comparison on popularity can be in terms of the season when is it best to release the movie. The highest popularity values are concentrated in specific periods, such as late April 2018 and late October 2014. The pattern might suggest that movies released during certain times of the year (e.g., spring or fall) could have higher popularity, potentially due to factors like holidays, school vacations, or award season considerations.
- Comparing the production budget and the worldwide gross to know whether there is any return on investment, to calculate the Return on Investment (ROI) for movies,The ROI values vary across the movies. Some movies with high production budgets also have high normalized ROI (e.g., Avatar), indicating they generated a significant return on investment compared to their production costs. Other movies might have high production budgets but lower ROI (e.g., Dark Phoenix), suggesting they may not have recouped their costs as effectively.

Recommendations to Microsoft:

- Focus on established and popular genres: Superhero films are a proven box-office draw, but other established genres like action, animation, or sci-fi with strong narratives can also be successful.
- Focus release dates based on seasons such as holidays,The highest popularity values are concentrated in specific periods, such as late April 2018 and late October 2014.The pattern might suggest that movies released during certain times of the year (e.g., spring or fall) could have higher popularity, potentially due to factors like holidays, school vacations, or award season considerations.
- Increased production budget may mean an increase in the earnings but not for all movies.The ROI values vary across the movies. Some movies with high production budgets also have high normalized ROI (e.g., Avatar), indicating they generated a significant return on investment compared to their production costs while others did not return the profits, therefore production budget is not directly proportional to gross earnings for all movies.
