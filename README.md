# Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training


**Reviewer 1 [Q1]**

It is true there may exist some normal data that seem to be anomalies, although their amount is very small in practice. For convenience, these data are called **atypical normal data** here. When the atypical normal data are used in model training, anomaly detection models often shift their decision boundary to adapt to accommodate then. In this case, detection performance often degrades since the typical normal data, which accounts for the vast majority of data, are sacrificed.

Owing to the introduction of the sample weights, our method can mitigate this problem.  More specifically, the negative impacts of atypical normal data are weakened since they are assigned relatively small weights. To show how our method takes effect, we conduct experiments on a toy dataset with synthetic data.

**Experiment setting**

For visualization, we choose to use synthetic dataset in our experiments. In this way, we can purposefully generate atypical normal data. 

To be specific, we use a two-dimensional Gaussian distribution (e.g., mean is (0,0) and covariance matrix is [[0.5,0], [0,0.5]], termed as Gaussian_Nor) to represent the typical normal data. And we use other Gaussian distributions (e.g., mean is (2,2) and covariance matrix is [[0.25,0], [0,0.25]], termed as Gaussian_Anor) to represent anomalous data. We then use the data in the overlapped area of these two distributions to refer to atypical normal data (i.e. the normal data seems to be anomalous). As shown in Fig. 1, the **blue** points are **typical normal data,** the **green** points are **anomalous data** and the **orange** points, located in the overlapped area, are **atypical normal data**. 

We consider three experimental settings, as depicted by the subfigures of Fig. 1. In the first setting, anomalous data come from one source. In the second and the third settings, the anomalous data come from more sources. 



<figure>
    <div align="center"/>
    <img src="file:///https://github.com/WWW2022-2519/Semi-supervised-Anomaly-Detection-with-Contamination-resilience-and-Incremental-Training/blob/main/figure1.png" width =350/>
    <img src="file:///C:/Users/pc/AppData/Local/Temp/lu224401qxmru.tmp/lu224401qxmss_tmp_a88e983ae8cfcd1f.png" width =350/>
    <img src="file:///C:/Users/pc/AppData/Local/Temp/lu224401qxmru.tmp/lu224401qxmss_tmp_7e7cd2688ef20dfb.png" width =350/>


​                                                                              **(a) setting1                   (b) setting2                 (c) setting3**                        

​                                                                                 **Fig. 1 The distribution of normal and anomalies data.**

**Experiments Result**

1. ​	How does our method assign **weights** to normal data samples?

We first discuss how our method deals with the atypical normal data. Recall that our model training only uses normal data samples, each of which is assigned a weight. To reflect how large the weight of every data sample is, Fig. 2 shows the heatmap of the weights assigned to these normal data samples. Note that our method introduces a hyperplane in weight assignment (please find the details in eq. 9 in our manuscript). The points (i.e., samples) close to the hyperplane are assigned a high weight. That is, the points in the center are more likely to have a high weight. Accordingly, the points corresponding to the atypical normal data are often assigned small weights, making them impose slight impacts on model performance.



<figure>
    <div align="center"/>
    <img 
   src="file:///C:/Users/pc/AppData/Local/Temp/lu224401qxmru.tmp/lu224401qxmsg_tmp_182c2347e7aa4363.png" width =330/ >
</figure>


​                                                                                                  **Fig. 2 The heatmap of the weight.**

2. Are our weights **helpful** for detection performance improvement?

To show our weight assignment method is helpful for performance improvement, we compare our method (i.e., **SAE-CRIT**) with the SOTA method **GOAD** and the method that is obtained by removing weight assignment from SAE-CRIT, denoted by **SAE-CRIT-NW**. Our experiments are conducted under the three experimental settings shown in Fig. 1. The results are shown in Fig. 3. It can be seen that, SAE-CRIT has higher detection performance than SAE-CRIT-NW due to the introduce of sample weights. In addition, SAE-CRIT also performs better than the SOTA method **GOAD** under all settings. 

 

<figure>
    <div align="center"/>
    <img src="file:///C:/Users/pc/AppData/Local/Temp/lu224401qxmru.tmp/lu224401qxmso_tmp_a5920e286e6a90b7.png" width =330/>
    <img src="file:///C:/Users/pc/AppData/Local/Temp/lu224401qxmru.tmp/lu224401qxmso_tmp_7eff18c68618e996.png" width =330/>
    <img src="file:///C:/Users/pc/AppData/Local/Temp/lu224401qxmru.tmp/lu224401qxmso_tmp_f6a2cd9b2835db11.png" width =330/>


​                                                                        **(a) setting1                   (b) setting2                 (c) setting3**

​                                                                **Fig. 3 Comparing different methods under various settings.**
