---
layout: post
title: "Open localhost from shell"
date: 2013-07-28 21:01
comments: true
published: true
categories: [ruby]
---

Add a few aliases to your .zshrc to quickly open localhost.

```
alias 3000="open http://localhost:3000"
alias 8000="open http://localhost:8000"
alias 8080="open http://localhost:8080"
alias 8082="open http://localhost:8082"
alias 9000="open http://localhost:9000"
```

If you use the fish shell I submitted a plugin to oh-my-fish to do just this.
