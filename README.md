# Is Science Becoming Less Disruptive?

A [recent paper in Nature](https://www.nature.com/articles/s41586-022-05543-x) created a stir in the scientific community, arguing that science is becoming less disruptive over time. According to the study, there are fewer groundbreaking papers in recent years. It appears that trailblazers are rare and that most research tends to build and expand existing research rather than opening up new paths of inquiry.

The core aim of this project is to investigate the validity of this claim by attempting to create a Machine Learning model that can ***predict, as accurately as possible, the disruptiveness of a scientific paper.***

The analysis conducted is a part of an [assignment](/disruptive_science_assignment.ipynb) of the course Applied Machine Learning at the Department of Management Science and Technology in the Athens University of Economics and Business (AUEB) by professor [Panos Louridas](https://github.com/louridas). 

## CD5 Index

Disruption can be quantified using a metric known as the [CD index](http://russellfunk.org/cdindex/). The CD index is computed within a specified time frame, such as five years (referred to as CD5), and it gauges the probability that a work referencing a scientific paper will also reference the sources mentioned in that paper. When subsequent works do not reference the same sources as the paper they are citing, it indicates that the paper has likely introduced new and groundbreaking ideas.

## Problem Definition

The CD index aims to capture the disruption level of a publication. By default the metric is caclulated using information before the papers publication (past), information about the published paper itself (present) and information about the citations of the paper (future). <br>

The key factors which influence the metric are:
- the authors
- the subjects
- the papers referenced
- the number of citations the publication obtains from other papers

The objective is to develop a model that can accurately forecast the CD index of a publication by utilizing solely the information available about the paper itself (current) and the papers it cites as references (prior). It is important to note that using any information from the future is strictly prohibited in this endeavor.

## Dataset

The dataset used for the analysis is derived from the [alexandria3k package](https://github.com/dspinellis/alexandria3k) created by [Professor Diomidis Spinellis](https://www2.dmst.aueb.gr/dds/). The library provides "Local relational access to openly-available publication data sets".
<br>

The particular dataset used comprises part of the publications graph for a 1% sample of all CrossRef available publications that have abstracts. The CD index of each paper is contained in the dataset. 

## Different Approaches

1. **Descriptive Statistics in a ML model**
- The metrics which can arise from the paper itself and its citations are leveraged
- Indicative features : number of authors, number of subjects, number of references etc.

2. **NLP Approach**
- The title and abstract of each publication are used to extract features by applying NLP techniques
- The features extracted are combined with the initial metrics into a single model
- Indicative features : number of words, number of sentences, number of unique words, density ratio etc.
- Word Embeddings from a pretrained model are also used to extract features from the title and abstract of each publication

3. **Classification Approach** <br>
At last, since the dataset is highly imbalanced and contains a significant number of papers with 0 or 1 cd5 index, the the CD index is transformed into a binary variable and the problem is turned into classification.

The in depth analysis and the results can be found in the [Solution](/cd5_prediction_solution.ipynb) notebook.

## Results Evaluation

- The models are evaluated using the Mean Absolute Error (MAE), Mean Squared Error (MSE) and the R2 Score
- Overall, the models have similar performance in terms of predicting the CD5 index
- In this case, the feature extraction using NLP techniques does not seem to improve the performance of the models
- The models perform better when the CD5 index is transformed into a binary variable, since the dataset is highly imbalanced and balancing techniques were ineffective

The best performing model was the Neural Network with the architecture below:

```
nn = keras.Sequential([
        normalizer,
        layers.Dense(64, activation='relu'),
        layers.Dense(64, activation='relu'),
        layers.Dense(1)
    ])
```

The metric `mean_squared_error` was used as the loss function and the `Adam` optimizer was used with a learning rate of `0.001`. The NN is trained for 100 epochs with a batch size of 32. During each epoch, 20% of the data is used for validation.

***Features used:***

- number of subjects of the publication
- number of cited papers by the publication
- number of authors of the publication
- mean age of the papers cited by the publication 
- standard deviation of the publication year of the papers cited by each publication 

***Results***
- MSE: 0.00841
- R2 (Test): 0.96


## Limitations

- The sample used - 1% of total publications graph - might lead to biased metrics and models
- Using a more complete dataset is not computationally feasible. However, it might lead to more accurate predictions
- The models proposed might not perform well when tested on the complete publications graph as they have been tweaked to fit on current data
- The heavily imbalanced dataset has lead to models which are accurate in predicting target values which are sufficiently represented in the training set
