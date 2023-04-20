---
marp: true
title: F1 doesn't matter (and other stories from the industry)
description: IN5550 2023 Guest lecture
theme: lead
paginate: true
size: 16:9
---
<!-- _class: invert -->
<!-- _paginate: false -->
# F1 doesn't matter
## 👨‍🚀 ... and other stories from **the industry**
> **Murhaf Fares** (he/him)
> **Emanuele Lapponi** (he/him)
> Machine intelligence @Fremtind

![bg right](figures/industry.gif)

---
## 🤔 Who are you?

- LTG PhD class of 2019
- Worked on NLP most of our professional lives
- Mostly together 🤗

---
## 🤔 What's a Fremtind

- One of Norway's leading insurance companies, owned by SB1 and DNB
- Data is at the core of most insurance processes

![bg right 60%](figures/fremtind-logo.png)

---
## 📝 Coming up in this talk

```
~/talks/f1-doesnt-matter  main ✔                                                   0m
▶ cat slides.md | grep "^# \*\*"
# **Quick NLP hacks can have a big impact**
# **Visualize all the things**
# **On customer satisfaction and feedback**
# **A simple model is enough**
# **Make the most of small data**
# **F1 doesn't matter**
# **Budget UI is better than no UI**
# **Model management is hard**
# **The world beyond NLP and deep learning**
# **Being cool always beats raw technical prowess**
```

---
<!-- _class: invert -->
# **Quick NLP hacks can have a big impact**
##### <!-- fit --> 🦟 Or: Don't kill a mosquito with a bazooka

<!-- 
This could be a "warm up"-story, short, and gives an example of something one has to wrestle with in the industry, before going into the Cura-world
-->

![bg right](figures/mosquito.gif)

---

**Task**: Given a "tell us what happened"-description, find out whether the travel destination is in Norway, Europe, or in the rest of the world

<!-- 
This one is interactive: how would you go about solving this problem?
-->

![bg right 90%](figures/destination.png)

---

**What worked:**
- Some off-the-shelf preprocessing with Spacy 💝
- Fuzzy matching against a knowledge-base of places

###### **~2000 claims automated 💰**

![bg right 90%](figures/destination.png)

---
<!-- _class: invert -->
# **Visualize all the things**
🗺️ **Seeing is believing**


![bg right](figures/scatter.gif)

---

- We work with a bunch of different datasets
- Some neural representations might be better than other
- Some might be cheaper
- What's a representation anyway? It's important to "build trust" across teams and customers. "Seeing is believing"

# <!-- fit -->🎨 One thing we do all the time: Embed, reduce, scatter

---

```python
import altair as alt
from umap import UMAP

two_d = UMAP().fit_transform(df["embeddings"])
df["x"], df["y"] = two_d[:, 0], two_d[:, 1]

alt.Chart(df).mark_circle(size=60).encode(
    x='x',
    y='y',
    tooltip=["text"]
).properties(width=1000, height=500).interactive()

```

![bg right](figures/scatterplot.gif)

---
<!-- _class: invert -->
# **On customer satisfaction and feedback**
# <!-- fit -->  😱 Or: Not all bad feedback is interesting
<!-- joint work -->
<!-- 
Here we just introduce the task/problem and its nuances
-->

![bg right 120%](figures/customer.gif)

---

**Customer surveys** are an important tool to improve processes in most product companies. At Fremtind, customers have the opportunity to give us written feedbacks at different steps of their journey: as they **purchase** new insurances, **update/upgrade/review** their coverage, and, perhaps most interestingly for our business, **after a claim has been approved or rejected** (some 50K a year.)

![bg left](figures/pitchfork.png)

---

"Vanilla" sentiment analysis is not necessarily useful to analyze and extract value from these written messages: Most feedbacks come pre-equipped with a 🎲 or a 👍/👎.

At the same time, these user generated scoring systems are useful to take the general temperature of customer satisfaction, but fall short at providing a ranking of feedbacks that are relevant to address different business problems.

# <!-- fit --> So how can we use customer feedback to improve customer satisfaction?

---

## Facts:
There are 3 key drivers for CS in insurance: (1) being paid, (2) being paid quickly, (3) clarity and respect in the claim process.

For (1), there is just not much we can do to improve: the damage is either covered or it isn't. For (2), well, we know. We either hire more temps in dire times, or some people are just going to have to be patient.

#### For **(3)**, we decided that absolutely all customers should be met with clear language and a high level of empathy and understanding.

##### **The degrees to which a given message is about unclear language, or whether the customer was met with impatience and/or antagonism become interesting dimensions.**

---

# Feedback Loops

In this project, we score customer communication from a variety of sources with the degree to which they pertain different (relevant) aspects of communication. This enables our (hundreds of) colleagues to **read more relevant messages** and use **text scores as quantities for their KPIs**.

![bg left](figures/dimensions.jpg)

---
<!-- _class: invert -->
# **A simple model is enough**
😎 **Or: how I learned how to stop worrying and just throw some pretrained embeddings at a dense layer**
<!-- joint work -->
<!-- 
Show how the models are built, and how it processes the data every day, maybe give a high-level idea of how AWS is set up
-->

![bg right](figures/natural.gif)

---

- Each of the aforementioned dimensions translates to a simple text classification problem 
- Supervised learning: a training set of pairs (text, relevant/irrelevant)
- We train a simple feedforward neural network with input representation from sentence transformers (e.g., Bert, Universal Sentence Encoders, etc...)

![bg right](figures/ff-network.jpg)

---

### <!-- fit --> **Topic classification for customer feedbacks**

- Find extra (textual) features such as the URL of the page on which the customer wrote their feedback
- Preprocess the URL and embed it
![bg left](figures/two-inputs.jpg)

---

### **Transfer learning**
- We have different models for different teams
- Each team define their own categories, but those overlap
- This can be exploited by simply initializing the model weights in model B using weights from model A
![bg right](figures/transfer.jpg)

---
## <!-- fit --> **Building the model is only one piece of the puzzle, though**
- Text preprocessing is a must regardless of how good your embeddings are
- For example, anonymization: no consumer data should leak into training data
## Look at your data, again and again 🧐
→ Don't sleep on embed/reduce/scatter!

---

#  **Serving the models**

- REST API in a microservice 
- Scheduled tasks: fetch the data every day/week/month, score it and write it back

### <!-- fit --> **More about this in _The world beyond_**
![bg left](figures/cloud.gif)




---
<!-- _class: invert -->
# **Make the most of small data**
🤬 **Or: nobody is going to like you if you constantly ask people to annotate data for you**
<!-- joint work -->
<!-- 
On weak supervision
-->

![bg right](figures/little-bit.gif)

---

### **We need classification models for clarity and impudence**
... but we didn't have a lot of data

Instead of asking the domain experts to annotate more data, can we pick their brains about how they would annotate the data and use that expertise somehow?

---

#### _Thank god for_
### <!--fit --> **Weak Supervision!**
- (Re)use existing sentiment dataset to create new datasets for clarity and impudence
- Translate their expertise to labelling functions to produce so-called silver data $(𝑋,\widetilde Y)$

![bg right](figures/ws.jpg)

--- 
🤿
Such functions can be as simple as:

```python
def lf_ short_message_positive(x):
    if len(x.doc) < 5 and ×. sentiment_ score < 0.4:
        return 0
    return -1
def lf_contains_one_strong_and_negative(x):
    if x. sentiment_score >= 0.5 and contains_word(x. doc, ASSETS ["'UNCLARITY"] ["strong"]) :
        return 1
    return -1
def lf_six_and_positive(x):
    if x. sentiment score < 0.5 and ×[dice_ col_unc] == 6:
        return 0
    return -1
```

<!-- 
- if the text includes a word that strongly indicate unclarity 
- the sentiment score of the text
- the dice score 
- message length
-->
The outputs of these labels functions are then combined either through an ML model or a simply majority vote



---
<!-- _class: invert -->
# **F1 doesn't matter**
💖 **Or: does it spark joy?**
<!-- joint work -->
<!-- 
I dont remember what we talked about
-->

![bg right](figures/helpful.gif)

---

- F1 _does_ matter, but it is not the only metric we consider
- What is a _good_ model? 
- What is good enough when we are limited by time and resources?

---
<!-- The figures show the distribution of the scores of two models on a binary 
classification problem; the two models have the exact same F1 score on the test
set, which model is better?   -->

![bg 90%](figures/nofocal.png)
![bg 90%](figures/focal.png)

<!--When you need a ranked list, the model to the left is almost of no use
In practical terms, the only difference between these two models is the loss
function; the one to the left uses binary cross-entropy whereas the the one to
the right uses focal cross-entropy 
(Focal loss applies a "focal factor to down-weight easy examples and focus more on hard examples)"-->

---

### <!-- fit --> Qualitative analysis is as important as quantitative scores
 <!--Back in the "olden" days, we were using a recurrent neural network to classify 
 feedback messages into positive and negative labels; the model was quite good
 but it was clear that the model was using the message length indirectly as a 
 feature (because of fixed-length sequences and padding) -->
- Short feedback messages tend to be positive, whereas longer ones fall more on the negative side
- A recurrent neural network could learn that
- Would the model output still be interesting? Depends on how it will be used 
## **Think about the consumers of the model output** 🛍️

---
#### <!-- fit --> Friends don't let friends obsess on hyperparameter tuning
<!-- another way in which F1 doesn't matter if it costs a lot to get 
obsessed with pushing it further-->
- Simplicity, efficiency vs improving F1 by 0.1 
- no point in eg spending a lot of resources and time on fine-tuning a bert, if vanilla works well enough
- hp tuning is a waste of time if you don't like what comes out of the model

#### <!-- fit --> **Simple/fast/not clunky is almost always more valuable than a few percentage points** 💰


---
## Tradeoffs of the trade 
- Precision and/or recall, at thresholds!
- FBeta
- Sometimes a bad model is better than no model at all 
- Can we tweak the system implementation so that false positives don't matter as much?

---
<!-- _class: invert -->
# **Budget UI is better than no UI**
🎨 **Or: unleash your inner frontend dev**

<!-- 
streamlit
-->

![bg right](figures/art.gif)

---

🔥 **Hot take**

# <!-- fit --> An interactive demo is worth more than a better model, 100 excel sheets or 1000 reports

`https://streamlit.io/`

→ cmd-tab to Cura

---
<!-- _class: invert -->
# **Model management is hard**
🎨 **Don't do it (yourself)**

<!-- 
mlflow
-->

![bg right](figures/findyou.gif)

---
Long story short: you gotta keep track of models, data, model versions, val/test splits, evaluation plots, comments, and more.

# <!-- fit --> You are **not** going to be able to do it (well) for yourself, let alone a whole team

We use `https://github.com/mlflow/mlflow/`

→ cmd-tab to ml-flow

---
<!-- _class: invert -->
# **The world beyond NLP and deep learning**
🚪 **Tabular data **


![bg right](figures/stranger.gif)


---

- lightgbm, pandas profiler, pandas_dq
- fraud
- churn 
- databases 


---
<!-- _class: invert -->
# **Being cool always beats raw technical prowess**
🤝 **Or: be a bud**


![bg right](figures/better.gif)

---

### Some things we have seen
- Dropping model names like you are swapping pokemon cards
- Gatekeeping and arrogance
- Toxic code reviews
- Now that every linkedin influencer is a chatGPT expert, it's easy to forget that NLP is hard.

##### **Empathy and willingness to learn and share make more ML money than hard skills**

![bg left](figures/check.gif)



