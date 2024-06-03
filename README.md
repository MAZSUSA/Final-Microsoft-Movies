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

In [2]: df  **=**  pd**.** read\_csv ( 'bom.movie\_gross.csv' ) 

df**.**head(10) 

Out[2]: **title studio domestic\_gross foreign\_gross year**



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

cleaning In [3]: df **.** info () 

<class 'pandas.core.frame.DataFrame'> RangeIndex: 3387 entries, 0 to 3386

Data columns (total 5 columns):

- Column          Non-Null Count  Dtype  

---  ------          --------------  -----   0   title           3387 non-null   object  1   studio          3382 non-null   object  2   domestic\_gross  3359 non-null   float64  3   foreign\_gross   2037 non-null   object  4   year            3387 non-null   int64  dtypes: float64(1), int64(1), object(3) memory usage: 132.4+ KB

From the above result, the columns do not all have 3387 entries, which shows we have some missing data,let's check the number of entries in our columns with missing data

In [4]: *# Check for missing values in all columns![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.005.png)*

print(df**.**isnull()**.**sum()) 

title                0 studio               5 domestic\_gross      28 foreign\_gross     1350 year                 0 dtype: int64

The foreign\_gross column has a significant number of missing values, let's check to understand the inconsistencies that this may come with

In [5]: *# Explore the 'foreign\_gross' column specifically![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.006.png)*

print(df['foreign\_gross']**.**unique())  *# View unique values to understand inconsistencies* len(df['foreign\_gross']**.**unique())  *# Number of unique values* 

['652000000' '691300000' '664300000' ... '530000' '256000' '30000']

Out[5]: 1205

From the above code, we have 1205 entries to work with, this is an okay number to use as a sample for

investigation and analysis, we can therefore go ahead and fill in the missing values in order to conduct analysis on the data

In [6]: *# filling in the missing values with 0![ref5]*

df['foreign\_gross'] **=** df['foreign\_gross']**.**fillna(0) df['foreign\_gross']**.**isnull()**.**sum() 

Out[6]: 0

In [7]: *# Check whether the column has been filled correctly ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.008.png)*print(df['foreign\_gross']) *#excpected entries are 3387*

0       652000000 1       691300000

2       664300000

3       535700000

4       513900000

...    

3382            0

3383            0

3384            0

3385            0

3386            0

Name: foreign\_gross, Length: 3387, dtype: object

In order to conduct any operations on the data, let us check whether it is a float or an integer since we want to do some arithmetic operations. Let's work on the foreign\_gross column statistics

In [8]: print ( df [ 'foreign\_gross' ] **.** dtype ) 

object

The code above means the data has some text and therefore we still need to clean by removing the objects

In [9]: df **.** dropna ( subset **=**[ 'foreign\_gross' ],  inplace **=True** )

print(df['foreign\_gross']**.**dtype) 

object

In [10]: **import**  numpy  **as**  np ![ref3]

df['foreign\_gross'] **=** pd**.**to\_numeric(df['foreign\_gross'],errors**=**'coerce') print(df['foreign\_gross']**.**dtype) 

float64

As seen above, we have successfully drpped any entry that had text and we can now perform our operations.

In [11]: *#finding the mean and median of foreign\_gross![ref2]*

mean\_foreign\_gross\_earning **=** df['foreign\_gross']**.**mean() median\_foreign\_gross\_earning **=** df['foreign\_gross']**.**median() print(f'mean\_foreign\_gross\_earning: {mean\_foreign\_gross\_earning}') print(f'median\_foreign\_gross\_earning: {median\_foreign\_gross\_earning}') 

mean\_foreign\_gross\_earning: 45096365.63660556 median\_foreign\_gross\_earning: 1500000.0

Above , we have seen that the average foreign gross is 45 Million, let us do the same for domestic gross in order to see where we get more earnings.

The next step is to focus on the domestic\_gross since it has few missing values: we will drop the missing values

- We will work with domestic\_gross and see if it is doing better than foreign\_gross
- We will drop the missing values NaN

In [12]: *#Dropping the missing values in the domestic\_gross column ![ref7]*

df**.**dropna(subset**=**['domestic\_gross'], inplace**=True**) *#inplace = True to maintain the initi #check the if all missing values have been dropped*

df['domestic\_gross']**.**isnull()**.**sum()  

Out[12]: 0

Since we have dropped all the missing values, we can go ahead and perform arithmetic operations on the domestic gross column

In [13]: *#mean of the domestic\_gross![ref8]*

mean\_domestic\_gross\_earning **=** df['domestic\_gross']**.**mean() print(f'mean\_domestic\_gross\_earning: {mean\_domestic\_gross\_earning}') *#median of the domestic\_gross*

median\_domestic\_gross\_earning **=** df['domestic\_gross']**.**median() print(f'median\_domestic\_gross\_earning: {median\_domestic\_gross\_earning}') 

mean\_domestic\_gross\_earning: 28745845.06698422 median\_domestic\_gross\_earning: 1400000.0

As seen above, the average domestic gross earnings are 28.7 Million, we can therefore conclude that foreign gross earnings are higher.

1st deduction from this data: What is made from foreign gross is higher that what is made from domestic gross, therefore some conclusions that can be made is to:

- Focus on International Appeal:Consider stories and themes that can resonate with a global audience, transcending cultural barriers.This could involve universal themes like love, family, adventure, or stories with minimal dialogue.
- Co-productions: Partner with international production companies to leverage their expertise in foreign markets and potentially gain access to financing and distribution channels i.e we can check another data set to see which studio is doing well and how is their foreign gross in order to consider partnerships.
- Domestic Market Value: Don't neglect the domestic market entirely. A strong domestic performance can still be valuable and contribute to a film's overall success.

## Exploratory Data Analysis

### Descriptive Statistics

In this section, we analyse the mean, median and other measures of central tendencies

- We will calculate the mean and median to find out the average gross earned from the movies

In [14]: *#mean of the domestic\_gross![ref7]*

print(f'mean\_domestic\_gross\_earning: {mean\_domestic\_gross\_earning}') *#median of the domestic\_gross*

print(f'median\_domestic\_gross\_earning: {median\_domestic\_gross\_earning}') 

mean\_domestic\_gross\_earning: 28745845.06698422 median\_domestic\_gross\_earning: 1400000.0

In [15]: *#finding the mean and median of foreign\_gross![ref9]*

print(f'mean\_foreign\_gross\_earning: {mean\_foreign\_gross\_earning}') print(f'median\_foreign\_gross\_earning: {median\_foreign\_gross\_earning}') 

mean\_foreign\_gross\_earning: 45096365.63660556 median\_foreign\_gross\_earning: 1500000.0

This shows the average of foreign gross earnings is 45 million while that of domestic gross earning from a movie is 28.7 million, let us put the individual earnings of the top earning movies to have a picture of the movies that are doing well so as to understand which structure in terns of theme etc. , will give better results.

-We will start with visualization of the domestic gross to see which movie had the highest domestic gross earning

- Visualization of the data on a graph

In [17]: *# Sort together by domestic gross ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.013.png)*

sorted\_df **=** df**.**sort\_values(by**=**'domestic\_gross', ascending**=False**)  *# Sort high to low gro*

- *Select top 10*

top\_10\_df **=** sorted\_df**.**head(10)  *# Get the first 10 rows (top 10 grossing)*

- *Create the bar graph*

plt**.**figure(figsize**=**(10, 6))  *# Set figure size*

plt**.**bar(top\_10\_df['title'], top\_10\_df['domestic\_gross'], color**=**'skyblue')  *# Plot bars wi* plt**.**xlabel('Movie Titles')  *# Label for x-axis*

plt**.**ylabel('Domestic Gross')  *# Label for y-axis* plt**.**title('Top 10 Grossing Movies (Domestic)')  *# Title for the graph*

plt**.**xticks(rotation**=**45, ha**=**'right')  *# Rotate if titles are long*

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.014.png)

Star Wars: The Force Awakens is the best perfoeming in terms of domestic gross, it is a space opera infused with action-adventure, fantasy, and coming-of-age elements. It explores a central theme of good versus evil, along with themes of family, hope, rebellion, and the consequences of violence.

-Let us now visualization foreign gross to see which movie had the highest foreign gross earning

In [18]: *# Sort together by foreign gross ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.015.png)*

sorted\_foreign\_df **=** df**.**sort\_values(by**=**'foreign\_gross', ascending**=False**)  *# Sort high to l*

- *Select top 10![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.016.png)*

top\_10\_foreign\_df **=** sorted\_foreign\_df**.**head(10)  *# Get the first 10 rows (top 10 grossing* top\_10\_foreign\_df

Out[18]: **title studio domestic\_gross foreign\_gross year**



|**328**|Harry Potter and the Deathly Hallows Part 2|WB|381000000\.0|960500000\.0|2011|
| - | - | - | - | - | - |
|**1875**|Avengers: Age of Ultron|BV|459000000\.0|946400000\.0|2015|
|**727**|Marvel's The Avengers|BV|623400000\.0|895500000\.0|2012|
|**3081**|Jurassic World: Fallen Kingdom|Uni.|417700000\.0|891800000\.0|2018|
|**1127**|Frozen|BV|400700000\.0|875700000\.0|2013|
|**2764**|Wolf Warrior 2|HC|2700000\.0|867600000\.0|2017|
|**1477**|Transformers: Age of Extinction|Par.|245400000\.0|858600000\.0|2014|
|**1876**|Minions|Uni.|336000000\.0|823400000\.0|2015|
|**3083**|Aquaman|WB|335100000\.0|812700000\.0|2018|
|**1128**|Iron Man 3|BV|409000000\.0|805800000\.0|2013|

In [19]: *# Create the bar graph![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.017.png)*

plt**.**figure(figsize**=**(10, 6))  *# Set figure size*

plt**.**bar(top\_10\_foreign\_df['title'], top\_10\_foreign\_df['foreign\_gross'], color**=**'skyblue') plt**.**xlabel('Movie Titles')  *# Label for x-axis*

plt**.**ylabel('Foreign Gross')  *# Label for y-axis* plt**.**title('Top 10 Grossing Movies (Foreign)')  *# Title for the graph*

plt**.**xticks(rotation**=**45, ha**=**'right')  *# Rotate if titles are long*

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.018.png)

Harry Potter and the Deathly Hallows Part 2 is the best performing in terms of foreign gross, while classified as fantasy, Harry Potter and the Deathly Hallows incorporates elements of coming-of-age, dark fantasy, and explores themes that resonate with readers beyond the fantastical setting.

-Comparison between foreign\_gross and domestic gross

In [20]: *#first let's find the top 10 values that we will compare![ref9]*

- *top\_10\_foreign\_df = df.sort\_values(by='foreign\_gross', ascending=False).head(10)* top\_10\_foreign\_df

Out[20]: **title studio domestic\_gross foreign\_gross year![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.019.png)**



|**328**|Harry Potter and the Deathly Hallows Part 2|WB|381000000\.0|960500000\.0|2011|
| - | - | - | - | - | - |
|**1875**|Avengers: Age of Ultron|BV|459000000\.0|946400000\.0|2015|
|**727**|Marvel's The Avengers|BV|623400000\.0|895500000\.0|2012|
|**3081**|Jurassic World: Fallen Kingdom|Uni.|417700000\.0|891800000\.0|2018|
|**1127**|Frozen|BV|400700000\.0|875700000\.0|2013|
|**2764**|Wolf Warrior 2|HC|2700000\.0|867600000\.0|2017|
|**1477**|Transformers: Age of Extinction|Par.|245400000\.0|858600000\.0|2014|
|**1876**|Minions|Uni.|336000000\.0|823400000\.0|2015|
|**3083**|Aquaman|WB|335100000\.0|812700000\.0|2018|
|**1128**|Iron Man 3|BV|409000000\.0|805800000\.0|2013|

In [21]: *#let's see if both columns are workable in terms of arithmetic operations![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.020.png)*

domestic\_type **=** print(df['domestic\_gross']**.**dtype) 

foreign\_type **=** print(df['foreign\_gross']**.**dtype) 

print(domestic\_type) print(foreign\_type) 

float64 float64 None None 

We can use a stacked graph to see the difference in domestic and gross earnings for a sample of the movies

In [22]: **import**  seaborn  **as**  sns ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.021.png)

- *top\_10\_joined\_tmdb = df.sort\_values(by='foreign\_gross', ascending=False).head(10)*
- *Ensure 'foreign\_gross' exists in 'df'*

plt**.**figure(figsize**=**(10,6)) 

*#plot for foreign\_gross*

sns**.**barplot(data**=**top\_10\_foreign\_df, x**=**'title', y**=**'foreign\_gross', color**=**'skyblue', label *#plot for domestic\_gross*

sns**.**barplot(data**=**top\_10\_foreign\_df, x**=**'title', y**=**'domestic\_gross',color**=**'orange', label**=**

plt**.**xlabel('Title') plt**.**ylabel('Gross Earnings') plt**.**title('Foreign Gross vs Domestic Gross') plt**.**legend() 

plt**.**xticks(rotation**=**90) 

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.022.png)

-From the above bargraph,a higher percentage of the gross earnings for most of these movies come from foreign markets compared to domestic markets.

Let's have another graph to show this relation:

In [23]: *# top\_10\_foreign\_df = df.sort\_values(by='foreign\_gross', ascending=False).head(10)![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.023.png)*

melted\_top\_10\_df **=** top\_10\_foreign\_df**.**melt(id\_vars**=**'title', value\_vars**=**['foreign\_gross', *#plot figure*

plt**.**figure(figsize**=**(10,6)) 

sns**.**barplot(data**=**melted\_top\_10\_df, x**=**'title', y**=**'gross', hue**=**'gross\_type') 

- *Customize the plot*

plt**.**xlabel('Title') plt**.**ylabel('Gross Earnings') plt**.**title('Foreign Gross vs Domestic Gross') plt**.**xticks(rotation**=**90) plt**.**legend(title**=**'Gross Type') 

- *Show the plot* plt**.**show() 

  ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.024.png)

  Exploring another data set

  We want to have more comparison for accurate recommendations, we therefore chose another data set under [tmdb.movies.csv](file:///tmp/tmdb.movies.csv)

In [24]: *#Reading the data from tmdb.csv![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.025.png)*

data **=** pd**.**read\_csv('tmdb.movies.csv') 

data**.**head() 

Out[24]: **Unnamed: genre\_ids id original\_language original\_title popularity release\_date title vote\_averag**

**0**



|**0**|0|[12, 14, 10751]|12444|en|<p>Harry Potter and the</p><p>Deathly Hallows: Part</p><p>1</p>|33\.533|2010-11-19|<p>Harry Potter</p><p>and the Deathly Hallows: Part 1</p>|7|
| - | - | - | - | - | -: | - | - | - | - |
|**1**|1|[14, 12, 16, 10751]|10191|en|How to Train Your Dragon|28\.734|2010-03-26|How to Train Your Dragon|7|
|**2**|2|[12, 28, 878]|10138|en|Iron Man 2|28\.515|2010-05-07|Iron Man 2|6|
|||||||||||
**3** 3 [16, 35, 862 en Toy Story 28.005 1995-11-22 Toy 7![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.026.png)

10751] Story

[28, 878,

**4** 4 27205 en Inception 27.920 2010-07-16 Inception 8

12]

Let's check which columns in the bom.movie\_gross.csv have similar columns with tmdb.csv for merging, in order to be able to analyse the dataframes

In [25]: *#display the dataframe for bom\_movies.csv![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.027.png)*

df**.**head(1) 

Out[25]: **title studio domestic\_gross foreign\_gross year![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.028.png)**

**0** Toy Story 3 BV 415000000.0 652000000.0 2010

In [192 … *#display the data for tmbd.csv![ref6]*

data**.**head(1) 

Out[192]: **Unnamed: genre\_ids id original\_language original\_title popularity release\_date title vote\_avera**

**0**



|**0**|0|[12, 14, 10751]|12444|en|<p>Harry Potter and the</p><p>Deathly Hallows: Part</p><p>1</p>|33\.533|2010-11-19|<p>Harry Potter</p><p>and the Deathly Hallows: Part 1</p>|7|
| - | - | - | - | - | -: | - | - | - | - |

Next let us merge the two dataframes on the basis that they have the 'title' column in both 'df' and 'data' Dataframes

In [26]: *# Merge DataFrame![ref8]*

joined\_tmdb **=** data**.**merge(df,on**=**'title',how**=**'left') 

*#For any missing values fill with 0*

joined\_tmdb**.**fillna(0,inplace**=True**) Change the comment below to a code(ctrl + ?) in order to download the merged csv, I've left it as a

comment to avoid multiple downloads of the file each time I Run All

In [28]: *# joined\_tmdb.to\_csv('Merged\_data.csv')![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.029.png)*

In [29]: *#Read the joined csv![ref9]*

joined\_tmdb**=** pd**.**read\_csv('Merged\_data.csv') 

joined\_tmdb**.**head() 

Out[29]: **Unnamed: Unnamed:**

**genre\_ids id original\_language original\_title popularity release\_date title 0 0.1**



|**0**|0|0|[12, 14, 10751]|12444|en|<p>Harry Potter and the</p><p>Deathly Hallows: Part</p><p>1</p>|33\.533|2010-11-19|<p>Harry Potter</p><p>and the Deathly Hallows: Part 1</p>||
| - | - | - | - | - | - | -: | - | - | - | :- |
|**1**|1|1|[14, 12, 16, 10751]|10191|en|How to Train Your Dragon|28\.734|2010-03-26|How to Train Your Dragon||
||||||||||||
|**2**|2|2|[12, 28, 878]|10138|en|Iron Man 2|28\.515|2010-05-07|Iron Man 2||
|**3**|3|3|[16, 35, 10751]|862|en|Toy Story|28\.005|1995-11-22|Toy Story||
|**4**|4|4|<p>[28, 878,</p><p>12]</p>|27205|en|Inception|27\.920|2010-07-16|Inception||
Let's now check on popularity of the movies The mean of the popularity column will be:

In [30]: joined\_tmdb\_mean  **=**  joined\_tmdb [ 'popularity' ] **.** mean () ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.030.png)

joined\_tmdb\_max **=** joined\_tmdb['popularity']**.**max() joined\_tmdb\_min **=** joined\_tmdb['popularity']**.**min() print(f'joined\_tmdb\_mean: {joined\_tmdb\_mean}') print(f'joined\_tmdb\_max: {joined\_tmdb\_max}') print(f'joined\_tmdb\_min: {joined\_tmdb\_min}') 

joined\_tmdb\_mean: 3.130912244974922 joined\_tmdb\_max: 80.773 joined\_tmdb\_min: 0.6

The popularity we should aim for is the highest which is at around 80, let's visualize this in order to see which movie did well in terms of popularity

In [31]: top\_10\_popular\_movies  **=**  joined\_tmdb **.** sort\_values ( by**=**'popularity' , ascending **=False** )[: 10] ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.031.png)

plt**.**figure(figsize**=**(14,6)) 

plt**.**bar(data**=**top\_10\_popular\_movies, x**=**'title', height**=**'popularity') plt**.**xlabel('Popularity') 

plt**.**ylabel('title') 

plt**.**xticks(rotation**=**90, ha**=**'right') 

plt**.**title('Popularity By Movies') 

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.032.png)

As seen above, Avengers:Infinity War did well in terms of popularity, what can be borrowed from Avengers: Infinity War's success in terms of popularity:Focus on established and popular genres: Superhero films are a proven box-office draw, but other established genres like action, animation, or sci-fi with strong narratives can also be successful.

Let's now compare popularity of the movie to the foreign gross since we have seen more earnings come from foreign gross

This visualization helps in understanding the relative performance of movies in terms of their foreign gross earnings and popularity, normalized to a common scale for easier comparison.

- Let's check whether an increase in popularity affects the foreign gross using correlation

In [32]: **import**  seaborn  **as**  sns ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.033.png)

**import** matplotlib.pyplot **as** plt 

- *Sort and select top 10 rows by 'foreign\_gross'*

top\_10 **=** joined\_tmdb**.**sort\_values(by**=**'foreign\_gross', ascending**=False**) top\_10 

- *# Calculate the Pearson correlation coefficient*

correlation **=** top\_10[['foreign\_gross', 'popularity']]**.**corr() print(correlation) 

- *# Plot a scatter plot*

plt**.**figure(figsize**=**(10, 6)) 

sns**.**scatterplot(data**=**top\_10, x**=**'popularity', y**=**'foreign\_gross') plt**.**xlabel('Popularity') 

plt**.**ylabel('Foreign Gross') ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.034.png)

- *Set the y-axis to log scale if needed*

plt**.**yscale('log') plt**.**title('Scatter Plot of Popularity vs Foreign Gross') plt**.**show() 

`            `popularity popularity         1.0

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.035.png)

This shows a weak relationship between foreign gross and popularity: The scatter plot suggests that popularity may not be a strong predictor of foreign gross. Other factors could be more significant in determining foreign gross.

Let us now compare popularity in terms of studio to see which studio has more popular movies, in case any co-productions, which studios should be considered.

In [34]: *#sort by popularity![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.036.png)*

top\_studio **=** joined\_tmdb**.**sort\_values(by**=**'popularity', ascending**=False**)  *# Sort popularity*

- *Select top 10*

top\_studio\_10 **=** top\_studio**.**head(10)  *# Get the first 10 rows* 

top\_studio\_10

- *Create the bar graph*

plt**.**figure(figsize**=**(10, 6))  *# Set figure size*

plt**.**bar(data**=**top\_studio\_10, x**=**'studio', height**=**'popularity', color**=**'skyblue')  *# Plot ba* plt**.**xlabel('Studio')  *# Label for x-axis*

plt**.**ylabel('Popularity')  *# Label for y-axis* plt**.**title('Popularity in relation to Studio')  *# Title for the graph*

plt**.**xticks(rotation**=**45, ha**=**'right')  *# Rotate if titles are long*

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.037.png)

Above, we can see that BV studio is doing well in terms of popularity, therefore can be the first consideration in case of any co-productions

Another comparison on popularity can be in terms of the season when is it best to release the movie. Let us compare release dates to popularity

In [35]: *#  top\_10\_df = sorted\_df.head(10)  # Get the first 10 rows (top 10 grossing)![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.023.png)*

joined\_tmdb**.**dropna(subset**=**['release\_date'], inplace**=True**) 

release\_month **=** pd**.**to\_datetime(joined\_tmdb**.**release\_date)**.**dt**.**to\_period('M')**.**dt**.**to\_timesta *#sort by popularity*

month\_by\_popularity **=** joined\_tmdb**.**sort\_values(by**=**'popularity', ascending**=False**)  *# Sort p*

- *Select top 10*

month\_by\_popularity\_10 **=** month\_by\_popularity**.**head(10)  *# Get the first 10 rows* 

- *Create the bar graph*

plt**.**figure(figsize**=**(10, 6))  *# Set figure size*

plt**.**bar(data**=**month\_by\_popularity\_10, x**=**'release\_date', height**=**'popularity', color**=**'skybl plt**.**xlabel('release\_month')  *# Label for x-axis*

plt**.**ylabel('Popularity')  *# Label for y-axis* plt**.**title('Popularity in relation to Release Date')  *# Title for the graph*

plt**.**xticks(rotation**=**45, ha**=**'right')  *# Rotate if titles are long*

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.038.png)

The highest popularity values are concentrated in specific periods, such as late April 2018 and late October 2014. The pattern might suggest that movies released during certain times of the year (e.g., spring or fall) could have higher popularity, potentially due to factors like holidays, school vacations, or award season considerations.

Exploring another data set

We want to have more comparison, we therefore chose another data set under [tn.movie_budgets.csv](file:///tmp/tn.movie_budgets.csv)

In [36]: *#Reading the csv file![ref5]*

df\_budget **=** pd**.**read\_csv('tn.movie\_budgets.csv') 

df\_budget**.**head() 

Out[36]: **id release\_date movie production\_budget domestic\_gross worldwide\_gross**



|**0**|1|Dec 18, 2009|Avatar|$425,000,000|$760,507,625|$2,776,345,279|
| - | - | - | - | - | - | - |
|**1**|2|May 20, 2011|<p>Pirates of the Caribbean: On</p><p>Stranger Tides</p>|$410,600,000|$241,063,875|$1,045,663,875|
|**2**|3|Jun 7, 2019|Dark Phoenix|$350,000,000|$42,762,350|$149,762,350|
|**3**|4|May 1, 2015|Avengers: Age of Ultron|$330,600,000|$459,005,868|$1,403,013,963|
|**4**|5|Dec 15, 2017|Star Wars Ep. VIII: The Last Jedi|$317,000,000|$620,181,382|$1,316,721,747|

Next thing is to compare the production budget and the worldwide gross to know whether there is any return on investment.

To calculate the Return on Investment (ROI) for movies, you can use the following formula: ROI = (Worldwide Gross − Production Budget)/Production Budget

In [37]: *# Convert columns to numeric after removing commas![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.039.png)*

df\_budget['worldwide\_gross'] **=** df\_budget['worldwide\_gross']**.**astype(str)**.**str**.**replace('[$, df\_budget['production\_budget'] **=** df\_budget['production\_budget']**.**astype(str)**.**str**.**replace( df\_budget['worldwide\_gross'] **=** pd**.**to\_numeric(df\_budget['worldwide\_gross']) df\_budget['production\_budget'] **=** pd**.**to\_numeric(df\_budget['production\_budget']) df\_budget['ROI'] **=** (df\_budget['worldwide\_gross'] **-** df\_budget['production\_budget']) **/** df\_ df\_budget**.**head(10) 

Out[37]: **id release\_date movie production\_budget domestic\_gross worldwide\_gross ROI**



|**0**|1|Dec 18, 2009|Avatar|425000000\.0|$760,507,625|2\.776345e+09|5\.532577|
| - | - | - | - | - | - | - | - |
|**1**|2|May 20, 2011|Pirates of the Caribbean: On Stranger Tides|410600000\.0|$241,063,875|1\.045664e+09|1\.546673|
|**2**|3|Jun 7, 2019|Dark Phoenix|350000000\.0|$42,762,350|1\.497624e+08|-0.572108|
|**3**|4|May 1, 2015|<p>Avengers: Age of</p><p>Ultron</p>|330600000\.0|$459,005,868|1\.403014e+09|3\.243841|
|**4**|5|Dec 15, 2017|<p>Star Wars Ep. VIII: The</p><p>Last Jedi</p>|317000000\.0|$620,181,382|1\.316722e+09|3\.153696|
|**5**|6|Dec 18, 2015|Star Wars Ep. VII: The Force Awakens|306000000\.0|$936,662,225|2\.053311e+09|5\.710167|
|**6**|7|Apr 27, 2018|Avengers: Infinity War|300000000\.0|$678,815,482|2\.048134e+09|5\.827114|
|**7**|8|May 24, 2007|Pirates of the Caribbean: At Worldâ   s End|300000000\.0|$309,420,425|9\.634204e+08|2\.211401|
|**8**|9|Nov 17, 2017|Justice League|300000000\.0|$229,024,295|6\.559452e+08|1\.186484|
|**9**|10|Nov 6, 2015|Spectre|300000000\.0|$200,074,175|8\.796209e+08|1\.932070|



- Positive ROI: Movies with positive ROI values (e.g., Avatar, Pirates of the Caribbean...) generated more revenue than their production costs. The higher the ROI, the greater the return on investment.
- Negative ROI: Movies with negative ROI values (e.g., Dark Phoenix) had production costs exceeding their worldwide gross, resulting in a financial loss. The more negative the ROI, the larger the financial loss relative to the investment.

Let's do a visualization of the ROI against production budget to see for instance, high-budget movies might

have a higher risk of financial loss even with a positive ROI due to a larger initial investment.

In [38]: *#Check for any missing data![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.040.png)*

print(df\_budget['ROI']**.**isnull()**.**sum()) 

0 

In [39]: **import**  warnings![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.041.png)

warnings**.**filterwarnings("ignore", message**=**".\*Glyph .\* missing from current font.\*") top\_10\_df\_budget **=** df\_budget**.**sort\_values(by**=**'production\_budget', ascending**=False**)**.**head(1

min\_roi **=** df\_budget['ROI']**.**min() 

max\_roi **=** df\_budget['ROI']**.**max() 

df\_budget['normalized\_ROI'] **=** (df\_budget['ROI'] **-** min\_roi) **/** (max\_roi **-** min\_roi) 

- *# Plot*

fig, ax1 **=** plt**.**subplots(figsize**=**(10, 6)) 

sns**.**barplot(data**=**top\_10\_df\_budget, x**=**'movie', y**=**'production\_budget', ax**=**ax1, color**=**'blue ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.042.png)ax2 **=** ax1**.**twinx() 

sns**.**barplot(data**=**top\_10\_df\_budget, x**=**'movie', y**=**'ROI', ax**=**ax2, color**=**'orange', label**=**'RO

ax1**.**set\_xlabel('Movie') 

ax1**.**set\_ylabel('Production Budget', color**=**'blue') 

ax2**.**set\_ylabel('ROI', color**=**'orange') ax1**.**set\_title('Production Budget vs ROI') ax1**.**set\_xticklabels(top\_10\_df\_budget['movie'], rotation**=**90, ha**=**'right') 

- *Show the plot* fig**.**tight\_layout() plt**.**show() 

  ![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.043.png)

  X-axis: The x-axis labels represent the movie titles in the top 10 (based on production budget). The labels are rotated 90 degrees for better readability.

  Y-axes: There are two y-axes in this chart:

- Left Y-axis (blue): This axis represents the Production Budget for each movie. The scale is likely in millions or billions of dollars, depending on your data. The blue bars show the production budget for each movie.
- Right Y-axis (yellow): This axis represents the ROI for each movie. Since the ROI values are normalized between 0 and 1, the scale doesn't have specific units but allows for easy comparison between movies on the same scale as the production budget. The yellow bars show the normalized ROI for each movie.

From the graph: The ROI values (yellow bars) vary across the movies. Some movies with high production budgets also have high normalized ROI (e.g., Avatar), indicating they generated a significant return on investment compared to their production costs. Other movies might have high production budgets but lower ROI (e.g., Dark Phoenix), suggesting they may not have recouped their costs as effectively.

Let's have a representation with bar graphs and a line graph for the ROI for better visualization

In [40]: **import**  matplotlib![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.044.png)

**import** matplotlib.pyplot **as** plt 

**import** seaborn **as** sns 

matplotlib**.**rc\_file\_defaults() 

top\_10\_df\_budget

ax1 **=** sns**.**set\_style(style**=None**, rc**=None** ) 

fig, ax1 **=** plt**.**subplots(figsize**=**(12,6)) 

sns**.**lineplot(data **=** top\_10\_df\_budget['ROI'], marker**=**'o', sort **=** **False**, ax**=**ax1) ax2 **=** ax1**.**twinx() 

sns**.**barplot(data **=** top\_10\_df\_budget, x**=**'movie', y**=**'production\_budget', alpha**=**0.5, ax**=**ax2 ax1**.**set\_xticklabels(top\_10\_df\_budget['movie'], rotation**=**90, ha**=**'right') 

plt**.**show() 

![](Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.045.jpeg)

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

[ref1]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.001.png
[ref2]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.002.png
[ref3]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.003.png
[ref4]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.004.png
[ref5]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.007.png
[ref6]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.009.png
[ref7]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.010.png
[ref8]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.011.png
[ref9]: Aspose.Words.c67f5186-3cfe-4b3b-a4ed-ee3b0f8c76d8.012.png
