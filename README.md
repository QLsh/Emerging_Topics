# Emerging_Topics

## Stating problem
Contextual targeting is a privacy-preserving way to serve ads on the internet. One of the biggest drawbacks is that on average the relevance of the ad to the internet user is not ideal because it only serve ads related to the content of that website. Therefore, to reduce the drawback of contextual targeting, we need to add more information to it for better decisioning power.


## Methodology
### Dataset
The data set we used are from open Common Crawl News Corpus, a large, open-source web crawl over news websites and combining it with our internal, proprietary data sources.

### Data pre-processing
Before feeding any text to the NLP models, data pre-processing is usually a very crucial step.

### Main procedure
The initial approach taken in this project is comparing the performance of a set of models. A series of static natural language process model was chosen for the problem. The LDA model is used as the baseline model. Some of its variants like DMRModel and PTModel are also applied to a subset of data to compare their performance with the baseline. BERTopic model is then introduced as a representation of embedding topic model. 

### Model evaluation
The models are evaluated by two chosen measurable metrics: topic coherence and topic diversity. Reason why we chose this is because they are achievable in a shorter period. The definition of these two metrics is shown as followed:

After choosing the better model based on the overall scores of the model quality, we then try to develop the static model into a dynamic model. By assigning all the topics back to the URLs for the news corpus, we can aggregate the traffics over 7 days for each topic so that topics with commercial values can be identified in this way.
  
## Specific problem-solving
    1. Poor data pre-processing: initially the data pre-processing is not working as expected because of the order we set it up. Lemmatization from `nltk` package remove ‘s’ at the end of all words without difference. So, in the next step when we were trying to remove the common words, some of the common words like ‘was’ was lemmatized as ‘wa’ and hence cannot be removed. The solution we used was simply changed the order of these two steps and the data-cleaning is much more efficient.
    2. Difficult to calculate the quantification metrics: packages we used to calculate topic coherence is called `gensim`. When we tried to calculate the coherence score, some of the topics does not have available coherence score so we must calculate the coherence score per topic instead and write additional code to take the average to get the total coherence score.
    3. Run model at scale: the other problem we encountered was to apply BERTopic model at scale. Because the BERTopic model runs over the whole dataset, it is difficult to distribute the work to different nodes in the clusters. However, the whole dataset is so huge that it took ages to run without distributing. The approach of solving this is sampling the dataset so that we were able to extract a set of topics for further study.
What results did we get?

## Evaluation of static models
After the initial exploration of the static models, we evaluated the LDA topic model and BERTopic model with the two measurable metrics. The results can be shown as followed:

![image](https://user-images.githubusercontent.com/90244300/220976105-b3104115-6088-4ec8-b590-95476782477a.png)
![image](https://user-images.githubusercontent.com/90244300/220976139-13d0110c-bca6-42e4-83fc-1259bb14fe1f.png)

BERTopic model performs better in the topic diversity. When the vocabulary of the documents grows, it performs even better. Meanwhile, the result for topic coherence metric shows that topics extracted from LDA model has better coherence compared to BERTopic model. This might be caused by the number of key words of BERTopic is from 1 to 3. It can reduce the coherence by having repeated keywords.

As it was stated in the previous section, the model quality is defined as the product of two metrics. Similar bar chart is presented in the following figure:

![image](https://user-images.githubusercontent.com/90244300/220976213-d873574b-3d54-4335-bf9d-9a8807e363e6.png)

It is clearly shown that the overall score of BERTopic model is always higher than LDA topic model, which means that BERTopic model is overperforming LDA model no matter how many vocabularies are used as the inputs. 

## Topics over time
By evaluating the static models, we chose the best performance static model and develop it into a dynamic model. This way we were not only able to extract topics from the corpus but also with the time stamp for each topic and its subtopics. The first few topics are shown in the tale below:

![line](https://user-images.githubusercontent.com/90244300/220979038-ed87a5c0-f7b9-48e6-b751-37d23d631d98.png)

As the topics are ranked by frequencies of the topics. The first new topics with topics id -1 is considered as common words topics. In this case, the model still needs modifications because the queen’s death is also considered as noise in this case.

The visualization of some representative topics over time is shown in the figure below:

![table](https://user-images.githubusercontent.com/90244300/220979175-1cb9fc01-3329-4769-bc30-17db62f97070.png)

Each peak can also represent a subtopic of the general topic. We can use this graph to interpret the growth and death of a topic over time and analyse the feature of different type of topics.

## Impacts
By finishing this project, we managed to extract topics that we can used for advertising from 3TB of news corpus. With all the topics over time we extracted from the corpus, we built a prototype of advertising product. This outcome also contributed to the Trade Desk’s privacy-preserving set of tools.
