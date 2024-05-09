# IJAN-MovieProject2
Movie Project2
data = pd.read_csv(r'C:\Users\HP USER\Desktop\IMDB-Movie-Data.csv')

data.shape
(1000, 12)

#display top 10 rows of the dataset
data.head(10)

#check for missing values
data.isnull().sum()

Rank                    0
Title                   0
Genre                   0
Description             0
Director                0
Actors                  0
Year                    0
Runtime (Minutes)       0
Rating                  0
Votes                   0
Revenue (Millions)    128
Metascore              64
dtype: int64

#check for missing values
data.isnull().sum()

Rank                    0
Title                   0
Genre                   0
Description             0
Director                0
Actors                  0
Year                    0
Runtime (Minutes)       0
Rating                  0
Votes                   0
Revenue (Millions)    128
Metascore              64
dtype: int64

#percentage of missing values
per_missing = data.isnull().sum() * 100/len(data)
per_missing

#dropped missing rows
data.dropna(axis = 0)

#check for duplicates
dup_data = data.duplicated().any()
Print("Are there any duplicate values?", dup_data)
Are there any duplicate values? False

#get overall statistics about data:mean,min, max etc for numerical columns
data.describe()

#get overall statistics about data for all columns
data.describe(include = 'all')



QUESTIONS FORMULATED:

#Title of movie having runtime >=180mins
data[data['Runtime (Minutes)']>= 180]['Title']

#show variation btw year had the highest voting
data.groupby('Year')['Votes'].mean().sort_values(ascending = False)

#show variation btw year had the highest average voting
avg_votes_per_yr = data.groupby('Year')['Votes'].mean()
plt.figure(figsize = (10,6))  
plt.bar(avg_votes_per_yr.index, avg_votes_per_yr.values, color = 'red')
plt.xlabel('Year')
plt.ylabel('Average Votes')
plt.title('Average Votes by Year')
plt.grid(True) 
plt.show()

#which year had the highest average revenue 
data.groupby('Year')['Revenue (Millions)'].mean().sort_values(ascending = False)

#Graphical display of variation between year and highest average revenue
avg_revenue_per_yr = data.groupby('Year')['Revenue (Millions)'].mean()
plt.figure(figsize = (10,6))  
plt.barh(avg_revenue_per_yr.index, avg_revenue_per_yr.values, color = 'darkblue')
plt.xlabel('Average Revenue (Millions)')
plt.ylabel('Year')
plt.title('Average Revenue (Millions) by Year')
plt.grid(True) 
plt.show()

#average rating for top 10 directors
mean_rating_per_director = data.groupby('Director')['Rating'].mean()
top_10_directors = mean_rating_per_director.sort_values(ascending=False).head(10)
print(top_10_directors)

plt.figure(figsize = (10,6))  
top_10_directors.plot(kind = 'bar')
plt.xlabel('Director')
plt.ylabel('Average Rating')
plt.title('Top 10 Directors by Average Rating')
plt.xticks(rotation=45, ha = 'right')
plt.grid(True) 
plt.show()

#top 10 lengthy movies and runtime
Top10_len=data.nlargest(10,'Runtime (Minutes)' )[['Title', 'Runtime (Minutes)']].set_index('Title')
Top10_len

sns.barplot(x='Runtime (Minutes)', y=Top10_len.index, data=Top10_len)
plt.title('Top 10 Movies by Runtime(Minutes)')
plt.show()

#how many movies were watched in each year
data['Year'].value_counts()

sns.countplot(x='Year', data=data, color = 'darkblue')
plt.ylabel('Number of Movies Released')
plt.title('Number of Movies Per Year')
plt.show()

#most popular movie title
data[data['Revenue (Millions)'].max()==data['Revenue (Millions)']]['Title']

#top 10 highest rated movie titles and its directors
Top10_len=data.nlargest(10,'Rating' )[['Title', 'Rating', 'Director']].set_index('Title')
Top10_len

sns.barplot(x=Top10_len.index, y='Rating', data=Top10_len, hue='Director', dodge=False)
plt.legend(bbox_to_anchor=(1.05,1), loc=2)
plt.xlabel('Title')
plt.ylabel('Rating')
plt.title('Top Movies by Rating')
plt.xticks(rotation = 45)
plt.grid(True)
plt.show()

#top 10 highest revenue movie titles
Top_10=data.nlargest(10, 'Revenue (Millions)')[['Title','Revenue (Millions)']].set_index('Title)
Top_10

sns.lineplot(x='Revenue (Millions)',y=Top_10.index, data=Top_10, marker ='o', color = 'darkblue')
plt.grid(True)
plt.title('Top 10 Highest Revenue Movie Titles')
plt.show()

#Average rating of movies Year wise
data.groupby('Year')['Rating'].mean().sort_values(ascending = False)

#Does rating affect revenue
sns.scatterplot(x='Rating',y='Revenue (Millions)', data=data)
plt.show()
#Yes because as Rating is increasing, Revenue is also increasing



#classify movies based on excellent, good and average 
#using user defined function
def rating(rating):
    if rating>=7.0:
        return "Excellent"
    elif rating>=6.0:
        return "Good"
    else:
        return "Average"
data['rating_cat']=data['Rating'].apply(rating)
data.head()


data['Genre'].dtype
dtype('O')
#count number of action movies
len(data[data['Genre'].str.contains('Action',case=False)])
303
