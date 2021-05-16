---
mathjax: true
layout:  post
title:   "Modeling Multidomain Task Oriented Chatbot E2E as a Sequence Model"
date:    2021-05-02 21:28:00 -0800
---

Task oriented chatbot is designed to assist users to finish tasks as efficiently as possible. Unlike open domain chatbot, we can target a handful of domains, and a fixed list of tasks or task templates. So we can set up the stage in the beginning.

With the domains and tasks in mind, it is time to collect and curate data. Multi-Domain Wizard-of-Oz dataset ([MultiWOZ](https://arxiv.org/abs/1810.00278)) showed an example. In MultiWOZ, there are 7 domains: restaurant, hotel, attraction, taxi, train, hospital, police. And a task example looks like: You wanna book a train to Cambridge on Friday, book a hotel in Kendall Square, and look for attractions nearby. MultiWOZ uses human-to-human dialogues to collect data, as well as human to annotate every turn in the collected dialogues.

The human-to-human dialogues are done in a Wizard-of-Oz fashion, and are focused on completing the tasks given to user. The paper showed the interface from the wizard side and user side. Wizard can access database to retrieve values and use them to fill or compose dialogue. On the annotation side, at every turn, a human annotator annotate the belief states after user's utterance, and the action after wizard's utterance. A belief state is a list of 3-tuples (domain, slot name, value). For example, (restaurant, price, moderate). A action is a list of 3-tuple (domain, act, slot name). For example, (restaurant, request, location). As another example, (restaurant, offer to book, _). Task oriented dialogue system is often defined by an ontology, a structured representation of the back-end database. The ontology defines all entity attributes called slots and all possible values for each slot. Based on a given ontology spanning several domains, a task template was created for each task through random sampling. This results in single and multi-domain dialogue scenarios and domain specific constraints were generated.

With the stage setup, at each turn $t$ of the wizard or system (S), all previous turns are the context $$C_t = [U_0, S_0, ..., U_t]$$. Then belief states are generated $$B_t = f(C_t)$$. Belief states are used to query database to get values $$D_t = Retrieve(B_t)$$. Then action is generated $$A_t = f(C_t, B_t, D_t)$$, and the delixicalized response is generated $$S_t = f(C_t, B_t, D_t, A_t)$$. A single training datapoint consists of the concatenation $$x^t = [C_t, B_t, D_t, A_t, S_t]$$. Training is done with transfer learning, in the sense that the tokenization of a previous work and their embeddings are used. The training model architecture is a seq-2-seq Transformer where the output sequence is the input sequence shifted to the right by 1 token.

[A Simple Language Model for Task-Oriented Dialogue](https://arxiv.org/abs/2005.00796)