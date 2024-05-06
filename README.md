<h1 align="center">Search Relevance Prediction</h1>

<h2 align="center">
An NLP project utilizing Language Models, Char-Level and Word-Level LSTM <br>
for improved relevance prediction
</h2>

<p align="center">
  <img src="https://github.com/OdedReg/Search-Relevance-Prediction/blob/main/HomeDepot.jpg" width="800">
</p>


create for me a readme file for github repository of a project that i have done based on the following information:
Home Depot Product Search Relevance
Predict the relevance of search results on homedepot.com
Shoppers rely on Home Depot’s product authority to find and buy the latest products and to get timely solutions to their home improvement needs. From installing a new ceiling fan to remodeling an entire kitchen, with the click of a mouse or tap of the screen, customers expect the correct results to their queries – quickly. Speed, accuracy and delivering a frictionless customer experience are essential.

In this competition, Home Depot is asking Kagglers to help them improve their customers' shopping experience by developing a model that can accurately predict the relevance of search results.

Search relevancy is an implicit measure Home Depot uses to gauge how quickly they can get customers to the right products. Currently, human raters evaluate the impact of potential changes to their search algorithms, which is a slow and subjective process. By removing or minimizing human input in search relevance evaluation, Home Depot hopes to increase the number of iterations their team can perform on the current search algorithms.

Dataset Description
This data set contains a number of products and real customer search terms from Home Depot's website. The challenge is to predict a relevance score for the provided combinations of search terms and products. To create the ground truth labels, Home Depot has crowdsourced the search/product pairs to multiple human raters.

The relevance is a number between 1 (not relevant) to 3 (highly relevant). For example, a search for "AA battery" would be considered highly relevant to a pack of size AA batteries (relevance = 3), mildly relevant to a cordless drill battery (relevance = 2), and not relevant to a snow shovel (relevance = 1).

Each pair was evaluated by at least three human raters. The provided relevance scores are the average value of the ratings. There are three additional things to know about the ratings:

The specific instructions given to the raters is provided in relevance_instructions.docx.
Raters did not have access to the attributes.
Raters had access to product images, while the competition does not include images.
Your task is to predict the relevance for each pair listed in the test set. Note that the test set contains both seen and unseen search terms.

File descriptions
train.csv - the training set, contains products, searches, and relevance scores
test.csv - the test set, contains products and searches. You must predict the relevance for these pairs.
product_descriptions.csv - contains a text description of each product. You may join this table to the training or test set via the product_uid.
attributes.csv -  provides extended information about a subset of the products (typically representing detailed technical specifications). Not every product will have attributes.
sample_submission.csv - a file showing the correct submission format
relevance_instructions.docx - the instructions provided to human raters

Data fields
id - a unique Id field which represents a (search_term, product_uid) pair
product_uid - an id for the products
product_title - the product title
product_description - the text description of the product (may contain HTML content)
search_term - the search query
relevance - the average of the relevance ratings for a given id
name - an attribute name
value - the attribute's value

evaluation was on the root mean squared error (RMSE). another metric examined is MAE.

Number of rows in train: 74067
Number of rows in test: 112067
There are 11795 unique searches in Train
There are 20986 unique searches in Test


In this project, I implemented the architecutre in the article:
Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks - 
a modification of the pretrained BERT network that use siamese network and one-shot learning structures to derive semantically meaningful sentence embeddings that can be compared using a distance metric.

I implemented the major keys from this article that got the best results:
1. computing the mean of all output vectors from the model 
2. The sentence embeddings u and v are concatenated with the element-wise difference |u-v| and multiplied with the trainable weight Wt, and softmax
3. Using a preterained and finetuned model for sentence embedding. BERT-NLI-STSb-base noted in the article as the best model, but since This model is deprecated, i chose the best model from  sentence-transformers which is all-mpnet-base-v2.

i tried 4 different approaches:
1. XGBRegressor
2. Char-Level LSTM
3. Word-Level LSTM
4. LM (sentence-transformers/all-mpnet-base-v2)

Char-level approach:
Average search length: 18
Average text length: 960

Word-Level approach:
Average search length: 3
Average text length: 163

The predictions generated by the XGB model closely align with the mean relevance. The predicted relevance exhibits minimal variance (0.03 compared to 0.53), indicating that the model predominantly forecasts values in close proximity to the mean. This suggests that the model's performance is suboptimal.

Char-Level LSTM achieved better RMSE and MAE results comapred to the xgboost model. the model does not predict values lower than 1.59, which is problematic for low scores. Additionally, it generalizes well for scores around the median (2.38), but makes more mistakes towards the extremes of the relevance scale.

Word Level LSTM has slightly better MAE results than char-level model, while achieving the same rmse. We observe that the model does not predict values lower than 1.48, which is an improvement from the char-level model, but is still problematic for low scores. Additionally, it generalizes well for scores around the median (2.38), but makes more mistakes towards the extremes of the relevance scale.
