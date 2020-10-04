## background

Ever noticed that top comments on reddit threads seem to have the same time stamp as the post? Well, I did, or at least thought I did, so I wanted to know for sure.

Essentially, the question is this: just how time-lopsided are top comments? This has some various tangents:
- if you make a comment super early, is it likely to be more upvoted?
- if you make a commenet late-ish, how /unlikely/ is it to be upvoted?
- are votes based on content, or just being seen?
- how bad is the "scroll burden"; reddit defaults to sorting by best, which means that to see new comments (added later), users have to scroll... will they?
- does this vary by sub?

## method

I used [`praw`](https://praw.readthedocs.io/en/latest/) to scrape the top 50 subreddits for their top 150 posts of all time, obtaining:
- the top 10 upvoted comments
- the first ~500 comments

## results

Yes, early comments are /way/ more likely to be upvoted. In fact, 25% of the top 10 comments are made within the first 25min, 50% within the first hour, and 75% within the first 2hrs.

To calcualte "score dominance," I used the following: `comment_score/sum(comments)`. Thinking of a comment score like it's weight, if I threw the first 500 comments and the top comments into a bag, 0.5 means that comment was responsible for 50% of the weight of the whole bag.

Crazily, to me, of the oldest 500 comments /outside/ of the top 10, the highest score dominance was 0.11. The "scroll burden" and snowball effect of early comments getting points and staying at the top is immense.

Hence, "with great punctuality comes great responsiblity." If you write one of the first comments... take a moment to really think about what you want say!