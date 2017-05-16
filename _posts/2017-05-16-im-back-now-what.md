---
layout: post
title: Consider This My First Commit to Getting Back On Track
date: 2017-05-16 13:43:00
description: What to do after you've fallen off track?
published: true
---

# I'm Back 
<br>
Being in the process of self-teaching or freelancing can be a cruel mistress. It requires having discipline, resources, and courage to keep going. Without any assistance at all, I believe it may be impossible. But even when all of those things are in place, life throws itself at you in beautiful ways, and you end up with zero contributions for several weeks. Let me be clear that I'm exclusively working on open source and personal projects at the moment, so my github is an active representation of the amount of work I get done.

<div class="img_row" style="height: 100%">
	<img class="col three" src="/img/contributions.png">
</div>
<div class="col three caption">
	That latest contribution is me opening an issue in my own repository. 
</div>

So that's just it. I ended up not writing or contributing any code for several weeks. What do I do now? 

# Now What?
<br>
Little ol' me didn't have the wherewithal to keep a detail journal of every little thing I was working on, but that doesn't mean I'm without documentation. **Tests**, **`git log`**, and **issue tracking**, can all help me piece together my work so that I can pick up where I left off.
<br><br>
Lucky for me, I've kept track of my work using Agile methodologies, or at least in the way I understand them. My issues are tracked with <a href="https://waffle.io" target="_blank">waffle.io</a>, and I'm able to go back and take a look at my progress and see what I was working on before my hiatus began.
<br><br>
Also, I can use the handy interface on github's website to view my current branch's changes to the code base by comparing it to my master branch. Keeping Agile work methods in mind, I always work on a feature branch and submit PRs to my master branch, even when working by myself.
<br><br>
Lastly, I'm able to use my tests to see what the intention of my current feature is. I often do TDD, especially now as I'm still learning how to write good (read: decent) specs. I start with feature tests, then write unit tests as need be. Together, they help express what the current code accomplishes, and what it does not yet.
<br><br>
# Lessons Learned?
<br>
It seems as though I've learned a few things here. Besides using Agile tools to piece together my own work, I've developed a strategy to help me get up to speed when approaching an existing codebase! Of course it's easier to be reintegrated as opposed to newly on boarded, but the sleuthing is very similar. But to avoid punishing myself with vague documentation in the future, I think that now I'll keep a notebook just to jot down specifically what I've completed and what I'm working on. For example, I was rewriting some tests while simultaneously working on a new feature. Not always good practice, but in this case, I found it necessary before moving on. Because that was the case, my tests aren't entirely helpful to me when trying to re-familiarize myself with my project. A little note in a notebook outlining why and how I was changing my tests would be greatly beneficial. 
<br><br>
In conclusion, as I continue on my self-taught journey, I go through ups and downs; lefts and rights. The important thing for me is to maintain a throughline, but sometimes the line can be cut. When that happens, I need to remain calm and committed and grant myself the patience to get myself reoriented.
<br><br>
With all of that said: `git commit -m "I'm back!"`