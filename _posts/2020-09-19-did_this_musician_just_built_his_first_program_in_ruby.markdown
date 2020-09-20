---
layout: post
title:      "Did this musician just built his first program in Ruby!? "
date:       2020-09-20 01:16:53 +0000
permalink:  did_this_musician_just_built_his_first_program_in_ruby
---


Do you know that feeling of accomplishment when you can say you've completed a project or task that took a lot of preparation and support from your family, peers, and teachers? I think I do.

I'm a dreamer and my first inclination in creating something is to make it perfect and complete. I think that may come from my classical music training. There is that deep current in the culture of the classical music world of preparing so well that a performance comes off as perfectly as possible. Brahms is known to have taken so long to complete his first symphony partly because of the shadow of Beethoven's greatness cast over his shoulder, but maybe more so because of Brahms's perfectionism. With my journey through creating my first program, I just kept running into my shortcomings of knowledge and my lack of speed. I found that I had to change my mindset.

I moved from focusing on a program that would "save the world", as I sometimes call it, to one that works. At first I wanted to create a program that would help the user discover musical scores previously unknown to them. It would be a way for a classical musician to discover repertoire they would never have actively searched out. Not everyone has access to large libraries of sheet music, or a well-stocked music store, or, in this corona virus times, a library. So, creating a program that would help expand a musician's repertoire outside of their usual sources and tastes seemed timely and useful. 

The first place where I and a number of fellow musicians look for public domain music is the IMSLP website. Currently it has 531,632 scores and it's growing. When I found that it has an API as well, I was very excited! I encountered a problem though; In order to make the data manageable, while providing a lot of different end points, the API does not provide any hashes to search by instrument on the first level. If I decided to scrape the website for just keyboard works, I'd still only have a random sampling of composers beginning with the letter "A". 

This was disappointing. So, I searched for other classical score databases and found the [OpenOpus.org](http://openopus.org) API. While it poses a similar problem, I (and my teacher) thought for the purposes of this exercise it would be best to pivot the focus of the program from an instrument-score focused to a composer-information focused project. 

Now that I had the data, how was I to create a user interface? Having only just dipped my toes into a local environment, and having a shaky understanding of the concepts of object orientation, APIs, and environment set-up, would I be able to do it? I wasn't sure. Thankfully, I had a lot of support from a lot of people: my family, by taking care of my daughter and household duties when I was holed up in the office; the patient TAs on Ask A Question taking time to help me through the labs; my educational coach, Katie by helping me put my journey into perspective and moving me through my stuckness; members of my cohort, for their encouragement and help; and my teacher, Ally, for encouraging me, helping me troubleshoot issues in my project, and patiently going over basic concepts with this knucklehead! While these statements may sound more like an acceptance speech for an Academy Award than a blogpost, I think that I need to acknowledge how crucial these folks have been in helping me acheive this first project. (And I haven't even had my dreaded assessment yet!)

So, I've learned that sticking to my goals, relying on my support team, taking stock of my resources, and being grateful to those who have helped me out make this journey possible and worthwhile. And I've learned some code along the way, too!
