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

## Limitations