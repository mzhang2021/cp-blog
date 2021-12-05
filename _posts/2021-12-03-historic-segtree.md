---
layout: post
title: "Historic Information on Segment Trees"
tags: [tutorial, algo]
featured: true
usemathjax: true
---

It's been a while, but I'm back with some more estoric knowledge! Historic information is a concept I've only seen briefly mentioned in [this tutorial](https://codeforces.com/blog/entry/57319) on segment tree beats, and the section only covers one example of it, so I'll cover some more examples in this article.

## What is Historic Information?

Let's look at the following problem: You're given an array of $n$ integers $a_i$, and you have another array $b_i$ initially equal to $a_i$. You want to support the following operations.

1. For $l \leq i \leq r$, add $x$ to $a_i$ ($x$ may be negative).
2. Query for $\max_{i=l}^r a_i$.
3. Query for $\max_{i=l}^r b_i$.

After each operation, update $b_i$ to $\max(b_i, a_i)$ for all $1 \leq i \leq n$. $b_i$ is essentially the maximum $a_i$ over time, and is an example of historic information.

The first two operations are pretty standard lazy segment tree. We use a single lazy variable to store the sum of all updates applied to a node. What about the third operation? One option would be to add the lazy value to $b_i$ once you're pushing down at a node, but that isn't exactly right. Consider the following scenario:

1. You add $2$ to $a_i$.
2. You add $6$ to $a_i$.
3. You add $-15$ to $a_i$.

The total lazy value is adding $2 + 6 - 15 = -7$ to $a_i$, so $a_i \to a_i - 7$. But what about $b_i$? It's incorrect to say $b_i \to b_i - 7$, because actually $b_i \to b_i + 8$. This is because the value was changed three times: $b_i \to b_i + 2 \to b_i + 2 + 6 \to b_i + 2 + 6 - 15$. The maximum of all of those would be $b_i + 2 + 6 = b_i + 8$. And here lies the issue: just storing the sum of all updates as our lazy value is not sufficient to track the history of changes across multiple operations.

To remedy this, we introduce another lazy value, dubbed `hlazy` (short for "historic lazy"), which stores the maximum prefix of lazy updates applied. So say there were four updates applied to a node: $\\{+2, +3, -6, +9\\}$. Then `hlazy` would equal $\max(0, +2, +2 + 3, +2 + 3 - 6, +2 + 3 - 6 + 9) = 8$. When we push down node $u$ onto node $v$, we have `v.hlazy = max(v.hlazy, v.lazy + u.hlazy)`. And now, we update `b[i] = max(b[i], a[i] + hlazy)` which gives the correct new value of $b_i$.

<details markdown="1" style="margin-bottom: 5%"><summary>Code</summary>

```c++
// based on how I implement lazy segment tree
// https://github.com/mzhang2021/cp-library/blob/master/implementations/data-structures/SegmentTreeNodeLazy.h
struct Node {
    int maxA, maxB, lazy, hlazy, l, r;

    void leaf(int val) {
        maxA = maxB = val;
        lazy = hlazy = 0;
    }

    void pull(const Node &a, const Node &b) {
        maxA = max(a.maxA, b.maxA);
        maxB = max(a.maxB, b.maxB);
    }

    void push(const Node &other) {
        hlazy = max(hlazy, lazy + other.hlazy);
        lazy += other.lazy;
    }

    void apply() {
        maxB = max(maxB, maxA + hlazy);
        maxA += lazy;
        lazy = hlazy = 0;
    }
};
```

---

</details>

## A Harder Example

Now let's look at another problem: You're given an array of $n$ integers $a_i$, and you have another array $b_i$ initially equal to $a_i$. You want to support the following operations.

1. For $l \leq i \leq r$, add $x$ to $a_i$.
2. Query for $\sum_{i=l}^r a_i$.
3. Query for $\sum_{i=l}^r b_i$.

After each operation, add $a_i$ to $b_i$ for all $1 \leq i \leq n$. So this time our historic information is an accumulation of all $a_i$ over time.

Operation 3 is a bit trickier to deal with this time. To deal with it, we introduce a new concept: a timestamp. We number each of the operations as happening at times $1, 2, \dots, m$. For each node, we store a timestamp denoting the time of latest operation we've applied to this node (or $0$ if no operations have been applied to this node yet). Now, let's take a look at how $b_i$ changes as we apply updates to it. For sake of simplicity, say we're updating a leaf node. Let's say we apply $+x$ at time $t_x$, $+y$ at time $t_y$, and $+z$ at time $t_z$, where $t_x < t_y < t_z$. The initial timestamp before applying these updates is $t_i$.

- $b_i \to b_i + a_i \cdot (t_x - t_i) + (a_i + x) \cdot (t_y - t_x) + (a_i + x + y) \cdot (t_z - t_y)$
- $a_i \to a_i + x + y + z$
- $t_i \to t_z$

Let's simplify the update to $b_i$:

$$
b_i \to b_i + a_i \cdot (t_x - t_i) + (a_i + x) \cdot (t_y - t_x) + (a_i + x + y) \cdot (t_z - t_y) \\
= b_i + a_i \cdot t_x - a_i \cdot t_i + a_i \cdot t_y - a_i \cdot t_x + x \cdot (t_y - t_x) + a_i \cdot t_z - a_i \cdot t_y + (x + y) \cdot (t_z - t_y) \\
= b_i + a_i \cdot (t_z - t_i) + x \cdot (t_y - t_x) + (x + y) \cdot (t_z - t_y)
$$

Let's denote $x + y + z$ as `lazysum` and $x \cdot (t_y - t_x) + (x + y) \cdot (t_z - t_y)$ as another lazy value `cumsum` (short for "cummulative sum").

How does push down work? Say we have a node with updates $\\{+u, +v, +w\\}$ and we push down another node with updates $\\{+x, +y, +z\\}$. Then we get:

- Initial `cumsum`: $u \cdot (t_v - t_u) + (u + v) \cdot (t_w - t_v)$
- After push down:

$$
u \cdot (t_v - t_u) + (u + v) \cdot (t_w - t_v) + (u + v + w) \cdot (t_x - t_w) + (u + v + w + x) \cdot (t_y - t_x) + (u + v + w + x + y) \cdot (t_z - t_y) \\
= \text{cumsum} + (u + v + w) \cdot (t_x - t_w + t_y - t_x + t_z - t_y) + x \cdot (t_y - t_x) + (x + y) \cdot (t_z - t_y) \\
= \text{cumsum} + \text{lazysum} \cdot (t_z - t_w) + \text{other.cumsum}
$$

I will leave it as an exercise to figure out the update for a non-leaf node and how to merge two segment tree nodes. Once you've figured it out, you can check your answers with the code below:

<details markdown="1" style="margin-bottom: 5%"><summary>Code</summary>

```c++
// based on how I implement lazy segment tree
// https://github.com/mzhang2021/cp-library/blob/master/implementations/data-structures/SegmentTreeNodeLazy.h
struct Node {
    int sumA, sumB, timestamp, lazysum, cumsum, lazyTimestamp, l, r;
    bool lazy;

    void leaf(int val) {
        sumA = val;
        sumB = timestamp = lazysum = cumsum = lazyTimestamp = lazy = 0;
    }

    void pull(const Node &a, const Node &b) {
        sumA = a.sumA + b.sumA;
        timestamp = max(a.timestamp, b.timestamp);
        sumB = a.sumB + b.sumB + a.sumA * (timestamp - a.timestamp) + b.sumA * (timestamp - b.timestamp);
    }

    void push(const Node &other) {
        if (lazy) {
            cumsum += lazysum * (other.lazyTimestamp - lazyTimestamp) + other.cumsum;
            lazysum += other.lazysum;
        } else {
            cumsum = other.cumsum;
            lazysum = other.lazysum;
            lazy = true;
        }
        lazyTimestamp = other.lazyTimestamp;
    }

    void apply() {
        sumB += sumA * (lazyTimestamp - timestamp) + (r - l + 1) * cumsum;
        sumA += (r - l + 1) * lazysum;
        timestamp = lazyTimestamp;
        lazysum = cumsum = lazyTimestamp = lazy = 0;
    }
};
```

---

</details>

## Applications

Ok, when on earth would you ever need this? Aside from contrived problems designed to test this technique, I've found historic information most useful when doing some sort of "all pairs" problem via linesweep. Take [this problem](https://codeforces.com/gym/103069/problem/G) from the 2020 ICPC Asia East Continent Final for example.

<details markdown="1" style="margin-bottom: 5%"><summary>Solution</summary>

The algorithm I describe below is very similar to the offline algorithm for solving [SPOJ DQUERY](https://www.spoj.com/problems/DQUERY/), which you can see [here](https://codeforces.com/blog/entry/8962?#comment-146571). For sake of brevity, I will assume that you have read and understood that solution in the paragraphs below.

We will use an offline algorithm. As we sweep from left to right, we maintain an array $p$ where each index's value is 1 if a subarray starting at that index is valid, and 0 otherwise. So say $a = [3, 9, 4, 3, 2, 9, 9, 3, 2, 3, 2, 2, 2]$. Then $p = [0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 1, 1]$. Now say you process the next element in your linesweep. There are two cases:

1. You've never seen this element before. In our example, say the next element is a $6$, so $a = [3, 9, 4, 3, 2, 9, 9, 3, 2, 3, 2, 2, 2, 6]$. Then now $p = [1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 1]$. Notice that we basically just did a range flip operation.
2. You've seen this element before. In our example, say the next element is a $9$, so $a = [3, 9, 4, 3, 2, 9, 9, 3, 2, 3, 2, 2, 2, 9]$. First, we delete the last occurrence of $9$ (we basically pretend it doesn't exist and set $p_i = p_{i+1}$ at that index). So $p = [1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 1, 1, 1]$. And now we add our new last occurrence of $9$ at the end, giving us $p = [0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 1]$. So we've essentially done just two range flip operations.

If we make $p$ a segment tree, we can query for the number of valid subarrays for each right endpoint. But our original problem needs the number of valid subarrays with left endpoint $\geq l$ for each right endpoint for ALL right endpoints in $[l, r]$. So to remedy this, we instead query for the historic sum of $p_i$ for all $l \leq i \leq r$. So we need a segment tree that supports range flip and historic sum. I leave the details of what values to store in the segment tree node as an exercise for the reader, because I think being able to figure this out will serve as a good test of understanding. If you're stuck, you can refer to my implementation [here](https://ideone.com/aADK7p) (ideone link because Codeforces gym links aren't public).

---

</details>

## Problems

Here are some other problems where I've used historic information. I don't have many at the moment, so if you have any more feel free to let me know.

[SPOJ GSS2: Can you answer these queries II](https://www.spoj.com/problems/GSS2/)

<details markdown="1" style="margin-bottom: 5%"><summary>Solution Sketch</summary>

Similar to the 2020 ICPC Asia East Continent Final problem explained above: linesweep, store only last occurrence, support range add and historic maximum. Explained in more detail [here]({{site.baseurl}}/gss/).

---

</details>

[Codeforces Round 751, Div 1 C: Optimal Insertion](https://codeforces.com/contest/1601/problem/C)

<details markdown="1" style="margin-bottom: 5%"><summary>Solution Sketch</summary>

Compress the input to values in the range $1, 2, \dots, n + m$. It's optimal to sort $b$ and have them be in the same relative order in $c$ to minimize the number of inversions. Now for each $b_i$, we can insert them in at the beginning, at the end, or between any $(a_i, a_{i+1})$ pair. Let's sweep through each of $a_i$ from left to right, and update a segment tree maintaining the number of inversions each element would add if inserted in that gap. Finally, for each element in $b$, query for the historic minimum of $b_i$ in the segment tree. So we need to support range add and historic minimum.

[Submission for Reference](https://codeforces.com/contest/1601/submission/133199732)

---

</details>

[UOJ 164](https://uoj.ac/problem/164)

Translation: Support the following operations:

1. For $l \leq i \leq r$, add $x$ to $a_i$.
2. For $l \leq i \leq r$, set $a_i$ to $\max(a_i - x, 0)$.
3. For $l \leq i \leq r$, set $a_i$ to $x$.
4. Query for $a_y$.
5. Query for $b_y$.

<details markdown="1" style="margin-bottom: 5%"><summary>Solution Sketch</summary>

This is the example problem for historic information described [here](https://codeforces.com/blog/entry/57319). The blog also contains the solution.

[Submission for Reference](https://uoj.ac/submission/519426)

---

</details>

---

And that's a wrap! Thanks for reading!

My next article idea (which will probably be published in a month from now) is to make a tier list of online judges. But this isn't gonna be just a baby tier list. I've made an insane number of accounts on different online judges, so this list will be as comprehensive as possible. Currently, I have the following list, and if you have any others to add, let me know. The only requirement is that the online judge must be in English.

<details markdown="1" style="margin-bottom: 5%"><summary>The List</summary>

- [Codeforces](https://codeforces.com/)
- [Atcoder](https://atcoder.jp/)
- [Codechef](https://www.codechef.com/)
- [SPOJ](https://www.spoj.com/)
- [UVa](https://onlinejudge.org/)
- [ICPC Live Archive](https://icpcarchive.ecs.baylor.edu/)
- [USACO](http://www.usaco.org/)
- [USACO Training](https://train.usaco.org/)
- [HackerRank](https://www.hackerrank.com/)
- [HackerEarth](https://www.hackerearth.com/)
- [LeetCode](https://leetcode.com/)
- [binarysearch](https://binarysearch.com/)
- [DMOJ](https://dmoj.ca/)
- [Aizu Online Judge](https://judge.u-aizu.ac.jp/onlinejudge/index.jsp)
- [PKU Online Judge](http://poj.org/)
- [Timus](https://acm.timus.ru/)
- [LightOJ](https://lightoj.com/)
- [oj.uz](https://oj.uz/)
- [CSA Academy](https://csacademy.com/)
- [Kattis](https://open.kattis.com/)
- [CodeDrills](https://codedrills.io/)

I decided not to add sites like [CSES](https://cses.fi/problemset/list/) or [Yosupo Judge](https://judge.yosupo.jp/) because they serve a different purpose than traditional online judges (e.g. CSES is a database of classical problems). I also might exclude interview prep sites like [LeetCode](https://leetcode.com/) since they also serve a different purpose, though I'm not sure at the moment.

---

</details>