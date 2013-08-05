---
layout: post
title: "Display TODO list in Conky"
date: 2013-08-28 21:01
comments: true
published: true
categories: [linux, openbox]
---

Show a TODO, or other text file, in Conky.

<!--more-->

![desktop screenshot](/images/conky.png)

```bash
vi ~/.conkyrc
```

Add the following lines under the `TEXT` section:

```bash
TODO
${hr}
${execi 3000 cat ~/TODO}
```

`execi` executes the command with an interval of 300ms.

My full conkyrc can be [found
here](https://github.com/krisleech/dotfiles/blob/master/desktop/conkyrc).
