Predicting wine quality using measurements of physiochemical tests
================
Alex Truong, Bruhat Musinuru, Rui Wang and Sang Yoon Lee </br>
2020-11-26 (updated: 2020-12-13)

  - [Summary](#summary)
  - [Introduction](#introduction)
  - [Methods](#methods)
      - [Data](#data)
      - [Analysis](#analysis)
  - [Results & Discussion](#results-discussion)
  - [References](#references)

## Summary

For this analysis, we used the neutral network Multi-layer Perception
(MLP) model in order to try to predict wine quality based on the
different wine attributes obtained from physicochemical tests such as
alcohol, sulfur dioxide, fixed acidity, residual sugar. When we test it
with the different validation data sets, the model yield robust results
with 81% f1 micro score. We also have comparably high score at 83%
f1 micro when we run the model on our test set. Based on
these results, we opine that that the model seems to generalize well
based on the test set predictions.

However, it incorrectly classifies considerable number of examples in the lower end of
spectrum (between normal and poor). This could be due to class imbalance
present in the data set where normal samples outnumber poor ones (as
demonstrated in Figure 2 below). Improving the data collection methods
to reduce the data class imbalance and using an appropriate assessment
metric for imbalanced data can help to improve our analysis. On the
other hand, given the rate of miss-classification is not so high and the
impact can be corrected in further assessment, we believe this model
could decently serve its purpose as a wine predictor to conduct
first-cut assessment, which could help speed up the wine ratings
process.

## Introduction

Traditional methods of categorizing wine are prone to human error and
can vary drastically from expert to expert. We propose a data mining
approach to predict wine quality using machine learning techniques for
classification problems. The resulting model, we hope, could serve as as
one of scientific and systematic ways to classify wine, which is a
springboard for further research in personalized wine recommendation,
quality assessment and comparison unit.

Moreover, we believe wineries or wine rating institutes could find the
model as a useful and reliable first-cut wine quality test before
further expert’s assessment. This could lead to a more cost and
time-effective wine screening process, and subsequently facilitate more
effective and efficient business decisions and strategies.

## Methods

### Data

The data set used in this project is the results of a chemical analysis
of the Portuguese “Vinho Verde” wine, conducted by Paulo Cortez, A.
Cerdeira, F. Almeida, T. Matos and J. Reis (Cortez et al. 2009). It was
sourced from the UCI Machine Learning Repository (Dua and Graff 2017)
which can be found
[here](https://archive.ics.uci.edu/ml/datasets/wine+quality).

There are two datasets for red and white wine samples. For each wine
sample observation , the inputs contains measurements of various
objective physicochemical tests, and the output is the median wine
quality ratings given by experts on the scale from 0 (very bad) and 10
(very excellent).The author notes that data on grape types, wine brand,
wind selling price among other are not available due to privacy and
logistics issues. There are 1599 observations for red wine and 4898
observations of white wine (as demonstrated in Figure 1).

<div class="figure" style="text-align: center">

<img src="../eda/wine_EDA_files/distribution_of_type_of_wine.svg" alt="Figure 1: Distribution of type of wine" width="50%" />

<p class="caption">

Figure 1: Distribution of type of wine

</p>

</div>

<div class="figure" style="text-align: center">

<img src="../eda/wine_EDA_files/wine_quality_rank.svg" alt="Figure 2: Class imbalance in wine quality rank" width="50%" />

<p class="caption">

Figure 2: Class imbalance in wine quality rank

</p>

</div>

### Analysis

At the preprocessing stage, we decided to combine the red and white data
set as well as group the data in bigger classification, namely “poor”,
“normal” and “excellent” for scale “1-4”, “5-6” and “7-9” so as to
have bigger sample size (as per Figure 3). We acknowledge that the data
is imbalanced, hence instead of using accuracy to judge the
model performance, we are using f1-score as our main
assessment metric. When choosing our scoring metric, we had quite a few options but we narrowed them down to F1 macro , F1 micro  and F1 weighted. We wanted our score to calculate metrics globally while also considering the false negatives and false positives. But, Since our classes are imbalanced, F1 macro cannot be used as it does not take label imbalance into account. F1 weighted seems to be a choice but since we wanted to take precision and recall into account, this wouldn't be our ideal choice. Finally, we decided to use F1 micro as it calculates metrics globally while taking the total true positives, false negatives and false positives into consideration.

<div class="figure" style="text-align: center">

<img src="wine_classification.png" alt="Figure 3: Regrouping of wine quality classification" width="50%" />

<p class="caption">

Figure 3: Regrouping of wine quality classification

</p>

</div>

In this project we are trying to predict the quality of a given wine
sample using wine attributes obtained from various physicochemical
tests. Based on our literary review, we found that researchers from
Karadeniz Technical Univeristy used Random Forest Algorithm had also
tried to classify between red wine and white wine for the same dataset
(Er and Atasoy 2016). They further used 3 different data mining
algorithms namely k-nearest-neighbourhood random forests and support
vector machine learning to classify the quality of both red wine and
white wine. This motivates us to proceed with to use cross-validation to
select the best model for our analysis.

We eventually decided to pick neutral network Multi-layer Perception
(MLP) model as the model that yield the best results after running the
various machine learning models through the train data set, comparing
their performance based on f1-score and checking consistency across
cross-validation runs. We noticed that random forest recorded high
f1-validation score at 0.84, however, it also had a large gap between
train and validation with a perfect train score of 1. This caused us to
think the model has overfitted. Logistic regression also showed
promising f1 validation score results in our case, yet this high results
were not consistent across cross-validation splits. Hence, with most
models struggled to get to the 0.8 f1-score mark without significantly
overfitting on the train set, while MLP shows consistent results across
all cross-validation splits, our final choice landed on MLP model
because we think it would generalize better.

<div class="figure" style="text-align: center">

<img src="../results/f1_score_all_classifiers.svg" alt="Figure 4: Score results among different machine learning model we have explore"  />

<p class="caption">

Figure 4: Score results among different machine learning model we have
explore

</p>

</div>

The Python and R programming languages (R Core Team 2019; Van Rossum and
Drake 2009) and the following Python and R packages were used to perform
the analysis: scikit-learn (Pedregosa et al. 2011), docoptpython
(Keleshev 2014), docopt (de Jonge 2018), altair (VanderPlas et al.
2018), vega-lite (Satyanarayan et al. 2017), IPython-ipykernel (Pérez
and Granger 2007), matplotlib (Hunter 2007), scipy (Virtanen et al.
2020), numpy (Harris et al. 2020), pandas (McKinney and others 2010),
graphviz (Ellson et al. 2001), pandas-profiling (Brugman 2019), knitr
(Xie 2014), tidyverse (Wickham 2017), kableExtra (Zhu 2020). The code
used to perform the analysis and re-create this report can be found
[here](https://github.com/UBC-MDS/Wine_Quality_Predictor#usage)

## Results & Discussion

Looking at the distribution plot of the respective wine quality group
interacting with each explanatory features, we can see that higher
quality wine seems to be more associated with higher `alcohol` level and
lower `density`. Lower `volatile acidity` also seems to be indicative of
better wine. Better ranked wine also seem to have `higher free sulfur
dioxide` level than poor wine though the relationship is not that clear
based on the plot. The rest of the features do not seems be very
distinguishable among different quality wine.

<div class="figure" style="text-align: center">

<img src="../eda/wine_EDA_files/wine_quality_rank_per_feature.svg" alt="Figure 5: Distribution plot between wine quality and various attributes from physicochemical test"  />

<p class="caption">

Figure 5: Distribution plot between wine quality and various attributes
from physicochemical test

</p>

</div>

Since this is a multi-class classification, our goal was to find a model
that was consistent and able to recognize patterns from our data. We
choose to use a neutral network Multi-layer Perception (MLP) model as it
was consistent and showed promising results. If we take a look at the
f1 scores across cross validation splits, we can see
that MLP is pretty consistent which was not the case with Random forests and many models.

<div class="figure" style="text-align: center">

<img src="../results/f1_score_random_forest.svg" alt="Figure 6:  f1 micro scores across cross validation splits for RandomForest model (left) and neutral network Multi-layer Perception (MLP) model (right)" width="50%" height="20%" /><img src="../results/f1_score_mlp.svg" alt="Figure 6:  f1 micro scores across cross validation splits for RandomForest model (left) and neutral network Multi-layer Perception (MLP) model (right)" width="50%" height="20%" />

<p class="caption">

Figure 6: f1 micro scores across cross validation splits for
RandomForest model (left) and neutral network Multi-layer Perception
(MLP) model (right)

</p>

</div>

Our model performed quite well on the test data as well. If we take a
look at the confusion matrix below. As we discussed earlier, the
prediction at the lower end of wine quality spectrum is acceptable. As
we can see from the row normalized confusion matrix below, the error rate for the
lower end of spectrum is very acceptable and we have relatively low number of false classifications in
the high end of spectrum. The final test f1\_micro score for our model
is similar to our validation score and right at where we expected it to be, at 0.83538.

<div class="figure" style="text-align: center">

<img src="../results/final_model_quality.png" alt="Figure 7: Confusion Matrix" width="640" />

<p class="caption">

Figure 7: Confusion Matrix

</p>

</div>

Having said that the research also need further improvement in terms of
obtaining a more balanced data set for training and cross-validation.
More feature engineer and selection could be conducted to minimize the
affect of correlation among the explanatory variable. Furthermore, in
order to assess the robustness of the predicting model, we need to test
the model with deployment data in real world besides testing with our
test data.

In conclusion, we think that with a decent error rate, our predicting
model based on neutral network Multi-layer Perception (MLP) model would
serve well as an effective first-cut assessment on wine quality.

# References

<div id="refs" class="references hanging-indent">

<div id="ref-pandasprofiling2019">

Brugman, Simon. 2019. “pandas-profiling: Exploratory Data Analysis for
Python.” <https://github.com/pandas-profiling/pandas-profiling>.

</div>

<div id="ref-cortez2009modeling">

Cortez, Paulo, António Cerdeira, Fernando Almeida, Telmo Matos, and José
Reis. 2009. “Modeling Wine Preferences by Data Mining from
Physicochemical Properties.” *Decision Support Systems* 47 (4): 547–53.

</div>

<div id="ref-docopt">

de Jonge, Edwin. 2018. *Docopt: Command-Line Interface Specification
Language*. <https://CRAN.R-project.org/package=docopt>.

</div>

<div id="ref-Dua:2019">

Dua, Dheeru, and Casey Graff. 2017. “UCI Machine Learning Repository.”
University of California, Irvine, School of Information; Computer
Sciences. <http://archive.ics.uci.edu/ml>.

</div>

<div id="ref-graphviz">

Ellson, John, Emden Gansner, Lefteris Koutsofios, Stephen North, Gordon
Woodhull, Short Description, and Lucent Technologies. 2001. “Graphviz -
Open Source Graph Drawing Tools.” In *Lecture Notes in Computer
Science*, 483–84. Springer-Verlag.

</div>

<div id="ref-er2016classification">

Er, Yeşim, and Ayten Atasoy. 2016. “The Classification of White Wine and
Red Wine According to Their Physicochemical Qualities.” *International
Journal of Intelligent Systems and Applications in Engineering*, 23–26.

</div>

<div id="ref-harris2020array">

Harris, Charles R., K. Jarrod Millman, St’efan J. van der Walt, Ralf
Gommers, Pauli Virtanen, David Cournapeau, Eric Wieser, et al. 2020.
“Array Programming with NumPy.” *Nature* 585 (7825): 357–62.
<https://doi.org/10.1038/s41586-020-2649-2>.

</div>

<div id="ref-matplotlib">

Hunter, J. D. 2007. “Matplotlib: A 2D Graphics Environment.” *Computing
in Science & Engineering* 9 (3): 90–95.
<https://doi.org/10.1109/MCSE.2007.55>.

</div>

<div id="ref-docoptpython">

Keleshev, Vladimir. 2014. *Docopt: Command-Line Interface Description
Language*. <https://github.com/docopt/docopt>.

</div>

<div id="ref-pandas">

McKinney, Wes, and others. 2010. “Data Structures for Statistical
Computing in Python.” In *Proceedings of the 9th Python in Science
Conference*, 445:51–56. Austin, TX.

</div>

<div id="ref-scikit-learn">

Pedregosa, F., G. Varoquaux, A. Gramfort, V. Michel, B. Thirion, O.
Grisel, M. Blondel, et al. 2011. “Scikit-Learn: Machine Learning in
Python.” *Journal of Machine Learning Research* 12: 2825–30.

</div>

<div id="ref-IPython">

Pérez, Fernando, and Brian E. Granger. 2007. “IPython: A System for
Interactive Scientific Computing.” *Computing in Science and
Engineering* 9 (3): 21–29. <https://doi.org/10.1109/MCSE.2007.53>.

</div>

<div id="ref-R">

R Core Team. 2019. *R: A Language and Environment for Statistical
Computing*. Vienna, Austria: R Foundation for Statistical Computing.
<https://www.R-project.org/>.

</div>

<div id="ref-vega-lite">

Satyanarayan, Arvind, Dominik Moritz, Kanit Wongsuphasawat, and Jeffrey
Heer. 2017. “Vega-Lite: A Grammar of Interactive Graphics.” *IEEE
Transactions on Visualization and Computer Graphics* 23 (1): 341–50.

</div>

<div id="ref-altair">

VanderPlas, Jacob, Brian Granger, Jeffrey Heer, Dominik Moritz, Kanit
Wongsuphasawat, Arvind Satyanarayan, Eitan Lees, Ilia Timofeev, Ben
Welsh, and Scott Sievert. 2018. “Altair: Interactive Statistical
Visualizations for Python.” *Journal of Open Source Software* 3 (32):
1057. <https://doi.org/10.21105/joss.01057>.

</div>

<div id="ref-Python">

Van Rossum, Guido, and Fred L. Drake. 2009. *Python 3 Reference Manual*.
Scotts Valley, CA: CreateSpace.

</div>

<div id="ref-SciPy">

Virtanen, Pauli, Ralf Gommers, Travis E. Oliphant, Matt Haberland, Tyler
Reddy, David Cournapeau, Evgeni Burovski, et al. 2020. “SciPy 1.0:
Fundamental Algorithms for Scientific Computing in Python.” *Nature
Methods* 17: 261–72. <https://doi.org/10.1038/s41592-019-0686-2>.

</div>

<div id="ref-tidyverse">

Wickham, Hadley. 2017. *Tidyverse: Easily Install and Load the
’Tidyverse’*. <https://CRAN.R-project.org/package=tidyverse>.

</div>

<div id="ref-knitr">

Xie, Yihui. 2014. “Knitr: A Comprehensive Tool for Reproducible Research
in R.” In *Implementing Reproducible Computational Research*, edited by
Victoria Stodden, Friedrich Leisch, and Roger D. Peng. Chapman;
Hall/CRC. <http://www.crcpress.com/product/isbn/9781466561595>.

</div>

<div id="ref-kableExtra">

Zhu, Hao. 2020. *KableExtra: Construct Complex Table with ’Kable’ and
Pipe Syntax*. <https://CRAN.R-project.org/package=kableExtra>.

</div>

</div>
