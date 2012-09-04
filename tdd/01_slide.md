!SLIDE
# Zen and the art of TDD #

!SLIDE
# Unit testing won't help your code

> The only reason to have Unit tests is to make sure that code that already works doesn’t break. 
Writing tests first, or writing code to the tests is ridiculous. 

Source: http://programmers.blogoverflow.com/2012/08/20-controversial-programming-opinions/

!SLIDE
\#2 most popular opinion. Why is this?

!SLIDE
Testing still in the minority - X % of projects contain it

!SLIDE
# Matt Steele

* Developer @ Union Pacific
* @mattdsteele
* http://matthew-steele.com

!SLIDE
Kent beck: "I get paid for code that works, not for tests"
Source: http://stackoverflow.com/questions/153234/how-deep-are-your-unit-tests/153565#153565

!SLIDE
So why do it?

!SLIDE
# 1. Proves code is correct

!SLIDE
Tests have two lives; when you're writing them, and then forever on.

Gives you behavior preservation.

!SLIDE
2. Fast feedback loop
Inventing on Principle https://vimeo.com/36579366
Closest we can get to this with normal tools is TDD

!SLIDE
3. Refactoring
I do it for me 
Provides a safety net

!SLIDE
4. Prevents you from doing stupid shit
Working Memory
http://www.ncbi.nlm.nih.gov/pubmed/17983308

!SLIDE
5. Design effects
Testing code that hasn't been written tests beforehand is a nightmare. (example jquery code)
You write your code very differently as you're writing it. (use bkeeper's example)
Solving design problems solves testing problems

!SLIDE
6. Makes you happy

!SLIDE
Any surprise languages we've historically valued least (JS) had the least testing?

!SLIDE
Thanks to
Katrina Owen http://www.confreaks.com/videos/1071-cascadiaruby2012-therapeutic-refactoring
