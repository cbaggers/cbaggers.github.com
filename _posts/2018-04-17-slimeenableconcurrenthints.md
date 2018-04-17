---
layout: post
title: slime-enable-concurrent-hints
description:
date: 2018-04-17 19:20:10
category:
tags: ['lisp', 'cepl']
commentIssueId:
---

This is just me dumping something I keep forgetting. When you are live coding you often block the repl, you can call swanks `handle-requests` function to keep the repl which works but the minibuffer likely wont be showing function signatures (or other hints) any more. To enable this add this to your `.emacs` file and call using `M-x slime-enable-concurrent-hints`

```
(defun slime-enable-concurrent-hints ()
  (interactive)
  (setf slime-inhibit-pipelining nil))
```
