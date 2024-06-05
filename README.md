# Top UK TIKtOK Influencer Analysis

## Overview

This project aims to analyse a dataset of top TikTok influencers in the UK to derieve meaningful insights for marketing strategies, 
influencers partnerships and understanding the dynamics of influenceer reach and engagement.

## Dataset

The dataset contains information about the TikTok influencers in the UK, including thier name, handle, number of followers, potential reach and topic of influence.

## Analysis Process

#### Step 1: Data Preparation



* 1. Loading the Data:
 The dataset was imported from a csv file into python using pandas for further analysis.

 ```python
 import pandas as pd
import os

df = pd.read_csv(tiktokdata)
tiktokdata 
```

* 2. Data Cleaning: Handled missing values and using delimeter to seprate columns, also seprated the account names from the account handle and converted the numberic columns that contain string values to Int using convert to int functions in python.
```python
df = pd.read_csv("~/Downloads/Tiktok_data_united-kingdom.csv", delimiter=';')
```
````python
df[['Account_Name', 'Account_Handle']] = df['NAME'].str.split('@', expand=True) 
````
`````python
mask = df ['FOLLOWERS'].isnull()
mask = df ['POTENTIAL REACH'].isnull()
`````
````python
def convert_to_int(val):
    val = val.strip().lower()  
    match = re.match(r'^(\d+\.?\d*)([mk])?$', val)  # Use regex to extract numeric value and multiplier
    if match:
        numeric_value, multiplier = match.groups()
        numeric_value = float(numeric_value)
        if multiplier == 'k':
            return int(numeric_value * 1000)  # Multiply by 1000 for 'K'
        elif multiplier == 'm':
            return int(numeric_value * 1000000)  # Multiply by 1000000 for 'M'
        else:
            return int(numeric_value)  # No multiplier, return as integer
    else:
        return None


df['POTENTIAL REACH'] = df['POTENTIAL REACH'].apply(convert_to_int)
````
`````python
def convert_to_int(val):
    val = val.strip().upper()  # Convert to uppercase and remove leading/trailing whitespace
    if 'M' in val:
        # Remove 'M' and multiply by 1,000,000 to convert to integer representing millions
        return int(float(val.replace('M', '')) * 1_000_000)
    else:
        return int(val)  # Convert directly if no 'M' suffix (this case won't actually be used here)

# applying the function to the followers column
df['FOLLOWERS'] = df['FOLLOWERS'].apply(convert_to_int)
`````




 #### Step 2: Exploratory Data Analysis (EDA)

* 1. Distribution Analysis : To visualise the distribution of followers. Also the distribution of potential reach and followers for each topic of influence.
````python
#imorting libaries
import matplotlib.pyplot as plt  # scatter plot to visualize the relationship between followers and potential reach
import seaborn as sns
import plotly.express as px # creating interactive plots to visualize the mean and median for followers and potential reach for each topic of influence

# Distribution Analysis of followers
plt.figure(figsize=(10, 6))
sns.histplot(df['FOLLOWERS'], bins=30, kde=True)
plt.title('Distribution of FOLLOWERS')
plt.xlabel('FOLLOWERS')
plt.ylabel('Frequency')
plt.show()

#Hist shows that there are around 70 infulencers who fall within a specific range of followers
`````

![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/output.png?version%3D1717605463272)

* 2. Relationship Analysis : Using scatter plot to understand the connections between followers and potential reach.
````python
# Relationship between followers and potential reach
plt.figure(figsize=(10, 6))
# Create a scatter plot
sns.scatterplot(x='FOLLOWERS', y='POTENTIAL REACH', data=df, s=100, color='green', alpha=0.6, edgecolor='w', linewidth=0.5)
# Add titles and labels
plt.title('Relationship Between Followers and Potential Reach')
plt.xlabel('Number of Followers')
plt.ylabel('Potential Reach')

# Show the plot
plt.show()
````



![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/output2.png?version%3D1717605568921)
 * 3. Metrics : I calculated summary statistics for numerical colums such as followers and potential reach to understand thier distribution. Using mean and median for both by Topic of Influence.
 ````python
 #creating interactive plots to visualize the mean and median for followers and potential reach for each topic of influence
# Group and aggregate the data
topic_analysis = df.groupby('TOPIC OF INFLUENCE').agg({'FOLLOWERS': ['mean', 'median'], 'POTENTIAL REACH': ['mean', 'median']}).reset_index()
topic_analysis.columns = ['TOPIC OF INFLUENCE', 'FOLLOWERS Mean', 'FOLLOWERS Median', 'POTENTIAL REACH Mean', 'POTENTIAL REACH Median']

# bar plot for FOLLOWERS Mean and Median
fig_followers_bar = px.bar(topic_analysis, x='TOPIC OF INFLUENCE', y=['FOLLOWERS Mean', 'FOLLOWERS Median'], barmode='group',
                           title='Mean and Median FOLLOWERS by TOPIC OF INFLUENCE')
fig_followers_bar.show()

#  bar plot for POTENTIAL REACH Mean and Median
fig_reach_bar = px.bar(topic_analysis, x='TOPIC OF INFLUENCE', y=['POTENTIAL REACH Mean', 'POTENTIAL REACH Median'], barmode='group',
                       title='Mean and Median POTENTIAL REACH by TOPIC OF INFLUENCE')
fig_reach_bar.show()


# result shows that the values specifically for the 'FOOD' topic, the average and central figures for both followers and potential reach is in this category.
# with a mean score of 20.1 and potential reach score of 6.0 while the 
# median score for followers and potential reach is 19.1 and 5.7 respectively
````

![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/git-repo/followers%20mean.png?version%3D1717604055438)![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/git-repo/newplot.png?version%3D1717604708303)




#### Step 3: In - depth Anlysis

* 1. Top Influencers : Identified the top influencers based on the number of followers and potential reach.
````python
Visualizing the top 30 influencers by followers using Plotly
# Find top 30 influencers by followers
top_influencers_followers = df.nlargest(30, 'FOLLOWERS')
# Create a bar chart using Plotly
fig = px.bar(top_influencers_followers, x='NAME', y='FOLLOWERS', title='Top 30 TikTok Influencers by Followers')
fig.show()
````
![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/git-repo/topfollowers.png?version%3D1717604914702)


`````python
# Top Influencers by Potential Reach
top_influencers_reach = df.nlargest(30, 'POTENTIAL REACH')
# Create a bar chart using Plotly
fig = px.bar(top_influencers_followers, x='NAME', y='POTENTIAL REACH', title='Top 30 TikTok Influencers by POTENTIAL REACH')
fig.show()
`````



![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/git-repo/potentail%20reach.png?version%3D1717605141033)





* 2. Influence by Topic : Averaged folllowers and potential reach were analyzed for each topic of influence to identify high engagements topics.

#### Step 4 :  Visualization

* 1. Followers vs Potential Reach : Scatter plots were used to visualize the relationship between followers and potential reach,
where each green marker on the plot represents an influencer and thier number of followers plotted on the x-axis and thier potential reac o the y-axis.

* 2. Top influencers Visualisation : Bar charts were used to display the top influencers based on followers and potential reach. 

* 3. Topic of Influence : Bar plot to calculate the average and central figure for both followers and potential reach for a particular Topic.

* 4. Distribution of followers and potential reach by Topic of influence : Histogram to show the most popular in terms of influential people and thier audience size
````python

````# creating a histogram to visualize the distribution of follwers and distribution of potential reach by topic of influence

fig_followers_bar = px.histogram(df, x=df['FOLLOWERS'], color=df['TOPIC OF INFLUENCE'], nbins=30, barmode='overlay', title='Distribution of FOLLOWERS by TOPIC OF INFLUENCE')
fig_followers_bar.show()

fig_reach = px.histogram(df, x=df['POTENTIAL REACH'], color=df['TOPIC OF INFLUENCE'], nbins=30, barmode='overlay', title='Distribution of POTENTIAL REACH by TOPIC OF INFLUENCE')
fig_reach.show()



# The hist shows the Entertainment and Music topic is the most popular in terms of both the number of influential people and their audience size.
# It leads in both the number of followers and the potential reach, making it the standout category.
````


![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/git-repo/df.png?version%3D1717605232362)



![alt text](https://file%2B.vscode-resource.vscode-cdn.net/Users/vee/Desktop/Projects/Ex_Files_ML_with_Python_Foundations/git-repo/dp.png?version%3D1717605289784)

#### Step 5 : Insights and Recommendations

* 1. Key isnights :  The analysis shows insights into high engagement topic and significant patterns in data.
* 2. Recommendations: Based on the findings, actionable recommendations were provided for marketing strategies and influencer partnerships.
