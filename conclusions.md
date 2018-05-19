# Conclusions: Recommendations for the Best Practices in Building a Topic Model

## Decide on the Use Case for the Topic Model

Topic models can serve many purposes. In this project I have indicated three: (1) the analysis of a large corpus of documents; (2) grouping documents based on similar content; and (3) information retrieval. The task of the topic model should dictate they way the topic model is built, different parameters will make a topic model more, or less, suited for a task.

## Process the Corpus Carefully

A topic model is only as good as the corpus upon which it is built. The most important part of of preparing a corpus is trying to select the most informative features and leaving aside those which are not informative. After stop words are removed and the remaining words are lemmatized, it is still necessary to filter out words which occur to frequently or too infrequently. Beyond that, I have demonstrated that although nouns are highly informative of a documents content, it is not necessary to filter out other parts of speech. A topic model built on a noun-only corpus is no more useful than  model built on all parts of speech.

## Experiment with Topic Numbers

There is no rule than can be formulated for choosing the right amount of topics for a topic model. The literature reviewed here shows the importance of trying models with different numbers of topics. I also demonstrated here that the number of topics chosen should depend on the task the topic model will accomplish. A smaller amount of topics may be more helpful for grouping documents because fewer documents will be left without topic assignments. On the other hand, if one is using a topic model for the analysis of a large corpus, one needs to try many models with different topic numbers to find the model whose topics are specific without becoming redundant. This process will take some trial and error, but it is necessary for building a model which can be used to makes sense of a large corpus. Balance must also be found for building a topic model for information retrieval. Here a model with too few topics will return matches with high similarity but those matches may be only vaguely related to the query document. A model with too many topics will return matches at a lower similarity score but the matches may be very nuanced. Again, a middle ground must be achieved between high similarity scores with overly general matches and low similarity scores with nuanced matches.

## Pay Attention to the Alpha Value

The alpha value is important because it controls the distribution of topics over the documents. Although I have demonstrated that the alpha value does not have a significant effect on topic coherence, it is nonetheless important for other properties of a topic model. If the goal of the model is to group documents in meaningful ways, a lower value for alpha is recommended. A lower value for alpha allows a topic to have a higher probability association with a given document and therefore it is easier to group documents by topics in the model. However, if the goal of the model is information retrieval, a higher value for alpha may be of use. The model tested here with a high alpha value returned relevant documents at high similarity scores for information retrieval. 

