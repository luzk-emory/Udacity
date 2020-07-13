
# Project: Wrangle and Analyze Data

## Wrangle Report

In the wrangling stage, I fixed the following issues in the datasets:

1. Data quality issues:
   1. Variable `timestamp` has unsupported time format.
   2. Variable `name` contains a lot of invalid values.
   3. Variable `source` contains redundant information.
   4. Retweeted and in-reply-to posts need removing.
   5. Tweets without image url need deleting.
   6. Tweets not about dogs need deleting.
   7. Dog breed predictions need to be cleaned.
   8. Other redundant variables and incorrect data format require cleaning.

2. Data tidiness issues:

   1. There three separate tables which can be combined into one.

   2. There are four variables representing the stage of a dog.

Details are as follows.

#### Data issue #1

Variable `timestamp` in `twitter-archive-enhanced.csv` has a string format. To better utilize Python's support for time, I transformed it into Python's datetime format.

#### Data issue #2 

Variable `name` contains a lot of invalid values. For example, the following values were present in the original data. 


    a               55
    the              8
    an               7
    very             5
    just             4
    quite            4
    one              4


These names do not look like real names. I replaced all of them with NaN.

#### Data issue #3

Variable `source` contains unnecessary information: 


```php
<a href="http://twitter.com/download/iphone" rel="nofollow">Twitter for iPhone</a>     2221
<a href="http://vine.co" rel="nofollow">Vine - Make a Scene</a>                          91
<a href="http://twitter.com" rel="nofollow">Twitter Web Client</a>                       33
<a href="https://about.twitter.com/products/tweetdeck" rel="nofollow">TweetDeck</a>      11
```

Basically, it is in php format with lots of redundant components. I use regular expression matching to extract the essential part:


    Twitter for iPhone     2221
    Vine - Make a Scene      91
    Twitter Web Client       33
    TweetDeck                11

#### Data issue #4

The dataset contains a lot of the retweeted posts. For example, the following tweets all start with `RT @dog_rates`. Since we only the need original post, we need discard all these tweets.


    19    RT @dog_rates: This is Canela. She attempted s...
    32    RT @Athletics: 12/10 #BATP https://t.co/WxwJmv...
    36    RT @dog_rates: This is Lilly. She just paralle...
    68    RT @dog_rates: This is Emmy. She was adopted t...
    73    RT @dog_rates: Meet Shadow. In an attempt to r...

Similarly, we need discard those "in reply to" tweets. This task is easy as we already have the indicator variables `retweeted_status_id` and `in_reply_to_status_id`. We can simply remove those with a `True` value.


#### Tidiness issue #1 Merge data

Since our main table has been cleaned, we are ready to merge all three tables. 


```python
df1 = df_twitter_clean.merge(df_predictions, on='tweet_id', how='left').merge(df_api, on='tweet_id', how='left')
```


#### Data issue #5

After matching, we can find some tweets do not have a pic url, so we are gonna remove them.


#### Data issue #6

We need drop tweets with pictures that are not dogs as this is an activity of rating dogs. The results from the image classifier provides three predictions. I impose a relatively loose standard: tweets with at least one valid prediction are kept. 

#### Data issue #7

There are three predictions of dog's breed. For the purpose of analysis, we only need one. So I have to determine the (predicted) breed of the dog. Since there are three predictions provided, I pick the dog breed label with highest probability as the (predicted) breed. This means:

- If all three labels are dogs, simply pick the one with highest probability
- If the label with highest probability is not dog, skip it and look for the one with 2nd highest probability.

Further, I cleaned these labels, replacing `-` and `_` with spaces.

#### Tidiness issue #2

There are four variables representing the stage of a dog. This violates the tidiness standard of data. These four variables should be consolidated into a single variable. I first use a four-digit string to represents the stage:


    0000    1409
    0010     166
    1000      54
    0001      21
    0100       7
    1010       7
    1100       1
    1001       1
    Name: stage, dtype: int64

Here, the i-th digit corresponds to the i-th elements of the list `['doggo','floofer','pupper','puppo']`. 

For example, 
- 0010 represents `pupper`.
- 1100 represents both `doggo` and `floofer`, which may not be a valid value. But we keep it here.

However, this variable may not be very useful. I then create `stage2` which treats those with multiple stage labels as NaN:


    pupper     166
    doggo       54
    puppo       21
    floofer      7
    Name: stage2, dtype: int64

#### Data issue #8

Finally, I drop all redundent variables and correct the data types.

```python
df_final = df1_clean.drop(columns=['in_reply_to_status_id','in_reply_to_user_id',
                               'retweeted_status_id','retweeted_status_user_id','retweeted_status_timestamp'])
df_final.drop(columns=stage_list, inplace=True)
df_final.drop(columns=df_final.columns[10:19], inplace=True)
df_final['img_num']  = df_final.img_num.apply(int)
```

The data looks good now.
