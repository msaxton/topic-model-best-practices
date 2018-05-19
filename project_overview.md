# Project Overview

> "All models are wrong, but some are useful" - George Box

## Aim

Topic modeling is a text mining technique in which an algorithm is applied to a large corpus of documents that identifies patterns of word co-occurrence. These patterns of word co-occurrence are conceptualized as "topics" which can be used to discover latent structures in the corpus, to group similar documents together, or to serve as the basis of information retrieval. While the application of topic modeling algorithms to a corpus is carried out by a computer, it is necessary for a researcher to make preliminary decisions about how to prepare the corpus for modeling and how to set the parameters and hyper-parameters for the model. The aim is to analyze various topic models in order to make recommendations for the best practices in building a topic model.

## Background

Since the development of latent semantic analysis (LSA) beginning in the 1980s (Deerwester et al., 1990) and the development of latent Dirichlet allocation (LDA) (Blei et al., 2003) in the 2000s, the literature on topic modeling has blossomed. Many of the studies which discuss the process and parameters of topic modeling are highly technical and inaccessible to practitioners outside of the fields of statistics and machine learning. At the same time, many of the studies authored by practitioners focus on the application of topic models without systematic exploration of how the process and parameters of topic modeling effect results of topic modeling. This project attempts to be as transparent as possible with the process and parameters of topic modeling by documenting a series experiments in building topic models in different ways from the same dataset. The results of these experiments will inform the recommendations made for best practices for topic modeling.

## Procedure

This project analyzes the properties of topic models under various conditions. It begins with a literature review which introduces topic modeling, surveys select studies which have employed topic models, and discusses the decisions that must be made when building a topic model. Following the literature review, a series of analyses are offered, each of which is a combination of code and discussion. These analyses demonstrate the ways in which a topic model changes based upon how it was constructed. Finally, the project closes with a summary discussion of the findings and suggestions for best practices for the building of topic models.

## Method

On the bases of a single dataset several topic models are constructed for the sake of comparing how the properties of a topic model change based on various parameters. The first comparison analyzes the effects a different number of topics has on a model; three models are compared:
* Model with 25 topics, alpha set to 'symmetric', based on a noun-only version of the corpus
* Model with 75 topics, alpha set to 'symmetric', based on a noun-only version of the corpus
* Model with 150 topics, alpha set to 'symmetric', based on a noun-only version of the corpus

The second comparison analyzes the effects different alpha values have on a model; three models are compared:
* Model with 75 topics, alpha set to 'symmetric', based on a noun-only version of the corpus
* Model with 75 topics, alpha set to 'auto', based on a noun-only version of the corpus
* Model with 75 topics, alpha set to '0.5', based on a noun-only version of the corpus

The third comparison analyzes the effects parts of speech have on a model; two models are compared:
* Model with 75 topics, alpha set to 'symmetric', based on a noun-only version of the corpus
* Model with 75 topics, alpha set to 'symmetric', based on a regular version of the corpus (containing all parts of speech)

In order to analyze these models, each will be tested in three different ways: a topic coherence test, a clustering test, and an information retrieval test. Each test is related to a different use case for topic models.

### Topic Coherence Test

One use case for a topic model is the analysis of a large corpus of documents. If a corpus contains hundreds, thousands, or even tens of thousands of documents, a topic model is a means of seeing the "big picture" of the corpus. In order to successfully explore a corpus in this way, a topic model must produce coherent topics. Therefore an important measure of a topic model is whether or not the topics it produces are coherent, whether that coherence is semantic or contextual. By semantic coherence, I mean that the most dominant terms in the topic all fall in the same semantic range; by contextual coherence I mean that given the context of the document (i.e. a scholarly journal of biblical literature) the dominant words in the topic are related. While this measure is subjective, it often proves more meaningful than traditional metrics of topic coherence ([Chang, et al., 2009](http://legacydirs.umiacs.umd.edu/~jbg/docs/nips2009-rtl.pdf)). Topics which do not qualify as coherent may be dismissed as "junk topics" because they do not reveal anything about the corpus.

Just because a topic is coherent, however, does not guarantee that it is useful. A coherent topic may be too general to reveal anything meaningful about the corpus. Ideally, a topic model will split topics which are too general into more specific topics. On the other hand, a coherent topic may be redundant with other topics in the model, in this case, a good topic model will merge redundant topics.

The topic coherence test in this project uses a python library called `pyLDAvis` which visualizes a topic model. This allows for an easy examination of the words in each topic. (It also displays how prominent a given topic is in the corpus and how prominent a word is in a topic.) For the purposes of this project, each topic is labeled as "coherent", "mixed", or "junk." A topic is labeled "coherent" if at least 8 of its top 10 words come from the same semantic or contextual range; a topic is labeled "mixed" if its top 10 words come from two (but not more than two) semantic or contextual ranges; finally, a topic is labeled "junk" if it does not qualify as coherent or mixed.

### Clustering Test

Another use case for a topic model is to group documents in a corpus together based on similar content. If those groups have predefined labels, this process is called classification, but if the groups do not have predefined labels this process is called "clustering." Topic models do not name the topics provided, so using a topic model for this process is an example of clustering. Therefore, a way of evaluating a topic model is to test how much of the corpus it is able to cluster into groups. For this test I define a function which identifies what percentage of the corpus documents the model is able to assign one topic, to multiple topics, or to no topics at all. The threshold I have decided to use is that a document must have a minimum threshold of 20% to be assigned to a topic. That is to say, 20% of the words used in a document must come from a particular topic if that document is to be clustered with other documents in that topic.

### Information Retrieval Test

A third use case for a topic model is information retrieval. A topic model can use a document, which it has not processed, to retrieve similar documents that it has processed. This is similar to clustering, but here similarity is not just measured by one or two topics, but rather by what probability a document has for each topic in the model. If a topic model has 25 topics, then each document has some probability value for each of those 25 topics (however small that probability may be). Therefore each document would have 25 points of comparison with every other document. Similarity between documents can be measured based on how "close" the documents are for each of the 25 topics.

For the information retrieval test I use a module from `gensim` called `similarities` to build an index based on each model. Then I supply abstracts of three resent *JBL* articles as query documents:

* [Greene, N. E. (2017). Creation, destruction, and a Psalmist's plea: rethinking the poetic structure of Psalm 74. *Journal Of Biblical Literature*, 136 (1), 85-101.](https://doi.org/10.15699/jbl.1361.2017.156672)

* [Hollenback, G. M. (2017). Who Is Doing What to Whom Revisited: Another Look at Leviticus 18:22 and 20:13. *Journal Of Biblical Literature*, 136 (3), 529-537.](https://doi.org/10.15699/jbl.1363.2017.161166)

* [Dinkler, M. B. (2017). Building Character on the Road to Emmaus: Lukan Characterization in Contemporary Literary Perspective. *Journal Of Biblical Literature*, 136(3), 687-706.](https://doi.org/10.15699/jbl.1363.2017.292918)

Each model will be evaluated on its ability to return similar documents and on the similarity score it provides for those documents.

## Data

The dataset used for this project is the *Journal of Biblical Literature* (*JBL*). This is a peer-reviewed scholarly journal which focuses on the academic study of biblical literature. This dataset provides an interesting opportunity to model a relatively homogeneous corpus of documents. JSTOR labs generously made the *JBL* available for this project in the form of a xml files containing metadata for each article and plain text files containing the optical character recognition full text for each article. The raw dataset contains 10,355 individual articles. A simple python script applied to the metadata files revealed 10,312 articles to be written in English, 40 to be written in German, and 3 whose language was not listed in the metadata. However, even the English articles contain quotations in German, Greek, and Hebrew (sometimes the latter two languages are transliterated). After eliminating articles which were not English or which contained peripheral matter such as front matter or back matter, the corpus contained 9,348 articles (which will simply be referred to as documents).