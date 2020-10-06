## background

Have you ever seen a top comment on a reddit post and noticed the time stamp matches the post (i.e. both happened "7hrs ago")? Well, I did, or at least thought I did, so I wanted to know for sure.

Essentially, the question is this: just how time-lopsided are top comments? This question has various interpretations/corollaries:
- if you make a comment super early, is it more likely to be upvoted?
- if you make a comment later, how *unlikely* is it to be upvoted?
- are votes based on content, or just being toward the top (and seen)?
- because the reddit default sort is "best," users have to scroll to read newer comments; will they, or is there a high "scroll burden"?
- do these metrics/phenomenon vary by sub?

## method

I used [`praw`](https://praw.readthedocs.io/en/latest/) to scrape 57 subreddits for their top 150 posts of all time, obtaining:
- the top 10 upvoted comments
- the oldest ~500 comments

The top 50 subs were pulled from https://frontpagemetrics.com/top, and I added 7 of my own.
```
subs = ['adviceanimals', 'announcements', 'art', 'askreddit', 'askscience', 'aww', 'blog', 'books', 'creepy',   
        'dataisbeautiful', 'debatereligion', 'diy', 'documentaries', 'earthporn', 'explainlikeimfive', 'food', 
        'friendsafari', 'funny', 'futurology', 'gadgets', 'gaming', 'gifs', 'history', 'hockey', 'iama',
        'internetisbeautiful', 'jokes', 'lifeprotips', 'listentothis', 'memes', 'mildlyinteresting', 'movies', 
        'music', 'nba', 'news', 'nosleep', 'nottheonion', 'oldschoolcool', 'personalfinance', 'philosophy', 
        'photoshopbattles', 'pics', 'science', 'showerthoughts', 'soccer', 'space', 'sports', 'teenagers', 
        'television', 'tifu', 'todayilearned', 'twoxchromosomes', 'upliftingnews', 'videos', 'worldnews',
        'writingprompts', 'wtf']
```

To calculate "normalized score dominance," I used the following approach per post:

```
# comment weight / expected weight if all comments had the same score
(comment_score/sum(comment_scores)) / (1/number_of_comments)

# reduces to
comment_score/mean(comment_scores)
```

This calcualtes the per-comment weight, normalizing to 1 (which better allows comparing each post's results, since the total comments ranges from ~250-500 per post).

For an example, consider a thread with 500 total comments
- if all comments had 10 upvotes, each comment would have a normalized dominance of `(10/(10*500)) / (1/500) = 1`
- if one comment had a score of 1000 with all others 10, this yields `(1000/(1000+(10*499))) / (1/500) = 83.5`. This score has 83x the score dominance vs. if all comments were equally upvoted.

I'm no data scientist/statistician, but this metric does an okay job at getting at "how much of an upvote outlier is this comment?"

Returning to our key inquiry: the key inquiry is about score dominance vs. *time*. If a) comments have a random probability of being worthy of votes, and b) redditors content by value/worth, and c) comments are equally likely to be read/reviewed/voted, we should a low correlation between score dominance and time.


## results

This is not at all the case: early comments are *drastically* more likely to be upvoted. In fact, the quantiles for top 10 comments are:

```
|     | time, hours | order, nth | order/total comments, % |
|-----+-------------+------------+-------------------------|
| 25% |          25 |         10 |                    0.5% |
| 50% |          67 |         33 |                    1.8% |
| 75% |         130 |         99 |                    5.4% |
```

Of the oldest 500 comments *outside* of the top 10, the highest score dominance was only 50 (which is a complete outlier; the mean is only 0.3). The "scroll burden" and snowball effect of early comments getting points and staying at the top is immense.

![infographic](https://github.com/jwhendy/reddit-comment-scraper/blob/master/infographic.png)

## interpretation

Hence, "with great punctuality comes great responsiblity." If you write one of the first comments... take a moment to really think about what you want say, as you have a higher probability of becoming a top comment, and subsequently stealing the viewership (and contribution to the discussion) of later comments.

This assumes, again, that upvotes are simply a function of being seen, not that already upvoted comments are purely upvoted due to being upvoted. This *could* be the case, but I think a more likely explanation is that genuinely good/funny/insightful *early* comments are quickly made evident through genuine votes, and thus they continue to receive more screen time via the default "best" sorting, and viewers rarely scroll very far down. Thus, already good comments are snowball voted.

This does, indeed, tend to vary by sub. Here's the density of time since first comment, colored by top 10 vs. oldest 500. They're sorted by the mean of the top comment times. My interpretation:
- if there's a massive spike all the way toward the left, these subs have higher prominence of snowballing votes on early comments
- if the distribution is wider/flatter/longer tail, these subs are more willing to scroll down, read and evaluate comments, and upvote based on merit
- it's interesting that top/oldest *generally* correlate, but that sometimes the top 10 are still quite early while the oldest might have a much fatter tail. This might speak to the "shelf life" of a post per sub. Some appear consumed and disposed of very fast, or life is faster in that sub... others might be frequently visited and commented on quite a bit later.
- admittedly, a caveat on all of the above is we're limited to ~1 day of activity. It took almost a day to scrape all of this as-is, and I was particularly looking for posts with high activity for this analysis; the per-sub observation was just a side endeavor.

![infographic](https://github.com/jwhendy/reddit-comment-scraper/blob/master/oldest-vs-top_by-sub.png)