<h1 align="center">Home Depot Product Search Relevance Prediction</h1>

<p align="center">
  <img src="https://github.com/OdedReg/Search-Relevance-Prediction/blob/main/HomeDepot.jpg" width="800">
</p>



Shoppers rely on Home Depot’s product authority to find and buy the latest products and to get timely solutions to their home improvement needs. From installing a new ceiling fan to remodeling an entire kitchen, with the click of a mouse or tap of the screen, customers expect the correct results to their queries – quickly. Speed, accuracy, and delivering a frictionless customer experience are essential.

## Overview

In this project, the goal was to predict the relevance of search results on homedepot.com. Home Depot provided a dataset containing a number of products and real customer search terms from their website. The challenge was to predict a relevance score for the provided combinations of search terms and products. The relevance scores ranged from 1 (not relevant) to 3 (highly relevant).

## Dataset Description

The dataset provided by Home Depot includes:

- **train.csv**: The training set containing products, searches, and relevance scores.
- **test.csv**: The test set containing products and searches. Predictions need to be made for these pairs.
- **product_descriptions.csv**: Text description of each product.
- **attributes.csv**: Extended information about a subset of the products.
- **sample_submission.csv**: A file showing the correct submission format.
- **relevance_instructions.docx**: Instructions provided to human raters.

## Model Architecture

The architecture implemented in this project is based on the article "Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks."<br>It's a modification of the pretrained BERT network that uses siamese network and one-shot learning structures to derive semantically meaningful sentence embeddings that can be compared using a distance metric.

<p align="left">
  <img src="https://github.com/OdedReg/Search-Relevance-Prediction/blob/main/SiameseNetwork.png" width="300">
</p>

### Key Implementations:

1. Computing the mean of all output vectors from the model.
2. Concatenating the sentence embeddings with the element-wise difference and multiplying with a trainable weight followed by softmax.
3. Using a pretrained and finetuned model for sentence embedding, specifically "all-mpnet-base-v2" from sentence-transformers.

## Approaches Explored

Four different approaches were tried:

1. **XGBRegressor**
2. **Char-Level LSTM**
3. **Word-Level LSTM**
4. **Language Model (LM)** using "all-mpnet-base-v2"

### Char-Level LSTM:

- Average search length: 18
- Average text length: 960
- Achieved better RMSE and MAE results compared to XGBoost model.
- Generalizes well for scores around the median but struggles towards the extremes of the relevance scale.

### Word-Level LSTM:

- Average search length: 3
- Average text length: 163
- Slightly better MAE results than char-level model with similar RMSE.
- Generalizes well for scores around the median but struggles towards the extremes of the relevance scale.

### Language Model (LM):

- Implemented using "all-mpnet-base-v2" from sentence-transformers.
- Dropout proved effective in preventing overfitting.
- Significantly outperformed XGBoost and LSTM models with improvements in both RMSE and MAE.
- Predicted values exhibited closer proximity to the true values.

## Conclusion

The Language Model approach using "all-mpnet-base-v2" demonstrated superior performance compared to other methods, showcasing its effectiveness in predicting the relevance of search results on Home Depot's website. The implementation successfully addressed the challenges posed by the task, contributing to an improved shopping experience for Home Depot customers.

Char-Level LSTM achieved better RMSE and MAE results comapred to the xgboost model. the model does not predict values lower than 1.59, which is problematic for low scores. Additionally, it generalizes well for scores around the median (2.38), but makes more mistakes towards the extremes of the relevance scale.

Word Level LSTM has slightly better MAE results than char-level model, while achieving the same rmse. We observe that the model does not predict values lower than 1.48, which is an improvement from the char-level model, but is still problematic for low scores. Additionally, it generalizes well for scores around the median (2.38), but makes more mistakes towards the extremes of the relevance scale.
