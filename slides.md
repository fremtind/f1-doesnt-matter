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
- Something something

![bg right 60%](figures/fremtind-logo.png)

---
## 📝 Coming up in this talk

Talk outline

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
# **Embed, reduce, scatter**
🗺️ **something something**


![bg right](figures/scatter.gif)

---

- scatter example

---
<!-- _class: invert -->
# **On customer satisfaction and feedback**
# <!-- fit -->  😱 Or: Not all bad feedback is interesting
<!-- joint work -->
<!-- 
Here we just introduce the task/problem and its nuances
-->

![bg right](figures/customer.gif)

---

- examples of uninteresting feedbacks with low terningkast and the converse
- explain what we are interested in KPI-wise
- 3 drivers: money, speed and communication
- do something about communication

---

- reflect on the interesting nuances of communication aspects: this is more complicated than vanilla sentiment analysis
- throw in product relevance too why not

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

```python
def sbert_embed(texts):
    clean = [clean_text(text) for text in texts]
    return ASSETS["SBERT"].encode(clean, device=DEVICE, show_progress_bar=False)
```

---

- Something about how preprocessing/best effort anonymization is enough

---

something about the cloud setup; reflect on full-stack ML work 

---

something about the temakategorisering, two inputs (sted), starting with pre-initialized weights from similar models

---
<!-- _class: invert -->
# **Make the most of small data**
🤬 **Or: nobody is going to like you if you constantly ask people to annotate data for you**
<!-- joint work -->
<!-- 
On weak supervision
-->

![bg right](figures/small.gif)

---

- ... but we didnt have a lot of data
- show what we did with weak supervision for clarity and impudence


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

- no point in eg spending a lot of resources and time on fine-tuning a bert, if vanilla works well enough

- hp tuning is a waste of time if you don't like what comes out of the model

- simple/fast/not clunky is almost always more valuable than a few percentage points

---

The "feel" of the delivered model is more important -- eg sometimes overfitting on shorter/longer messages might give better (F1) and more pedantically/technically better results, but the colleagues on the receiving end might want to see variety

---
- The tradeoffs between P and R are often essential, something something FBeta
- Often a bad model is better than no model at all (for example if false positives don't matter, think few shot object class with marius)

---
<!-- _class: invert -->
# **Budget UI is better than no UI**
🎨 **Or: unleash your inner frontend dev**

<!-- 
streamlit
-->

![bg right](figures/art.gif)

---
- mention streamlit & gradio, recommend for portfolio building
- alt-tab to a Cura demo

---
<!-- _class: invert -->
# **Model management is hard**
🎨 **Don't do it (yourself)**

<!-- 
mlflow
-->

![bg right](figures/findyou.gif)

---

- You end up with a bunch of models, evaluation residuals, test sets and so on
- show mlflow


---
<!-- _class: invert -->
# **The world beyond NLP and deep learning**
🚪 **something tabular data**


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

- reflections on the fact that an incredible engineer who is toxic can ruin a team
- anectodes on Taraka and that JP guy at Kindly
- NLP is hard, and more people without an NLP background like you want to do NLP work. There is a lot of value in spreading/teaching NLP in the industry. We try to do it like this and that
