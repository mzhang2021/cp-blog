---
layout: post
title: "Self Reflection and What's Next?"
tags: [personal, opinion]
featured: true
---

It's been a month since my last major milestone in CP, and high time for some self reflection. There are two major CP competitions I competed in this month: Facebook Hacker Cup, and Kotlin Heroes 8.

## FHC

![image 1]({{ site.baseurl }}/assets/images/reflection-1.png)

In last weekend's round 3, I got a frustrating 222nd place, narrowly missing the top 200 badge on the t-shirt. To think that if I just did A and D1 faster, I could have gotten it! Or if only they counted top 200 for round 2 as well T_T.

Saltiness aside, my downfall in this contest was unfortunately implementation. In both my implementations for A and D1, I had a critical bug that required stress testing against a brute force solution to find. Those types of bugs are the worst, because it's so time consuming to write another solution AND a generator, and that's just the time taken to find the test case. When you add on the time it takes to analyze the test case, print debug output, and think about what's wrong, it's easy to lose a significant amount of time debugging. Thus, I submitted A at the 40 min mark and D1 at the 1 hr 50 min mark, both very undesirably late and not leaving much time to work on the other problems. A similar situation happened with C2 in round 2, so implementation woes is definitely a common trend with my FHC experience.

Practicing implementation is a strange thing. For me, writing clean implementations in a contest environment vs practice are two very different things. In practice, I'm usually able to keep my implementation relatively clean. On the other hand, in contest I'm often worried about other variables like time left in the contest, and I approach code writing in more of an additive manner, where I'll keep adding in more code on top of the old code to fix issues instead of refactoring, resulting in nonsense like this:

<div markdown="1" style="text-align: center; margin-bottom: 5%">
![image 2]({{ site.baseurl }}/assets/images/reflection-2.png)

<em>TFW you run out of ideas for variable names so you go with "yyyy"</em>
</div>
Perhaps the strat is to just approach implementation the same way I do in practice. Don't worry too much about the time, because writing sloppy code will probably consume more time overall anyways.

## Kotlin Heroes

![image 3]({{ site.baseurl }}/assets/images/reflection-3.png)
Kotlin heroes went slightly better for me, cause at least I got the shirt. Aside from the slow start from figuring out the kinks of Kotlin, I was on a pretty good pace for A-F, snagging me within top 50 off of speedforces. That being said, G was an annoying problem to not get because I felt that type of problem (DP with data structures) is within my ballpark. I got the naive DP recurrence: `dp[i][j] = max(dp[i-1][j] + a[i][j], dp[i-1][j+1])`, where `dp[i][j]` is the maximum damage after the first `i` warriors made moves and there are `j` barricades left, and `a[i][j]` is equal to the amount of damage warrior `i` can do if there are `j` barricades left (usually equal to 0). But the way I went about optimizing it was wrong: I tried to think of each row of the DP table as an array of segments of equal values. Then, the transition can be modeled as having each segment extend one unit to the left if it's larger than the left segment and updating a single point (which potentially creates a new segment). So kind of like a slope trick-esque approach. See the diagram below:

![image 4]({{ site.baseurl }}/assets/images/reflection-4.png)

Unfortunately, I couldn't find any good properties of this function, which made it difficult to speed it up past quadratic complexity. A better intuition is to map `dp[i][j]` as 2D coordinate points, and mark the transitions as querying a triangle:

![image 5]({{ site.baseurl }}/assets/images/reflection-5.png)

Essentially, most DP transitions don't change the DP value. Only a linear number of states actually change value by adding `a[i][j]`, so we can consider the transitions more directly between value changing points. `dp[i][j]` can transition from any previous state `dp[k][l]` if `k + l < i + j`. So a state can transition from any previous state within a triangle. Once you apply that intuition, the way to speed up the DP recurrence with data structures is far more apparent.

Missing the necessary intuition is probably the most common reason I fail to solve a problem, and unfortunately it's also the most nebulous to try to improve on. It's hard to practice to just "become smarter." In my experience, "becoming smarter" usually happens from the result of just doing a lot of relatively difficult problems (so above your rating range). Once you've done enough, you start to notice patterns in trains of thought and get better intuition for which ideas are good and which aren't (I talked extensively about this in a [post from last month]({{ site.baseurl }}/difficulty)). So perhaps I'd have to start grinding hard problems again if I want to get over this plateau.

## What's Next?
I'm not entirely sure. To be honest, I don't practice or spend anywhere close to as much time on CP as I used to, mainly because I've now found interest in other hobbies and things. Reaching red was kind of a "final boss" for me in CP, and now that I've hit red, I don't know how much effort I'd want to commit to the next milestone. But at the very least, I can assure you this blog will still be updated. Writing this blog hasn't gotten boring yet :)

How about you? Where are you in your CP journey, what's your next goal, and what specific aspects of CP do you need to work on? Feel free to comment them below.