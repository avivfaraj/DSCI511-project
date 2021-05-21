# DSCI 511 Final Project

by Aviv Farag, Jeff Braga, Gloria Sheppard

This project presents a dataset synthesizing Tech CEO's social media posts with their company's short term stock price data. We hope this dataset can fuel further discussion of social media posting as intentional market strategy and the ability to analyze such actions.

## Dataset

`data.json` is the final dataset created by the code. Each other json file is specific to a specific CEO, and represents one piece of the final dataset. 

## Instructions

To run the code, first open up the `tc_tweets.ipynb`. You will need authentication tokens for both Twython and IEX cloud for the initialize portion of the jupyter notebook.

1. Run code for importing packages
2. Run Functions # 1 - 10
3. Invoke `add_old_tweets` and `add_new_tweets` to fetch tweets for a specific CEO.
4. Invoke `sub_dataset` function in order to modify the tweet's dictionary and to download the required stock files from the IEX API. 
5. Invoke `add_to_dataset` function in order to create a dataset out of the tweet's dictionary and stock files and append it to the dataset file.

## Notes, Limitations, and Problems Encountered
Initially, we had intended to gather posts from both Facebook and Twitter, but after experimenting with the API's for both platforms and at the advice of the professor we are focusing on just Twitter as a platform. Facebook's API was not as user friendly for this purpose, and also nowhere near as straightforward to gather the sort of data we wished to collect for this project.

One problem that arose in the project design is the timezone differences. The New York Stock Exchange is run on Eastern Time, and it appeared that this was indeed the timezone given to us in the data returned from the IEX API. However, the Twitter API requests we make through the python wrapper Twython, return a timezone of GMT in their data. To make this even more confusing, if one is looking at the Twitter website itself, you will likely see a post in your own timezone, not GMT.

It would be problematic for collecting the data field for the stock price at the time of a post if we cannot accurately line up the data from different API's in terms of timezone. However, in ensuring that Twython was returning GMT, we updated our code to ensure we convert GMT to EST, so that the stock price we are looking for is in terms of EST. One further complication we note is that the timezone changes between Eastern United States and the United Kingdom (GMT) are not always synchronized. Therefore, we had to update our code once more to ensure that it accounts for the days when the timezone difference is shorter than it typically is for the rest of the year. This change only accounts for a certain range of years, but goes back at least the past 5 years correctly.

Another unexpected result comes from the Twython request options. If one specifies not to get retweets, then retweets are not in the data returned by the request. However, when one makes a request for a certain number of the most recent tweets, the retweets are still counted in this number, even if they are not returned. So a request for 100 most recent tweets of a profile that retweeted 80 of their last 100 retweets will in actuality only return the data for 20 tweets.

We also note that it is important to use the extended mode for the Twitter API request so as not to get a truncated tweet. Not seeing the full text could affect the accuracy in our "Does Post Mention Company Name" field.

Finally, one last limitation is the hours of the stock market. If someone posts after the market is closed, or not during market hours, we were initially unsure how to report this data. In fact, some of the "1 Day Later", "2 Days Later" and "7 Days Later" fields may fall on a weekend or day the market was closed. We report such data with "-".

