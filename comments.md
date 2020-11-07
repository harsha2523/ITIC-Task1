## We will Download the Mooc dataset file using the link provided in Trello.
## Now we will import the libraries required for generating graphs.


```python
#importig required libraries
import seaborn as sns
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from pandas import DataFrame

```

## Reading the datasets using pandas.


```python
#reading the data using pandas
course_data=pd.read_csv("course_information.csv")
course_threads=pd.read_csv("course_threads.csv")
course_posts=pd.read_csv("course_posts.csv")
sort = course_data.sort_values(by = 'num_threads' , ascending = False)

```

## Generating bar plot between course identifiers and number of threads usinig seaborn.


```python
plt.figure(figsize=(10,8))
sns.barplot(data = sort, x ='course_id', y = 'num_threads', color='blue');
plt.xticks(rotation = 90, horizontalalignment = 'center');

plt.xlabel("Course Identity")
plt.ylabel("Number of threads")
plt.tight_layout();
plt.title("Number of threads Vs. Course Identifiers")
```

## FIGURE 2: Total Number of Messages(log scale) by Coursera user type
## We are using group by function to get the count of the post_id and number of messages and then used it to plot a barplot between user type and number of messages(log scaled).
## We are using  seaborn to generate a barplot between coursera user type and log scaled number of messages.



```python
#using gorupby to create another dataframe with user_type and post id
course_posts_1 = course_posts.groupby('user_type')['post_id'].count().reset_index()
course_posts_1.sort_values('post_id', ascending = False, inplace= True)

sns.barplot(data = course_posts_1, x = 'user_type', y = 'post_id', color = 'blue')
plt.xlabel("Coursera User Type")
plt.ylabel("Number of Messages")

#adding log scale to numnber of messages
plt.yscale('log');
plt.xticks(rotation = 90, horizontalalignment = "center");
plt.title("Total Number of Messages(log scale) by Coursera user type")
#plt.show()
```


```python
#course_posts.head(5)
```

## Figure 3:
## creating a coloum which will tell us parent_id is 0 or not.





```python
course_posts['is_post'] = course_posts.parent_id==0 #checking for posts in parent_id

```

## We will first generate a new dataframe "tmp" where will will get the count of post_id using .groupby count() function
## We are creating new variable count which will have the values of post_id.
## Now we drop post_id from our dataset to avoid unnecessary iterations.


```python
#Grouping the variables using groupby.count
group = course_posts.groupby(['course_id', 'thread_id', 'is_post']).count()[['post_id']].reset_index() 
group
group['count'] = group['post_id'] # replacing the column post_id with count
group.drop('post_id', axis=1, inplace=True)
group.head()

#generating a new column with the name num_posts and assigning it value as 0.
#checking if is_post is true or false.
group['num_posts'] = 0
group['num_comments'] = 0
group.loc[tmp.is_post == True, 'num_posts'] = tmp['count']
group.loc[tmp.is_post==False, 'num_comments'] = tmp['count']
group.head()
```

## We will now use groupby.max() and reset the index


```python
group = group.groupby(['course_id', 'thread_id']).max().reset_index()
group.head()
```

##  we have the number of posts and comments so, we we need our data from forum_id with category 3(Assignments) and 4(Study Groups/ Meetups) to generate the scatter plot


```python
dat = course_threads.query("forum_id == 4|forum_id==3").merge(tmp, on=['course_id', 'thread_id'])
dat.head(2)
dat = fig4_dat[['forum_id', 'num_posts', 'num_comments']]
dat.head(2)

```

## we will now generate scatter plot using Seaborn


```python
sns.scatterplot(data=fig4_dat, x='num_posts', y='num_comments', hue='forum_id')

#setting limits for x and y axis

plt.xlim(0,100)
plt.ylim(0,100)

#setting labels and the title
plt.xlabel("Number of Posts")
plt.ylabel("Number of Comments")
plt.title("Scatter plot showing relationship between Number of comments and number of posts for Assignments and Study Group");

```


```python

```
