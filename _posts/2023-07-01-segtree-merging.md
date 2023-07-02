---
layout: post
title: "Segment Tree Merging"
tags: [tutorial, algo, ds]
featured: true
usemathjax: true
---

Segment tree merging often serves as an alternative to small-to-large while also cutting an extra log factor, or can be used for "directed" merging (i.e. the merging is not symmetric w.r.t. both sets). The extent to which this technique can be applied is actually quite extensive, and a lot of it is not documented in English (the only English resource I've found is [this cf post](https://codeforces.com/blog/entry/49446)), so I hope to bridge the gap with this post.

## The Technique

Suppose you have several [dynamic segment trees](https://usaco.guide/plat/sparse-segtree?lang=cpp). For our example, our segment trees will support point update and range sum.

```c++
const int NODES = 1e7 + 5;

int id = 1, sum[NODES], pl[NODES], pr[NODES];

int query(int p, int l, int r, int i, int j) {
    if (i > r || j < l || !p)
        return 0;
    if (i <= l && r <= j)
        return sum[p];
    int m = (l + r) / 2;
    return query(pl[p], l, m, i, j) + query(pr[p], m + 1, r, i, j);
}

int update(int p, int l, int r, int i, int v) {
    if (!p)
        p = id++;
    if (l == r) {
        sum[p] = v;
        return p;
    }
    int m = (l + r) / 2;
    if (i <= m)
        pl[p] = update(pl[p], l, m, i, v);
    else
        pr[p] = update(pr[p], m + 1, r, i, v);
    sum[p] = sum[pl[p]] + sum[pr[p]];
    return p;
}
```

Some notes on this implementation:
- Instead of C++ pointers, I prefer to implement dynamic segment trees by allocating a giant pool of nodes and using `int` as "pointers" (really indices to nodes in the pool).
- In the function signatures, `p` is the pointer to the current segtree node, `l` and `r` are the node range, `i` and `j` are the query range, and `v` is the updated value at index `i` in the update method.
- In the global scope, `id` is the next unused node in our pool. `sum` is the sum of our segment tree node and `pl` and `pr` are pointers to the node's left and right child.
- One neat thing about this implementation (and the reason why I prefer it over a C++ pointer approach) is that "null" is represented by index 0 and has sum 0, so we do not need to worry about dereferencing null pointers.
- Both of these methods are clearly $\mathcal O(\log n)$ by the same logic as normal segment tree.

Ok, now suppose I want to support merging two segment trees into one. Here is what that method looks like:

```c++
int merge(int p1, int p2) {
    if (!p1)
        return p2;
    if (!p2)
        return p1;
    pl[p1] = merge(pl[p1], pl[p2]);
    pr[p1] = merge(pr[p1], pr[p2]);
    sum[p1] = sum[pl[p1]] + sum[pr[p1]];
    return p1;
}
```

Essentially, we traverse down both segtrees at the same time. If either segtree is empty, we return the other. If both exist, then we recursively merge both children and we return the node of the first segtree.

What is the complexity of this? While a single merge may do up to $\mathcal O(n)$ work, the total complexity is bounded by $\mathcal O(n \log n)$ assuming the total sizes of all our segtrees add up to $n$. This is because whenever we keep recursing down in the merge method, we destroy the node of the second segtree (`p2` is no longer needed). So the total amount of work done by the merge method is bounded by the number of segtree nodes we can destroy, and we can only create at most $\mathcal O(n \log n)$ segtree nodes.

## POI Tree Rotations

Links to [$n \leq 2 \cdot 10^5$](https://szkopul.edu.pl/problemset/problem/sUe3qzxBtasek-RAWmZaxY_p/site/?key=statement) and [$n \leq 10^6$](https://szkopul.edu.pl/problemset/problem/b0BM0al2crQBt6zovEtJfOc6/site/?key=statement)

In this problem, we are given a binary tree, we have the option of swapping the children of each internal node of the tree, and we want to minimize the number of inversions in the inorder traversal of this tree.

Firstly, we might observe that each internal node's decision is independent of other internal nodes. Specifically, for each internal node, if $y$ was the number of pairs of elements in the left and right subtrees where the one in the left is greater than the one in the right, then the number of inversions contributed is $\min(y, sz[left] \cdot sz[right] - y)$, as we can either swap or not swap the children. So it remains to calculate $y$ quickly for each internal node.

One approach is small-to-large merging. We start at the leaves and maintain some data structure that stores elements and can query for the number of elements less than $x$ in the data structure. This could be a [pbds ordered set](https://codeforces.com/blog/entry/11080) or some other binary search tree. We merge these data structures bottom up with small-to-large merging and count the number of inversions between two subtrees as we merge into their parent. The complexity will be $\mathcal O(n \log^2 n)$.

But that's not the best we could do for this problem. Consider choosing a dynamic segment tree as our data structure. And instead of merging small-to-large, we use the merge method outlined above. Then the complexity becomes $\mathcal O(n \log n)$.

But wait, how do we count the number of inversions as we merge? The small-to-large merging approach looked something like this:

```c++
const int MAXN = 2e5 + 5;

long long inv;
ordered_set st[MAXN];

int merge(int a, int b) {
    bool flip = false;
    if (st[a].size() < st[b].size()) {
        st[a].swap(st[b]);
        flip = true;
    }
    for (int x : st[b]) {
        int cnt = st[a].order_of_key(x);
        inv += flip ? cnt : st[a].size() - cnt;
    }
    for (int x : st[b])
        st[a].insert(x);
    return a;
}
```

The key thing is that we had to iterate over every element in the smaller of the two sets and query for the number of inversion pairs it forms with the other set, which already consumes $\mathcal O(n \log^2 n)$ time.

The fix is to embed the inversion counting logic into our segtree merge. The "indices" of our segtrees are the values of its elements, and the aggregate is sum. Suppose that `p1` and `p2` are the segtrees of our left and right subtrees respectively. When we are at a stage in the merge method where both `p1` and `p2` exist, the number of inversions increases by `sum[pl[p2]] * sum[pr[p1]]`. All other inversion pairs will be counted at a later stage in the recursion. You can refer to the code below for more clarity:

```c++
long long inv;

int merge(int p1, int p2) {
    if (!p1)
        return p2;
    if (!p2)
        return p1;
    inv += (long long) sum[pl[p2]] * sum[pr[p1]];
    pl[p1] = merge(pl[p1], pl[p2]);
    pr[p1] = merge(pr[p1], pr[p2]);
    sum[p1] = sum[pl[p1]] + sum[pr[p1]];
    return p1;
}
```

So with this, we can solve the problem in just $\mathcal O(n \log n)$!

*Note that even though the complexity is correct for the $n \leq 10^6$ version of this problem, the memory limit is too tight for $\mathcal O(n \log n)$ memory, so to pass that version you have to merge a less sparse data structure such as splay trees or treaps.*

Now let's check out some other problems solvable with this technique which are a little harder to solve with small-to-large.

## [PKUWC2018 Minimax](https://loj.ac/p/2537)

**Translation:** You are given a **binary tree** of $n$ nodes rooted at $1$. If a node is a leaf, it is assigned an input weight. Otherwise, it is assigned some probability $p_i$. It has a $p_i$ chance of getting set to the maximum of its children and $1 - p_i$ chance of getting set to the minimum of its children. **It is guaranteed that all leaf node weights are distinct.** Now, if the root could become $m$ different values, then let $V_i$ denote the $i$th smallest value and $D_i$ denote the probability of the root becoming it, compute

$$
\sum_{i=1}^m i \cdot V_i \cdot D_i^2 \mod 998244353
$$

**Input format:** First line is $n \leq 3 \cdot 10^5$. Second line contains $n$ integers, the $i$th integer is the parent of node $i$ or $0$ if $i = 1$. Third line contains $n$ integers, the $i$th integer equals its weight $w_i \leq 10^9$ if it is a leaf node or $p_i \cdot 10^4$ otherwise (guaranteed to be an integer).

<details markdown="1" style="margin-bottom: 5%"><summary>Solution</summary>

Let's maintain for each node $i$ the set of values that node $i$ could become. For transition, say you're iterating over all values in a child's list. Accumulate the probabilities of the values in the other list that are larger than it, then multiply $p_i$ with that as well as its current probability to get the probability it is chosen. Same for smaller than it. This is $\mathcal O(n^2)$ as we need to iterate over both children's lists, and finding the position in the other can be maintained with some two pointers.

One could hope to speed this up with some sort of small-to-large merging, but unfortunately there isn't an easy way to update the probabilities in the big list. Instead, consider using a merging segment tree. Each node maintains its set of values as a segment tree sorted by values. The leaf nodes store the probabilities of attaining each of those values. The segment tree also maintains a lazy multiply value.

The merge function is more nuanced. As we traverse down the merge, we maintain lazy values $v_1$ and $v_2$ denoting the contribution of segment tree $1$ and $2$ to the other segment tree respectively. When we reach a point in the merge function where one exists but not the other, we lazy multiply the segment tree leftover. When both exist, consider the effect they have on each other. If we descend left down the segment tree, then we add the sum of the right segment tree all multiplied by $1 - p_i$ as all of those values are larger, so you need their sum of probabilities times $1 - p_i$ to contribute to the probability of selecting the values on the left. Analogously, when we descend right, we add sum of left segment tree multiplied by $p_i$. The complexity is $\mathcal O(n \log n)$.

To get a better sense of what I'm talking about it, you can refer to [my submission](https://loj.ac/s/1387916).

---

</details>

## Range Sort

Given an array of $n \leq 10^5$ integers, process $q \leq 10^5$ queries where you sort some subarray $[l, r]$ either increasing or decreasing. Output the final sequence after all queries.

I'm not aware of any submission link to this problem at the moment ([http://www.lydsy.com:808/JudgeOnline/problem.php?id=4552](http://www.lydsy.com:808/JudgeOnline/problem.php?id=4552) used to exist but that online judge is offline now). You could also submit as an overkill solution to [https://atcoder.jp/contests/abc237/tasks/abc237_g](https://atcoder.jp/contests/abc237/tasks/abc237_g).

<details markdown="1" style="margin-bottom: 5%"><summary>Solution</summary>

The key is to represent the array as a set of contiguous intervals, each one sorted either increasing or decreasing. So initially the array can have up to $\mathcal O(n)$ segments. With each query, we cut up to two segments on the ends, erase all segments fully contained within our query segment, and insert the query segment into the set. So the number of new segments in our set is at most $3$.

As for representing the segments, we can represent them as dynamic segment trees. And we just need to be able to merge and split these segments trees. Merge is the same as above, split is not difficult either and only creates at most $\mathcal O(\log n)$ new segtree nodes. The code below shows how to split a segtree into two segtrees, one containing the $k$ smallest elements and the other containing the remaining.

```c++
pair<int, int> split(int p, int k) {
    if (!p)
        return {0, 0};
    int q = id++;
    if (sum[pl[p]] >= k) {
        tie(pl[q], pl[p]) = split(pl[p], k);
        sum[p] -= k;
        sum[q] = k;
        return {q, p};
    } else {
        tie(pr[p], pr[q]) = split(pr[p], k - sum[pl[p]]);
        sum[q] = sum[p] - k;
        sum[p] = k;
        return {p, q};
    }
}
```

---

</details>

## More problems

- [https://codeforces.com/contest/1515/problem/H](https://codeforces.com/contest/1515/problem/H)
- [https://loj.ac/p/2553](https://loj.ac/p/2553)
- [https://loj.ac/p/2722](https://loj.ac/p/2722)
- [https://loj.ac/p/3340](https://loj.ac/p/3340)
- [https://www.spoj.com/problems/COT3/](https://www.spoj.com/problems/COT3/)