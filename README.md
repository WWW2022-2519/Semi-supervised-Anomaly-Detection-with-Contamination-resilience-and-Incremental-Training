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
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure1.png" width =330/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure2.png" width =330/>    
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure3.png" width =330/>

  
   **(a) setting1          &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp;         (b) setting2            &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp; &ensp;&ensp;&ensp;&ensp;     (c) setting3**                        
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
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure5.png" width =330/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure6.png" width =330/>
    <img src="https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure7.png" width =330/>


​                                                                        **(a) setting1         &ensp;          (b) setting2       &ensp;          (c) setting3**

​                                                                **Fig. 3 Comparing different methods under various settings.**
