---
layout: post
title: "How to Understand Hard Tutorials"
tags: [tutorial, opinion]
featured: true
usemathjax: true
---

*Hooray, it's my first post of October, meaning I've kept this blog running for at least a month!*

Tell me if this sounds familiar: you see a new tutorial blog posted on Codeforces about some crazy advanced topic. You read through it, and you find it goes way over your head really quickly. Or you open an editorial for some problem, and you get lost after reading the first few lines. While I can't guarantee you'll be able to understand every tutorial after reading this article, I can provide some insight on how I approach reading and understanding tutorials.

## 1. Make sure you meet the prerequisites.
Every tutorial assumes some amount of previous knowledge before reading it. For example, most tutorials assume you know how to code and possess an understanding of mathematics up to a high school level. A tutorial that builds on an existing topic will also usually require knowledge of that previous topic (i.e. a tutorial on persistent segment trees likely requires you to know what a segment tree is). Authors usually don't outright tell you what prerequisites are required, but you can infer them by skimming through the tutorial and looking for keywords. If a tutorial suddenly states "and now this is just a trivial application of convex hull trick" without further elaboration, "convex hull trick" is probably a prerequisite assumed by this tutorial.

Let's give an example. Say you're reading [this article on subtree lazy propagation for link-cut trees](https://codeforces.com/blog/entry/80145). What are the prerequisites?
1. Know what a link-cut tree is. It's literally in the title.
    1. Digging into link-cut trees reveal that you'll need to know what a splay tree is, since those are used to implement a link-cut tree is. More fundamentally, knowledge of what trees are and common properties of trees can help.
2. How to maintain subtree aggregate data for a link-cut tree.
3. How lazy propagation works, which you can pick up from a tutorial about lazy propagation for segment trees, since that's usually the simplest context where lazy propagation is introduced.

Most of those were just deduced from the title/first paragraph, which is quite convenient.

Here's another example: [this action-packed blog about generating functions](https://codeforces.com/blog/entry/77468). What are the prerequisites?
1. Being very comfortable with manipulating algebraic expressions and summations, given the high volume of LaTeX I see.
2. Knowledge of common math functions like $\exp$, $\ln$, and $\binom{n}{k}$.
3. You don't actually need to understand how to perform operations on polynomials such as $P(x) Q(x)$, $\sqrt{P(x)}$, or $\ln P(x)$ in subquadratic time, just know that that's possible.

I'd say that this blog isn't intimidating because of the knowledge required, but because of its density and how quickly it escalates. I believe that if you meet the prerequisites (most of which is likely acquired in your high school or undergrad depending on where you live), you can understand this blog by slowly working through the blog line by line. Which leads me to my second point...

## 2. Don't skip sections!
This is the fastest way to get lost. Tutorials are often self-building, where each section feeds understanding of the next. If you don't understand a section, I would highly recommend not moving on to the next section until you understand it, because it is very unlikely that a later section will clarify what you don't understand, and more likely that you'll just get lost.

**There is one exception to this rule: proofs.** Proofs are slightly different, because they're sometimes offered as a supplement for completeness or satisfaction rather than understanding. I've often found that if I understand the general intuition behind the concept, I can skip the proofs with little consequence (though usually I read the proof anyways out of curiosity). I wouldn't recommend always skipping over proofs though. A good proof can sometimes make things more clear.

## 3. Actively think about what you're reading.
Sometimes, you followed steps 1 and 2, but you still get stuck on a section. This might be because the step that the author is taking is not intuitive, or because the author skipped a few steps in between. While the author could be to blame for not doing enough handholding, the reader can also try to think on their own about why a step is true. In fact, it can sometimes be more satisfying to figure out why a step holds true on your own rather than getting it all from the author. Coming up with your own explanation or intuition for some concept is a great way to improve your thinking abilities. This isn't an invitation to authors to be as terse as possible in their writing though: a tutorial that skips too much is simply not helpful to anyone.

## 4. You rarely read a good tutorial once.
There are some blogs that I frequently refer back to when I want to recall how to do something. I don't have the best memory, so if I read a blog about ["queue undo trick"](https://codeforces.com/blog/entry/83467) a year ago, the most I might remember when I see such a problem is "oh, this is possible, I read some blog about this." Then, I'll pull up the blog from my bookmarks and figure out the implementation from there. Or other times, I'll see a cool blog but conclude I don't meet the prerequisites for it yet, so I'll store the second half of the article for future use. In fact, one such example is the [generating functions blog](https://codeforces.com/blog/entry/77468) I mentioned earlier, which I still have yet to fully read through even part 1, mostly because I'm too lazy to read and actively think about the later sections at the moment. The point is, I recommend saving blogs you like so you can come back to it when you have a better understanding of the prerequisites, or when you just want to recall the information.

## 5. Finally, ask if stuck.
I put this as the final step because I feel people often skip to this step too often before considering the previous ones. But indeed, sometimes the author simply didn't explain a part as clearly as they could have. Or they skipped over a non-trivial step thinking it was trivial. **My biggest pet peeve is when someone asks to simply "explain it again."** Too often, people ask something to the effect of "can you explain it again in more detail?" The issue here is it's not targeted. I have no idea which section/line you're confused about. Asking something like "I don't get how X is true in section Y because Z" is way more useful because the author immediately knows which section was unclear and can tailor their response to that question.

## Extra: Taking Notes?
I leave this as an extra section because I personally don't do this, but I know other people do. I have heard that taking notes yourself can help you retain the information better and also lets you organize the information in a way that helps with recall (step 4). However, when I personally tried it once, I deemed note taking to be too much effort for marginal benefit in return. Ultimately, it depends on what type of person you are. We all learn differently, so do what works for you.

---

That's all I have. I know some of this might sound like common sense, but one of my favorite quotes is "common sense is not common practice," and I hope this article can encourage some more common practice in your learning. Let me know if you agree or disagree with any of my suggestions.