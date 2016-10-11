---
layout: default
img: ml_pyramid 
caption: Machine Learning Skills Pyramid v1.0 by Steven Geringer
img_link: https://www.kaggle.com/forums/f/15/kaggle-forum/t/10799/what-makes-a-rock-star-machine-learning-scientist
title: Homework 2 | Logistic Regression
active_tab: homework
---



Logistic Regression <span class="text-muted">: Assignment 2 (30 points)</span> 
=============================================================


<div class="alert alert-info">
  <p>Due Thursday October 31st, 2016 (in 3 weeks); 3 flexible days for the whole semester; 20% penalty per day for late submission.</p>
</div>


We have been talking about logistic regression, one of the most important and fundamental machine learning (ML) algorithms, in the class. It is time for you to execute what you have learned in the class to really implement the logistic regression model. Instead of covering many algorithms quickly on the high-level and use ML algorithm libraries (e.g. [Scikit-learn](http://scikit-learn.org/stable/)), this course aims to explain a few important ML algorithms in-depth. It is meant to prepare you to be a a researcher/scientist --- the very top class in the [Machine Learning Skills Pyramid](http://socialmedia-class.org/assets/img/ml_pyramid.jpg), who can not only  apply algorithms but create algorithms! 



In this assignment, you will implement the logistic regression and get a good understanding of the key components of logistic regression (many other machine learning algorithms as well):

- hypothesis function
- cost function
- decision boundary
- gradient decent algorithm
- [feature scaling](https://en.wikipedia.org/wiki/Feature_scaling)
- and [gradient checking](http://ufldl.stanford.edu/tutorial/supervised/DebuggingGradientChecking/) (optional)

You will also apply your implemented logistic regression model to a small dataset and predict whether a student will be admitted to a university. This small dataset will allow you to visualize the data and debug more easily. You will find this **[documentation](http://socialmedia-class.org/slides/AndrewNg_ex2.pdf)** very helpful, though it is about how to implement logistic regression in Octave.  


#### Instructions

You will use [Jupyter Notebook](http://jupyter.org/), an interactive Python programming and data visualization tool, for this homework. You can follow [this guide](http://jupyter.readthedocs.io/en/latest/install.html) to install Anaconda, that conveniently includes Python, the Jupyter Notebook and other commonly used packages for scientific computing and data science. The start code is tested for Python 3.4 that you are recommended to use. 

You can download the homework assignment zip file from Carmen that contains the starter code, some visualization, and explanatory text. Then, [open the included *.ipynb file](http://jupyter.readthedocs.io/en/latest/running.html) in the Jupyter Notebook and follow all the instructions to do your work there.

After finished, follow the instruction, pack your code into a zip file named like 'hw2_yourdotid.zip', and submit it in the [OSU's Carmen](https://carmen.osu.edu/) system.


#### Extracurricular

If you are interested in applying logistic regression to some real-world NLP problems, you can try out this task and dataset: [Paraphrase Identification and Semantic Similarity in Twitter](http://alt.qcri.org/semeval2015/task1/). It was an international research competition (so called shared-task) organized by the instructor with Microsoft Research that has attracted 19 top research groups to participate in 2015, including Stanford, Columbia University, MITRE and etc. More details can be found in the [overview paper](https://cocoxu.github.io/publications/semeval_pit_2015_overview.pdf) of the 2015 shared-task. We are planning to organize it again in 2017. Note that, for a large dataset using many features, you often need to implement some regularizations (e.g. L2) to avoid overfitting and use an advanced optimization algorithm (e.g. L-BFGS; you probably should not implement these algorithms unless you are an expert in numerical computing) for minimizing the cost function. 











