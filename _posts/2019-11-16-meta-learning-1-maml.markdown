---
layout: post
title:  "Meta Learning 1 - MAML"
date:   2019-11-16 13:47:00 -0800
---
This post is my initial mumbling on meta learning.


Meta learning is a different way to learn and do tasks. Traditionally, a task is a singleton, learning to do it well end-to-end with minimal human intervention is all we need. Meta learning takes a step back, groups many tasks, and aims to learn all of them and generalize to new tasks. By bringing in structure, meta learning probably can learn many tasks in one go. The difficulty is "few shot" - a task provides only a few examples to learn from. In Professor [Nan Lin][Professor Nan Lin's Page]'s words that I still remember, we need to borrow information.


[Professor Nan Lin's Page]: https://pages.wustl.edu/nlin