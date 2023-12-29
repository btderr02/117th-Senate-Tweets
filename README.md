# 117th-Senate-Tweets
This repository contains Tweets gathered from the Twitter accounts of members of the 117th Senate. They were collected on 8 April 2023. It utilizes an Excel file with a list of each Senator's account for this term period. Tweets were gathered from January 3, 2021 to January 3, 2023. Raphael Warnock and Alex Padilla came into office on January 20, 2021. Their Tweets from January 3, 2021 to January 19, 2023 are still included in the dataset. Kamala Harris and Kelly Loeffler were members of the Senate from January 3, 2021 to January 20, 2021. None of their Tweets are included in the dataset. The following Python code was used to gather the data.

```python
import snscrape.modules.twitter as sntwitter
import pandas as pd
import openpyxl

# Load Twitter account names from Excel file into a DataFrame
df_accounts = pd.read_excel('twitter_accounts.xlsx')

# Create a list to store scraped data for each tweet
tweets_list = []

# Loop through each row in the DataFrame and scrape data from each Twitter account
for index, row in df_accounts.iterrows():
    # Extract Twitter account URL from current row
    url = row[0]  # Replace 0 with the index of the column containing the account URLs

    # Use snscrape to scrape tweets from the account URL
    for tweet in sntwitter.TwitterSearchScraper("from:" + url + " since:2021-01-03 until:2023-01-03").get_items():
        # Store tweet data in a list
        tweets_list.append([tweet.date, tweet.rawContent])

# Convert list of tweets into a Pandas DataFrame
df_tweets = pd.DataFrame(tweets_list, columns=["date", "content"])

# Save DataFrame as separate CSV files for each account
for index, row in df_accounts.iterrows():
    url = row[0]
    df_tweets[df_tweets['content'].str.contains(url)].to_csv(f'{url}_tweets.csv', index=False, encoding='utf-8-sig')

print("Twitter data scraped and saved successfully.")
```
