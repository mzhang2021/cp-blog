---
layout: post
title: "Long Challenges are Awesome"
tags: [opinion]
featured: true
usemathjax: true
---

Short contests are fun, although not without its fair share of frustrating moments. Have you ever come so close to solving a problem, but ran out of time? If only you had 10 extra minutes! Or have you ever "bricked" on a problem? That is, you come across a problem that should be within your rating range and easily solved, but you just can't think of the trick needed to solve it? This has happened to me many times, with the most recent example being [Global Round 15](https://codeforces.com/contest/1552/standings/participant/117714327#p117714327). I could not think of how to get past the naive $\mathcal O(n^2)$ solution for problem B for the longest time, and I had to skip that problem to do the other ones first in order to save my performance.

Now don't get me wrong. Codeforces contests are great, and the adrenaline rush they provide, from the last minute clutch solves to the anticipation of system tests, is unmatched. But sometimes, I just want to take it chill and casually solve problems. That's where the long challenge comes in.

The long challenge format is where contestants are given a long period of time, usually a week or several days, to work on a set of problems. Some notable examples include CodeChef Long Challenges, HackerEarth Circuits, and the Qualification Round of Google Code Jam. In this article, I'll talk about why I have a great appreciation for this contest format, and why this can be the most gratifying format to compete in.

## First, the elephant in the room.

Unfortunately, we can't talk about long challenges without acknowledging cheating. People cheat on programming contests. Programming contests are usually online and participated in from the comfort of your home, so no one can monitor your behavior. Couple that with the strong emphasis that certain software engineer employers place on competitive programming for hiring, and there's a tremendous incentive to cheat. The long challenge format is by far the easiest to cheat on, because solve times don't matter and there's a longer time frame to obtain answers from other people.

If I'm being honest, there's really not much one can do about cheating in programming contests. The fundamental nature of it being virtual means you can't spy on other contestants to prevent this type of behavior. There are countermeasures like MOSS (which doesn't really work and can't stop sharing of just ideas) and reporting by contestants, but the amount of cheating that happens each contest is likely intractable for contest admins to effectively shut down. What I can say is that cheating becomes far less prevalent as you move up in rating, so you could cheating as motivation to get out of lower divisions as quickly as possible. Also, since I usually don't do long contests for any form of rating (CodeChef long challenges are unrated for Div 1, and I don't really care about HackerEarth rating), I focus more on just trying my best to solve all the problems in the problemset. Cheating sucks, but it doesn't significantly impact my ability to enjoy long challenges.

Now with that out of the way, allow me to list some of the aspects I enjoy about this format.

## 1. Schedule Convenience
Where I live, the most popular contest platforms like Codeforces, Atcoder, and Codechef always host their short contests in the morning and end at noon. Especially now that school classes are returning to in-person, it's difficult to compete in these contests during the weekdays without them cutting into a significant part of my day, and it's annoying to wake up early on weekends to do them. Long challenges grant more flexibility in the timing, so I can work on them in the afternoon or nighttime instead.

## 2. Focus on Problem Solving
With long challenges, I feel like it's just me and the problems. I don't necessarily have to be sitting at my desk to think about problems. I can read them first, then keep them in the back of my head as I'm going through the rest of my day. If I'm tired or am getting repeatedly stuck on the same ideas, I can wait until tomorrow to try and get a fresh perspective. I remember in my first ever CodeChef long challenge, I was stuck on the problem [Blackjack](https://www.codechef.com/JAN21C/problems/BLKJK) after thinking about it for a few hours. For the next few days, I just kept it in the back of my head, working on the problem every once in a while and resetting my ideas constantly. Then one night, while I was brushing my teeth, I had an epiphany. A different way of thinking about the problem. And so I immediately rushed to my computer, coded up the solution, and claimed my AC.

I've also had plenty of moments where I've come up with a solution while on a walk. And because I'm walking outside instead of being at my computer, I can take the time to think about the implementation details and achieve mental clarity before I begin implementing. Long challenges give me the time to truly appreciate problem solving as an art.

## 3. Approximation Problems
Most long challenges will also have an approximation problem as a tiebreaker. In these problems, it is impossible to compute the optimal answer in the given limits, and contestants compete to use heuristics and approximation algorithms to get as close to the optimal answer as possible. I'm really bad at these because I'm used to approaching problems by thinking about the worst case and the optimal solution instead of thinking about heuristics, but these types of problems can be interesting to think about nonetheless.

Now, I'll talk more specifically about two platforms that host long challenges regularly: CodeChef and HackerEarth.

## CodeChef Long Challenges
![image 1]({{ site.baseurl }}/assets/images/long-challenge-1.png)
<div style="text-align: center; margin-bottom: 5%"><em>Despite finishing earlier than some of the contestants above me, I still appear below them on the leaderboard, so I have no clue what algorithm CodeChef uses for tiebreakers.</em></div>

CodeChef is where I first encountered the long challenge format. Long challenges usually happen once a month and have 8-10 problems. Scores are determined solely based on points from problems (time is disregarded), so multiple contestants can get the same place. They used to be rated for all divisions, but recently they were downgraded to just being rated for Div 3, presumably to prevent cheaters from reaching 7 stars with only long challenges. They also no longer have approximation problems, which is a bit of a shame, but it also means it's now possible for me to get a perfect score :D.

In terms of problem quality, CodeChef problems do not shy away from heavy implementation, although that is often not the bottleneck in the difficulty of their problems. I'd say the early problems are usually random fluff, where you just do one print statement, while the later problems contain some interesting ideas. For example, the August long challenge had a problem called [Alternating LG Queries](https://www.codechef.com/AUG21A/problems/ALTLG), which I thought required a neat observation.

<details markdown="1" style="margin-bottom: 5%"><summary>The Observation</summary>
GCD and LCM behave like a clamp function, so it is sufficient to compress a series of composed GCD and LCM functions into one GCD and one LCM function. The rough proof of this is to consider each prime factor separately, then notice that the exponent of the prime factor is either min'd or max'd, which is exactly what a clamp function does.
</details>

In terms of difficulty, I would say that older long challenges were harder since they were rated for Div 1 and often contained problems requiring research of esoteric algorithms to solve. Nowadays, the long challenges rated for Div 3 can usually be AK'd (means to solve all the problems) by a competitor that is Master+ on Codeforces (at least, that's the rating I was when I did them). All in all, I'd say the quality of these contests are pretty good.

## HackerEarth Circuits
![image 2]({{ site.baseurl }}/assets/images/long-challenge-2.png)

HackerEarth Circuits usually happen once a month as well, and tend to have 7 algorithm problems and 1 approximation problem. Scores are determined first based on total points, then sum of solve times as tiebreaker. Personally, I don't understand why they even incorporate solve times into the score calculation, since the whole point of long challenges is to be friendly to different time zones and disregard the time aspect of programming competitions. Luckily, the times don't really matter in practice since the approximation problem provides a tiebreaker anyways.

Problem quality for HackerEarth contests is definitely sketchier than the popular platforms. The problems tend to gather their difficulty from the algorithms and concepts used instead of the level of logical thinking required to make the observations. Also, some of the problems definitely push the boundary of what it means to be a **programming** problem. Take for example the problem [A Trigonometry Function](https://www.hackerearth.com/problem/algorithm/trignometry-function-4c614376/) (and people say Codeforces is Mathforces!).

<details markdown="1" style="margin-bottom: 5%"><summary>How I solved this in contest</summary>
Let $S(n) = \sum_{k=0}^n f(k)$. We want to compute $S(q) - S(p - 1)$.

We can split $f(n)$ into the sum of two functions, $f_1(n) = 2 \cdot 7^{n/2} \cdot \cos(n \theta)$ and $f_2(n) = n \cdot 4^n$.

Let's first tackle $f_1(n)$. It's not even immediately obvious that for integer inputs this function outputs an integer, let alone how this function behaves. Let's use WolframAlpha to compute the first few terms of the sequence.

$$
f_1(0) = 2 \\
f_1(1) = 4 \\
f_1(2) = 2 \\
f_1(3) = -20 \\
f_1(4) = -94 \\
f_1(5) = -236
$$

Ok, I'm not seeing any pattern at all. But maybe OEIS does. While there isn't an entry on OEIS for this exact sequence, there is an entry on OEIS for $a(n) = f_1(n) / 2$: [A213421](https://oeis.org/search?q=1%2C+2%2C+1%2C+-10%2C+-47%2C+-118&sort=&language=english&go=Search). Most of the formulas aren't too useful, except for the last one: $a(n) = 4 a(n - 1) - 7 a(n - 2)$.

Now remember, we don't need $a(n)$, but rather $S(n)$. Let's redefine $S(n) = \sum_{k=0}^n a(k)$ and just multiply our final answer by 2 at the end. A quick substitution will help us derive a recurrence relation for $S(n)$:

$$
\begin{align*}
S(n) &= a(0) + a(1) + \dots + a(n - 1) + a(n) \\
&= S(n - 1) + a(n) \\
&= S(n - 1) + 4 a(n - 1) - 7 a(n - 2)
\end{align*}
$$

Now we can calculate $S(n)$ in $\mathcal O(\log n)$ time using [matrix exponentiation](https://usaco.guide/plat/matrix-expo?lang=cpp). In our matrix, we maintain the values $a(n), a(n - 1), S(n)$.

For $f_2(n)$, there's a simple closed form from [WolframAlpha](https://www.wolframalpha.com/input/?i=sum+n*4%5En) (WolframAlpha is OP): $\sum_{k=0}^n f_2(k) = \frac{4}{9}(3 \cdot 4^n n - 4^n + 1)$.

</details>

<details markdown="1" style="margin-bottom: 5%"><summary>Rant about HackerEarth Contest Preparation</summary>
Aside from problem quality itself, the quality of the preparation and contest design is questionable. I questioned earlier why solve times were included in the scoring, but I think the real reason is simply that the devs recycled the contest system used for their short contests and couldn't be bothered to adapt it to sensible rules for a long contest. Furthermore, there's almost always some issue with at least one of the problems in each contest. [One problem](https://www.hackerearth.com/practice/algorithms/dynamic-programming/introduction-to-dynamic-programming-1/practice-problems/algorithm/matrix-and-xor-operation-a2e19185/) from December Circuits incorrectly handled certain inputs. The checker for the [approximation problem](https://www.hackerearth.com/practice/basic-programming/bit-manipulation/basics-of-bit-manipulation/practice-problems/approximate/longest-grid-path-a28ff86f/) in July Circuits was wrong and allowed paths going through blocked cells. The checker for the [approximation problem](https://www.hackerearth.com/practice/algorithms/string-algorithm/basics-of-string-manipulation/practice-problems/approximate/kolmogorov-78780f09/) in January Circuits contradicted the output format in the problem statement, and participants literally had to figure out that information from repeated submissions during the contest. Finally, most recently HackerEarth contests banned participants from copy pasting into the submission box for no reason, meaning they somehow expect you to retype complex algorithms from scratch into their submission box instead of importing it from your library. It's possible that they've fixed that issue, but I haven't competed on HackerEarth recently, because why would I?

The worst offense is that these mistakes are never corrected because nobody on the contest team ever replies. I even personally emailed the contest team for one of their contests once and only got a reply several days after the contest ended. Needless to say, I've stopped competing on this platform for the time being. It's really a shame that one of the only sources for long challenge contests was butchered so badly by poor preparation.
</details>

So that concludes my thoughts about long challenges. They're an underappreciated format that I wish there were more of, and they can be a lot of fun to compete in. Let me know your thoughts in the comments below.