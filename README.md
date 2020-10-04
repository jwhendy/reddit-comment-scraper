## background

Ever noticed that top comments on reddit threads seem to have the same time stamp as the post? Well, I did, or at least thought I did, so I wanted to know for sure.

Essentially, the question is this: just how time-lopsided are top comments? This has some various tangents:
- if you make a comment super early, is it more likely to be upvoted?
- if you make a comment later, how *unlikely* is it to be upvoted?
- are votes based on content, or just being toward the top (and seen)?
- how bad is the "scroll burden"? Namely, reddit defaults to sorting by best, which means that to see new comments (added later), users have to scroll... will they?
- how much does this vary by sub?

## method

I used [`praw`](https://praw.readthedocs.io/en/latest/) to scrape the top 50 subreddits for their top 150 posts of all time, obtaining:
- the top 10 upvoted comments
- the first ~500 comments

## results

Yes, early comments are /way/ more likely to be upvoted. In fact, 25% of the top 10 comments are made within the first 25min, 50% within the first hour, and 75% within the first 2hrs.

To calcualte "score dominance," I used the following: `comment_score/sum(comments)`. Thinking of a comment score like it's weight, if I threw the first 500 comments and the top comments into a bag, 0.5 means that comment was responsible for 50% of the weight of the whole bag.

Crazily, to me, of the oldest 500 comments *outside* of the top 10, the highest score dominance was 0.11 (which is a total outlier; the *3rd quartile* is only 5e-4). The "scroll burden" and snowball effect of early comments getting points and staying at the top is immense.

Hence, "with great punctuality comes great responsiblity." If you write one of the first comments... take a moment to really think about what you want say!

![infographic](https://github.com/jwhendy/reddit-comment-scraper/blob/master/infographic.png)

This does, indeed, tend to vary by sub. Here's the density of time deltas (time since the first comment), colored by top 10 vs. oldest 500. They're sorted by the mean of the top comment time deltas. My interpretation:
- if there's a massive spike all the way toward the left, these subs have higher prominence of simply snowballing votes on early comments
- if the distribution is wider/flatter/longer tail, these subs are more willing to scroll down, read and evaluate comments, and upvote based on merit
- it's interesting that top/oldest *generally* correlate, but that sometimes the top 10 are still quite early while the oldest might have a much fatter tail. This might speak to the "shelf life" of a post per sub. Some appear consumed and disposed of very fast, or life is faster in that sub... others might be frequently visited and commented on quite a bit later.

![infographic](https://github.com/jwhendy/reddit-comment-scraper/blob/master/oldest-vs-top_by-sub.png)
