---
layout: default
---

# Discussion

## The Number of Topics in a Topic Model

The number of topics assigned to a topic model has a significant effect on the properties of the topic model. On the one hand, assigning more topics to a model allows the model to group patterns of word co-occurrence in more nuanced ways. This allows for more specific, but possibly redundant topics. On the other hand, since each topic is distributed as a probability across each document (however low the probability may be), and since those probabilities must total 1.0, it follows that the more topics that are assigned, the lower, on average, those probabilities will be. How does this affect topic coherence, clustering, and information retrieval?

### Topic Coherence

The three models examined with different numbers of topics each produced approximately the same percentage of junk topics, mixed topics, and coherent topics. However, in terms of raw numbers, `model_150_topics` produced far more coherent topics than did the other models. The coherent topics from `model_150_topics` are also more specific than those provided by the other models. However, this larger number of coherent topics, and the level of specificity, comes at a cost; namely, more junk topics, potential redundancy in topics, and the difficulty of keeping track of so many topics (to say nothing of the additional computational time). So while the larger number of topics produces a larger number of coherent topics, and is therefore useful for exploratory analysis of a large corpus, a researcher may decide on a middle ground between a small number of topics and a large number of topics as a way of balancing cost and benefit.

### Clustering

`model_25_topics` was able to cluster nearly the entire corpus of the *JBL*, whereas the other two models fell far short of that goal. Of course, such clustering is only useful if the topics are coherent. Assigning a smaller number of topics to a model allows the model to cluster more documents with a topic at a higher threshold (like the 20% threshold used here) because the values in the probability distribution will be higher. Therefore, a smaller number of topics is desirable if the goal of the topic model is cluster documents.

### Information Retrieval

As far as information retrieval is concerned, the smaller the number of topics assigned to the model, the higher the similarity score will be for matching documents; but this does not necessarily mean that `model_25_topics` retrieved more relevant articles than did `model_150_topics`. Rather, since `model_25_topics`'s topics are less specific, it may return only general matches at a higher similarity score. The topics from `model_150_topics` are more specific and will therefore return matches at a lower similarity score. In other words, `model_25_topics` may fail to return matches which would be useful to the researcher. `model_75_topcis` may be a nice compromise between general matches at a higher similarity score and specific matches at a lower similarity score. On the whole, `model_75_topics`, achieved higher similarity scores than did `model_150_topics`, but it also has a higher level of nuance than `model_25_topics` by virtue of having a higher level of nuance.

## The Value of Alpha in a Topic Model

The alpha value has a significant effect on a topic model's ability to cluster documents and on its ability to retrieve information, but a negligible effect on topic coherence. The alpha value controls the distribution of topics across documents; a higher value for alpha distributes topics more widely than does a lower value for alpha. `model_alpha_symmetric` has a relatively low value (0.013) and although `model_alpha_auto` has a different value for alpha for each topic, the average alpha value is 0.06 (the highest alpha value is 0.56 and the lowest is 0.002). By contrast, `model_alpha_05` has a high alpha value of 0.5. How does this affect topic coherence, clustering, and information retrieval?

### Topic Coherence

These different alpha values have a negligible effect on topic coherence, although there is some variation regarding which words are more dominant in each topic and therefore which topics are more dominant in the corpus. `model_alpha_symmetric` and `model_alpha_auto` are the most similar regarding topic coherence because their respective alpha values were closer to one another (on average) than they were to `model_alpha_05` with its high alpha value.

### Clustering

Alpha values have a more significant effect on the model's ability to cluster the documents of the corpus. `model_alpha_symmetric` and `model_alpha_auto` do this task with more success than does `model_alpha_05`. `model_alpha_symmetric` assigned 55.2% of the corpus to one topic and 30.1% of the corpus to multiple topics, leaving only 14.5% of the corpus unassigned. `model_alpha_auto` assigned 56.8% of the corpus to one topic and 26.9% of the corpus to multiple topics, leaving only 16.3% of the corpus unassigned. By contrast, `model_alpha_05` only assigned 45.8% of the corpus to one one topic and only 5.2% of the corpus to multiple topics; this left 48.8% of the corpus unassigned. `model_alpha_05`'s inability to cluster so much of the corpus is because a wider distribution of topics across a document means that the probability values for each topic in a document will be lower and therefore fewer documents will have a probability for a given topic over the threshold of 20%.

### Information Retrieval

`model_alpha_05` was able to achieve higher similarity scores than the other two models in the information retrieval test. Higher scores here do not necessarily mean better results. However, to the degree that for the three information retrieval tasks each model returned articles that were in the right subject area (and often returned documents in common), it can be concluded that `model_alpha_05`'s higher average similarities scores are meaningful. A wider distribution of topics across a document will increase the similarities between documents because although a given topic will not have a very high probability score in a document, more topics will have at least low probability scores (as opposed to negligible probability scores). This means that the there is less difference between documents in reference to each topic in the model.

## Parts of Speech Used in a Model

Building a topic model on a noun-only corpus as opposed to a corpus with all parts of speech has a negligible effect on topic coherence, clustering, and information retrieval. There may be a few reasons that there was not a significant difference between the properties of these models. First, each model was was lemmatized and had stop words removed. This step may be more significant for preparing a corpus for modeling than filtering out non-nouns. Second, the nouns used in a scholarly article are more indicative of its content than are other parts of speech, especially adjectives, adverbs, and prepositions. An investigation of `r_model` shows that nouns tend to be the most dominant words in a given topic. Filtering out non-nouns has less of an effect as one might assume. Finally, an investigation of `n_model` shows that it does contain a number of instances of non-nouns. The `SpaCy` library used in corpus processing did not recognize and filter out all non-nouns.

### Topic Coherence

`r_model` and `n_model` each contained 76% coherent topics, furthermore, most of those topics were nearly identical in their top ten most prominent words.

### Clustering

There was also little difference between these two models regarding clustering. `r_model` assigned 58.7% of the corpus to one topic, 26.6% of the corpus to multiple topics, leaving 15.1% of the corpus unassigned. `n_model` assigned 55.2% of the corpus to one topic, 30.1% of the corpus to multiple topics, leaving 14.5% of the corpus unassigned.

### Information Retrieval

Finally, for the first two information retrieval tasks `r_model` and `n_model` achieved similar similarity scores. Only on the third task was there a significant difference with `r_model` achieving higher similarity scores and `n_model` achieving lower similarity scores. In each case, however, the models returned relevant documents.