+++
title = "Implement text classification using Naïve Bayes"
subtitle = "Learn how to implement text classification from scratch using Naïve Bayes"
summary = "Learn how to implement text classification from scratch using Naïve Bayes"
date = 2019-04-20T00:00:00Z
draft = false

# Authors. Comma separated list, e.g. `["Bob Smith", "David Jones"]`.
authors = ["Tung Pham"]

# Tags and categories
# For example, use `tags = []` for no tags, or the form `tags = ["A Tag", "Another Tag"]` for one or more tags.
tags = ["Classification", "Web Application", "Flask"]
categories = []

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["deep-learning"]` references 
#   `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
# projects = ["internal-project"]

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder. 
[image]
  # Caption (optional)
  caption = ""

  # Focal point (optional)
  # Options: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight
  focal_point = "Center"
+++

In the real world, data usually divided into different categories or genres. For example, movies would have action, horror, comedy, or family as genres. However, what if we have already had all the movies manually categorized, and we want to classify a new movie? This actually can be done using very simple concept of probability, which is **Naïve Bayes**. 

Refer to our data set of TED-Talks videos on TED-Talks (<https://www.kaggle.com/rounakbanik/ted-talks>), we can calculate the **conditional** **probability** of each term in the data set’s vocabulary appear in each category. For example, we would want to know what’s the **conditional probability** of the term *“laugh”* appears in *“Funny”* videos.

The classification is implemented on top of the **TED-Recommender** *Web Application*. Therefore, all the **App Framework**, **Presentation**, and **Hosting** would be the same and can be refer at the last post: *Implement TF-IDF weight for search*(https://tungpv.com/post/how-to-search/).

**1. Preprocessing:**

Because the data set **ratings** **column** consists of many categories for one video, the first step we have to do is to pick only one category and limited the number of categories.

![img](https://tungpv.com/img/classification-ted/clip_image002.jpg)

 For research purpose, and for faster processing time, I will only choose 10 categories to consider. Therefore, for each video, I would pick the **most vote** category, and if the **ratings ID** is bigger than 10, I would eliminate it and pick the next one.

![img](https://tungpv.com/img/classification-ted/clip_image003.png)

After saving the new data set, we can perform **RegexpTokenizer**, **Stemming**, and eliminate **stop words** on the data set to construct a full vocabulary list (Refer this important step for preprocessing to last post: <https://tungpv.com/post/how-to-search/>). 

 

**2. Processing:**

The whole process for classification would be implement using the concept of **Naïve Bayes.** 

![1556005263070](https://tungpv.com/img/classification-ted/1556005263070.png)

Therefore,the mission is to construct a table to store the **conditional probability** for each term in the 

vocabulary respect to each category.

However, first, for each class c in the categories, the **prior probability** would be the number of documents in class c divide by the total number of documents.

![1556005289606](https://tungpv.com/img/classification-ted/1556005289606.png)

 Then the **conditional probability** can be calculated by the frequency of term t in documents belong to class c.

![1556005441887](https://tungpv.com/img/classification-ted/1556005441887.png)

To implemented all the calculation into our **Ted Engine**, I run a loop to compute all the **condition probability** of each term in the respected class. 



![1556005628163](https://tungpv.com/img/classification-ted/1556005628163.png)



**3. Classification:**

Whenever a query is given for **classification**, they would go through the same preprocessing of **Tokenization**, **Stemming**, and eliminate **Stop Words**. Next step, a score will be given for each class using **Naïve Bayes**. The final step is to sort the score and send it to the front-end for presentation.

![1556006009172](https://tungpv.com/img/classification-ted/1556006009172.png)

**4. Challenging:**

The classification function use a very simple concept of **Naïve Bayes**. However, because there are assumptions about the relations of the **condition probabilities**, the classification score can be incorrect. Moreover, because the vocabulary is very large, the computational time is very long, even though the categories size has already been reduced a lot. 



**<u>Reference:</u>**

TED-Talks videos on TED-Talks (<https://www.kaggle.com/rounakbanik/ted-talks>).

Text classification and Naïve Bayes (<https://nlp.stanford.edu/IR-book/pdf/13bayes.pdf>).

