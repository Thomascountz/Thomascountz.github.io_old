---
layout: post
title: View a File without Checking Out
date: 2017-04-26 18:02:00
description: Quickly View A File on a separate Branch Without Checking Out
published: true
---
To view a file in a separate branch without using `git checkout` to update the file in your working tree, use `git show` instead:

```
git show <branchname>:<filepath>
```

You can also specify your text editor using the pipe character:
```
git show <branchname>:<filepath> | <texteditor>
```

For example:
```
git show upstream/development:app/models/post.rb | subl
```