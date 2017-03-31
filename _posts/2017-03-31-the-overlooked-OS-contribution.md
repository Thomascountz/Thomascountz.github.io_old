---
layout: post
title: The Almost Overlooked Open Source Contribution
date: 2017-03-31 16:38:00
description: We all hope to get a merged PR, but research has value too. 
published: true
---

<a href=" " target="_blank"> </a>

I was lucky enough to attend a meetup in NYC called <a href="https://www.meetup.com/hackerhours/" target="_blank">HackerHours</a>. They run many weekly meetups for any developer, at any level, using any language, to have a time and space (and free wifi) to fill a room with support and inspiration. However, this particular meetup had a topic, contributing to open source.

<br>
A developer from <a href="https://thoughtbot.com/" target="_blank">Thoughtbot</a> was in attendance (...and I nearly pee'd myself with fangirl excitement...), and along with him, he brought some issues from Thoughtbot's many open source projects that he thought might be fun for us to tackle. <a href="https://github.com/thoughtbot/factory_girl" target="_blank">Factory Girl</a>, <a href="https://github.com/thoughtbot/paperclip" target="_blank">Paperclip</a>, <a href="https://github.com/thoughtbot/croutons" target="_blank">Croutons</a>, and <a href="https://github.com/thoughtbot/high_voltage" target="_blank">High Voltage</a> were among the apps that he suggested we take a look at. Being familiar with Factory Girl, my pair and I chose <a href="https://github.com/thoughtbot/factory_girl/issues/992" target="_blank">Issue #992</a>.
<br>
<br>
<div class="img_row">
	<img class="col three" src="/img/factory_girl_issue_992.png">
</div>
<div class="col three caption">
	Thoughtbot's Factory Girl, Issue #992 
</div>
<br>
<br>
So my pair and I went off, forked the repo, found where the <code>ArgumentError</code> was being raised, and replicated the issue. We felt so cool! Here we were working on a contribution for Thoughtbot! We ended up finding the <a href="https://github.com/yuki24/did_you_mean" target="_blank">did_you_mean</a> gem, which we discovered was added to Ruby Core as of 2.3, and we tried to get things to work with Factory Girl. However, we weren't able to get it to work. And after nearly wiping out our dev environment with <code>RVM</code> issues, we felt defeated, but that's when our expert friend from Thoughtbot came in and offered us his wisdom:
<br>
<br>
<blockquote>
Keep digging, but if you're unable to find a solution, it's perfectly valid to comment on the issue and say, 'we've looked into this, here is what we tried, but it didn't work.' Either we find out that this isn't a feature that we can't easily implement, or someone else can come along and pick up where you left off.
</blockquote>
<br>
<br>
We felt invigorated and relieved. The two and a half hours we spent researching were not in vain. We were working on a Thoughtbot project, pull request or not. We submitted what we had learned that day:
<br>
<br>
<div class="img_row">
	<img class="col three" src="/img/factory_girl_comment.png">
</div>
<div class="col three caption">
	Our Factory Girl Contribution 
</div>
<br>
<br>
And that was it. Our research was valuable. The research stage is an often overlooked step in the agile process, and it's all to easy to not consider its benefits. You don't get a Merged PR credit in your list of contributions and it's not exactly something you'd include on your resume, but you're giving back to the open source community all the same. And not only that, my pair and I learned quite a bit: about Factory Girl, did_you_mean, RVM, and pairing with a senior developer at our dream company.
<br>
<br>
Thanks for reading, and happy coding!