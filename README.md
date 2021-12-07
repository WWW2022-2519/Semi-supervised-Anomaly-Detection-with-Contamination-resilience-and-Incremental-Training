# Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training


**Response to Reviewer 1 [Q1]: Experimental analyses for the normal data that seems to be anomalies.**

It is true there may exist some normal data that seem to be anomalies, although their amount is very small in practice. For convenience, these data are called **atypical normal data** in this response. When atypical normal data are used in model training, anomaly detection models often shift their decision boundary to accommodate them. And this often results in the degradation of detection performance. This is because typical normal data accounts for the vast majority of data, and scarifying the detection performance for them is more harm than good.

Owing to the introduction of the sample weights, our method can mitigate this problem. More specifically, the negative impacts of atypical normal data are weakened since they are assigned relatively small weights. To show how our method takes effect, we conduct experiments on a toy dataset with synthetic data.

**Experimental setting**

For the purpose of visualization, we choose to use a synthetic dataset in our experiments. Moreover, we can purposefully generate atypical normal data according to our requirements, through using a synthetic dataset. Without loss of generality, all data are generated based on Gaussian distributions.

To be specific, we use a two-dimensional Gaussian distribution (e.g., mean is (0,0) and covariance matrix is [[0.5,0], [0,0.5]]) to represent the typical normal data. And we use other Gaussian distributions (e.g., mean is (2,2) and covariance matrix is [[0.25,0], [0,0.25]]) to represent anomalous data. We then use the data in the **overlapped** area of these two distributions to represent atypical normal data (i.e. the normal data seems to be anomalies). These three kinds of data are depicted in Fig. 1. In the subfigures, the **blue** points are **typical normal data,** the **green** points are **anomalous data****,** and the **orange** points are **atypical normal data**.

It can be seen in these settings, atypical normal data seem to be anomalies. In the first setting shown in subfigure (a), anomalous data come from one source. In the second and the third settings, the anomalous data come from two or more sources. 




<figure>
    <div align="center"/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/fig1.jpg" width=90%/>                  
   **Fig. 1 The distribution of normal and anomalies data.**
</figure>
    
**Experiments result and analyses**

1. How does our method assign **weights** to normal data samples?

We first discuss how our method deals with atypical normal data. Recall that our model training only uses normal data samples, each of which is assigned a weight. For visualization, Fig. 2 shows the heatmap of the weights assigned to these normal data samples. Note that our method introduces a **hyperplane** in weight assignment (please find the details in Eq. 9 in our manuscript). Therefore, the points (i.e., samples) close to that hyperplane are assigned large weights. That is, the points in the center are more likely to have weights. Contrarily, the points corresponding to atypical normal data are often assigned small weights. And this weaken their impacts on model performance, since our method takes these weights into account during model training.


<figure>
    <div align="center"/>
    <img 
   src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure4.png" width =330/ >



​                                                                                                  **Fig. 2 The heatmap of the weight.**
</figure>


 
  
 2. Are our weights **helpful** for detection performance improvement?

To show our weights are helpful for performance improvement, we compare our method (i.e., **SAE-CRIT**) with the SOTA method **GOAD** and the method that is obtained by removing weight assignment from SAE-CRIT, denoted by **SAE-CRIT-NW**. Our experiments are conducted under the experimental settings shown in Fig. 1. The results are presented in Fig. 3. We can draw two conclusions from Fig. 3. 

(1)	Our SAE-CRIT method has higher detection performance than SAE-CRIT-NW, owing to the introduce of sample weights.  

(2)	Our SAE-CRIT method also performs better than the SOTA method **GOAD** under all settings.  


<figure>
    <div align="center"/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure5.png" width =30%/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure6.png" width =30%/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure7.png" width =30%/>


​  &ensp;&ensp;&ensp;&ensp;&ensp;&ensp; **(a) setting1         &ensp; &ensp;&ensp; &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; &ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp;           (b) setting2       &ensp;  &ensp;&ensp; &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;          (c) setting3**

​                                                                **Fig. 3 Comparing different methods under various settings.**
 </figure>  
    
    
    
**Response to Reviewer 3 [Q1]: The theoretical analyses about why our method is not affected by low base-rate.**

Thanks for your comment on this problem. We learn much from your comment and the recommended paper. In the following, we first explain why the **base-rate fallacy** considered in Axelsson’s TISSEC 2002 paper often occurs in **supervised and Bayes theory based methods**. We then discuss why our method is not affected by low base-rate. Finally, we supply the derivation process of our method to show that our method has no intersection with low base-rate fallacy.

**(1) Base-rate fallacy**

The base-rate fallacy [1] is people’s tendency to ignore base rates in favor of, e.g., individuating information (when such is available), rather than integrate the two. 

We first summarize the analyses presented in Axelsson’s TISSEC 2002 paper as follows. In this paper, the authors' analyses starts from the Bayes's theorem, i.e., 

<img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/image-20211207201049937.png" width=330 />

where *P(y)* represents prior probability, *P(X|y)* denotes posterior probability, *P(y|X)* is the class conditional probability, and *n* is the number of classes. For example, *n*  is set to *2* in anomaly detection tasks.

Accordingly, a **supervised** classification problem is divided into two stages: inference and decision.
In the inference stage, training data are used to learn posterior probabilities *P(X|y)*. In the decision stage, these posterior probabilities are employed to estimate class conditional probability *P(y|X)*. In practice, *P(X|y)* is often used to substitute *P(y|X)*. **Due to the low base-rate of data in a certain class,*P(X|y)* calculated from training data is heavily unequal to the class conditional probability *P(y|X)*, hence resulting in poor classification performance**. And this problem is termed base-rate fallacy.

Now we apply the above analyses to **supervised** anomaly detection tasks. In these tasks, there are two classes, i.e.,  the normal class (its data is denoted by *x1*) and the anomalous class (its data is denoted by *x2*). We define their labels by *y1* and *y2*, respectively. In training set, the ratio of the anomalous data *x2* to total data *x1+x2* is *1\%*. **This setting corresponds to the low base-rate of *1\%* mentioned by Reviewer 3**. Furthermore, we assume the posterior probability *P(x2| y2)* is *99%*, following the Axelsson’s TISSEC 2002 paper. Then the class conditional probability * P(y2 |x2)* can be calculated as

<img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/image-20211207201615447.png" width =500 />

In this case, the class conditional probability *P(y2|x2)*  (i.e., *50%*) and posterior probabilities *P(x2 | y2)* (i.e., *99%*) are heavily unequal to each other. **And this result tells us that the low base-rate indeed brings trouble in supervised anomaly detection methods.**  Hence, low base-rate is often emphasized in the studies of supervised anomaly detection. 

**(2) Our method is not affected by low base-rate. Why?**

Due to the following two reasons, we think our method is not affected by low base-rate.

* Due to the absence of prior probability, many methods choose to estimate the posterior probability *P(X | y)*, thus causing the low base-rate fallacy. The maximum likelihood rule chooses likelihood probability function maximization as the objective function. However, our method relies on the **least square method**  instead of the estimation of posterior probability *P(X | y)*. More specifically, our method takes the sum of the squares of the difference between the estimated value and the observed value as the loss function (please find the details at [1]). 
* Our method is **semi-supervised** instead of supervised. In our model training, we do not use anomalous data at all. Note that in anomaly detection tasks, the base-rate is the ratio of anomalous data amount to all training data amount. Therefore, the low base-rate problem does not exist in our work. 

Furthermore, it is worth noting that we use a ratio of *20%* in our experiments. We are sorry about the confusion. In fact, the ratio *20%* is not the base-rate. It is the proportion of anomalous data in our dataset KDD. To our knowledge, many related works use this dataset KDD and follow this setting. So we use this ratio in our model test. Moreover, our method is semi-supervised, and hence we do not use anomalous data in model training. As a result, this setting (i.e., *20%*) does not affect the anomaly detection capability of our model (the anomaly detection capability heavily depends on model training instead of model test). 

**(3) Derivation process of our method**

The relationship between Maximum a posteriori estimation (MAP) and our method

 A maximum posterior estimate is a point estimate of a quantity that is difficult to observe based on empirical data.

<img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/image-20211207201715777.png" width =600 />

We set  *e=Y-XW*, if the <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/image-20211207202545652.png" alt="image-20211207202545652" width =100 align="middle" /> and  *W~Lapace(0,b)*, the parameter *W* can be estimated by the Maximum a posteriori estimation as follows:

<img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/image-20211207202011826.png" width =600 />

So when the <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/image-20211207202545652.png" alt="image-20211207202545652" width =100 align="middle" /> and  *W~Lapace(0,b)*, The MAP estimation is equal to our method in the mathematical form, but there is no essential connection between the two, so there is no base-rate fallacy in our article.



[1] Refenes A N , Bentz Y , Bunn D W , et al. Financial time series modelling with discounted least squares backpropagation[J]. Neurocomputing, 1997, 14(2):123-138.
