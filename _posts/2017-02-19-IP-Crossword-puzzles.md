---
layout: post
title: "Making crossword puzzles with integer optimization"
categories: journal
tags: [projects, crossword, optimization, mathematics]
---

I love solving crossword puzzles.  The Will Shortz-edited [NYTimes puzzle](http://nytimes.com/crosswords) has set the bar for many years, although there is now a growing [indie crossword scene](https://fivethirtyeight.com/features/indie-crossword-puzzlers-are-shaking-up-a-very-square-world/).  I've tried making my own, and failed, an embarrassing number of times.  I always wondered: how the heck do people do it?  Do they agonize over a piece of grid paper until the eraser rubs right through the page, which was my method of choice?

Of course not.  I discovered that the majority of crossword constructors use software, and the challenge isn't putting the grid together, but feeding the computer a solid, and fresh set of words.  Upon further investigation, it appears the industry standard software is a Windows-only app called [Crossword Compiler](http://www.crossword-compiler.com) that starts at about 50 bucks.  Surprisingly though, I could not find a freeware (much less open source) version of this functionality, even at a lower quality.  (If you know of one, please [contact me](http://twitter.com/thestevemo).)

This is a travesty.  We need to get crossword construction into the hands of the people.  So I attempted to homebrew my own crossword compiler, and I'd like to share the first draft.


## IO formulation of the crossword generation problem 

It seems like most of the crossword generation research out there formulates the problem as a [**constraint satisfaction problem**](http://aima.cs.berkeley.edu/2nd-ed/newchap05.pdf).  Unfortunately, this field is not in my wheelhouse, so I chose a different formulation: integer optimization (IO) aka integer programming (IP).  (There is actually a lot of parallels between the two, and possibly something like a one-to-one correspondence, but that is beyond the scope of my brain and this blog post at the moment.)

So let's formulate crossword puzzle generation as an IP, and let's start by defining our decision variables.  There seem to be two approaches possible: (1) assigning words to slots, or (2) assigning letters to cells.  In the letter-to-cell approach, we make sure each cell in the grid has a single unique letter assignment, and the hard part is then making sure each slot in the puzzle ("23 down", "5 across") actually spells a word in the vocabulary.

I'll take the words-to-slots approach instead.  This method makes sure each slot has a single word assigned, and the hard part is making sure the intersecting letters for across and down slots are the same.  But let's start with the easy stuff.

(Removed paragraph.)

As a preprocessing step, we can go through and set $$z_{k,m}=0$$ wherever $$|W_k|\neq |S_m|$$, i.e., we can't assign a word to a slot if it doesn't even fit.  We can then define our first two sets of constraints in the usual IP way, as

$$
\begin{align*}
\sum_k z_{k,m} &= 1 \ \ \forall k \\
\sum_m z_{k,m} &\leq 1 \ \ \forall m
\end{align*}
$$

The first constraint ensures every slot has exactly one word assigned to it.  The second constraint ensures every word is assigned to at most one slot (crosswords don't like a word to occur more than once).

Now things get tricky.  We now need to ensure that each letter in the assigned word for an *across* slot matches the corresponding letters in the intersecting *down* slots.  This is a simple idea that is just a pain to translate into an IP formulation.  Let's introduce the following notation: $$G=\{g_{ij}\}$$ for the set of all open cells in the grid, indexed by row $$i$$ and column $$j$$; also $$r(g)$$ and $$c(g)$$ to denote the slot index of the row and column, respectively, corresponding to grid $$g$$; and $$P_m$$ to denote the set of indices of words that are possible in slot $$m$$.  Now our last constraint is:

$$
\sum_{k \in P_{r(g)}} z_{k, r(g)} = \sum_{k \in P_{c(g)}} z_{k,c(g)} \ \ \forall g
$$

Wow that's nasty.  But we can make it happen.  Last but not least, let's express our objective function as

$$
\text{max} \ \sum_k \sum_m c_k z_{k,m}
$$

where $$c_k$$ is some score associated with each word.  For example if we had some trendy, never-before-used phrase that we wanted to make sure made it into our next puzzle, we'd give it $$c_k = 100$$, and some awful crosswordese word like "oleo" we'd give $$c_k = 1$$.  Etc.  And that's it!


## Implementation in Python

To implement this in Python, I tried to stay away from any unnecessary dependencies, so I used pure base Python and the `pulp` package for the integer optimization.  All the code is in a [Github repo over here](http://github.com/stmorse/ip-crossword) so I won't bore you with the coding details, but I basically wrote a `Grid` class to handle all the messy grid manipulation, a couple dicts with preprocessing information, and a code chunk with the IP formulation as just described.

However, although it works, it can only manage grids that are $$3\times 3$$ or $$4\times 4$$, not much bigger.  I think this is due to a couple non-formulation related problems that I'd like to fix:

1. Improve the grid handling --- currently very hacky, using base Python, lots of list comprehensions and loops ... blech.
2. Use something better than the default solver for `PuLP`.
3. Related: a better solver might be able to give insight if there are pre-processing steps that speed things up.
4. Improve the word list and add points per word. Since most of a crossword maker's struggle (I'm told) is getting a good, well-sorted word list. Currently I'm using a ``ospd.txt file with a few thousand words, most of which are crummy/archaic, and assigning each word a random score.


That's all for now.  If you have comments, questions feel free to message me on [Twitter](http://twitter.com/thestevemo).  Thanks for reading!



