---
layout: post
title: "Why does anyone uses I J for loop counter variables names"
date: 2013-01-02 02:19
comments: true
categories: 
---

Just because it's new 2013 year, I started to learn [Forth](http://en.wikipedia.org/wiki/Forth_(programming_language)) language. It's cool [stack-oriented](http://en.wikipedia.org/wiki/Stack-oriented_programming_language) language, with strict [KISS](http://en.wikipedia.org/wiki/KISS_principle) principle in design. It's a shame that it's completely obsolete and forgotten.

<!-- more -->


So, I started with a cool book 
["Starting Forth"](http://home.iae.nl/users/mhx/sf.html) (with writing a [kindlefodder](https://github.com/danchoi/kindlefodder) [recipe](https://github.com/darvin/kindlefodder/blob/master/recipes/starting_forth.rb)). A very nice book, btw. Turns out, the only serious and up-to-date open source implementation of ANS Forth [gforth](http://www.gnu.org/software/gforth/) has a broken homebrew formula, so I had to [fix it](https://github.com/darvin/homebrew/blob/master/Library/Formula/gforth.rb)

Also, anything around Forth is pretty obsolete - for example, there is couple [package](http://gitorious.org/forth-tools/supply/trees/master) [managers](http://code.google.com/p/halfpence/) and no comment - documentation generation system. So plan is to write one (probably it gonna be literate-programming style [docco](http://jashkenas.github.com/docco/) clone with Forth-specific features)

So, what about `I` and `J` as traditional loop counter variables names?

```
: table 
  cr 11 1 
  do 
    11 1 
    do 
      i j * 5 u.r
    loop 
    cr 
  loop ;

table
```

_(again, it's a shame that [Pygments](http://pygments.org/) [support Ada and Befunge](http://pygments.org/languages/), but does not support Forth)_

Here we have two loops. Outer loop uses `1st` and `2nd` item of control stack for counter and finish value (`1` and `11` initially), inner loop uses `3rd` and `4th` (`1` and `11` initially, as well). Word `I` in Forth copies `1st` value from control stack on top of stack, `J` copies `3rd` value from control stack on top of stack, so they look as loop counters. That's it! Full KISS! :)

Output:

```
1    2    3    4    5    6    7    8    9   10
2    4    6    8   10   12   14   16   18   20
3    6    9   12   15   18   21   24   27   30
4    8   12   16   20   24   28   32   36   40
5   10   15   20   25   30   35   40   45   50
6   12   18   24   30   36   42   48   54   60
7   14   21   28   35   42   49   56   63   70
8   16   24   32   40   48   56   64   72   80
9   18   27   36   45   54   63   72   81   90
10  20   30   40   50   60   70   80   90  100
 ok
```
