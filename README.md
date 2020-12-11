# MeliDataChallenge2020

## Winner model for Mercado Libre Data Challenge 2020

This solution is called Efecto Bolo! and in honour of this lovely name, it was the first place of the [final leaderboard](https://ml-challenge.mercadolibre.com/final_results)!

## The challenge

>  "Build a Machine Learning model to predict next purchase based on the user’s navigation history."
>
> https://ml-challenge.mercadolibre.com/


## Data
The dataset provided for this competition contains users' navigation histories (list of visited items and searches before the purchase), and the purchased item (target).

The training set contains 413.163 rows, and the test set used for submissions has 177.070 rows.

Additionally, there is a dataset with the catalog of all available items for making predictions with the catalog of all available items for making predictions (2M items). It contains some extra information about the products, such as the price, the listing's title, and categories.

> https://ml-challenge.mercadolibre.com/downloads

## Evaluation

Submissions were evaluated using the average NDCG score of the top ten predicted items. This metric takes into account the order of the set of recommendations, assigns a relevance score if we hit the target purchase, and also a lower score for those predicted items with the same domains (it's like a category) of the target. The latter means that the solution could focus on item domains. There are 7,894 domains in the catalog, so this is an easier task than item predictions.

> https://ml-challenge.mercadolibre.com/rule

## Models

The strategy used for this challenge has several parts:

### Implicit rating

### Matrix Factorization

[Alternating Least Squares](http://yifanhu.net/PUB/cf.pdf) (ALS) is a matrix factorization model, this model uses implicit ratings of user-items interactions, instead of explicit rating (like stars movies), or the binary representation commonly used in implicit interactions. So it's great for the implicit ratings already created.

In addition to the items and users encoding in the model, I added features items and search terms in the user-item matrix representation, to enrich te semantic of the recommendations, especially for items with few pageviews (cold start problem), and to tend to recomend items of the same domain. ([notebook](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/02-AlternatingLeastSquaresModel.ipynb))

Finally, the last submission was a weighted ensemble of the implicit rating of the items viewed in the user session, and the predicted scores of two models ALS with different settings. ([notebook](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/03-EnsembleModel.ipynb))


## Results

| Model                                 | Local score       | Public score  | Private score  |
|---------------------------------------|-------------------|---------------|----------------|
| [Baseline](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/00-ExploratoryAnalysis.ipynb) (sorted visited items)  | 0.2085            |               |                |
| [Simple implicit rating](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/01-Item2itemModel.ipynb)               | 0.2639            |               |                |
| [Items to items](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/01-Item2itemModel.ipynb)          | 0.2889            | 0.28755       |                |
| [ALS](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/02-AlternatingLeastSquaresModel.ipynb) Ensamble (last submission](https://github.com/leolnn/MeliDataChallenge2020/blob/main/notebooks/03-EnsembleModel.ipynb))        | 0.3180            | 0.31293       | 0.30920        |

> https://ml-challenge.mercadolibre.com/final_results
