# AnomaliesDetector
This repository contains all source code, figures, etc. of our paper :
["Anomalies Detection in Metocean Simulation Results Using Convolutional Neural Networks"][1]


[1]: https://www.researchgate.net/publication/327900515_Anomalies_Detection_in_Metocean_Simulation_Results_Using_Convolutional_Neural_Networks
With our algorithm it is possible to reduce experts’ participation. One can find explanation of anomalies detection in fields of sea current velocity and ice concentration below. How-to-use instructions will be added later.
## Data
For the task of sea current velocity modelling data was obtained with NEMO ocean model and for ice concentration task was provided by OSI SAF from 1980 to 2015 year.
Why this is useful 
With our code it is possible to reduce expert labeling, so the process of predicting and detection of anomalies becomes more automatically working. This may be useful at metocean stations and anomalies research labs. 
## How it works
In order to increase training and testing set, we suggest to divide all data images into subzones (size 100x100 grid points is the most appropriate solution).
Then during sea current anomalies detection we sequentially check of the correctness of each sub-area using a CNN ((a simple model with 2 consequent convolution and pooling layers with ReLU activation function and 2 densely-connected with sigmoid function as an output) trained on correct (manually labeled by expert) Arctic sea images. 
Thus, on the test set the following **results** were obtained: AUC of ROC curve = 0.987; AUC of Precision-Recall curve = 0.959. As we expected, supervised learning shows great results.
In ice concentration detection task we took into account its spatial dependence nature and seasonal variability, so our data expanded significantly. The CNN trained on correct ice concentration images in this way for each area shows: which of the correct areas is closest to it. Based on the estimates it is possible to verify a full image and mark as correct or not. We treat seasonality by learning models for each month, so twelve neural networks with different training sets were trained as the basis of the solution. After neural network training procedure for every labeled zone of training set statistics of predictions is collected. Finally, confidence interval is chosen and indices collected in sets such that every label in training set is recognized with given probability in assumption that the indices in every set are equal. Such an approach helps us to minimize experts’ involvement.
## Few remarks
* According to the results of testing it was found that the presence of at least one sub-area predicted as anomalous enough to classify the entire image as anomalous.
* The images are represented by discrete ocean velocity vector fields, and the possible anomalies are vector absolute values exceeding a certain threshold. 
* Data, that can contain anomalies does not have source and often obtained by chance.
* In ice concentration anomalies detection task we used the following meta-data for each sub-zone: first, automatically given label index; second, index, predicted by neural network and last, degree of certainty of neural network prediction.
