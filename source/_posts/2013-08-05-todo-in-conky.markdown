---
layout: post
title: "Display TODO list in Conky"
date: 2013-07-28 21:01
comments: true
published: true
categories: [linux, openbox]
---

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
