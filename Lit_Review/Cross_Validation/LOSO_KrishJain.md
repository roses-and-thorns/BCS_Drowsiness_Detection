# LOSO ( Leave one Subject out ) Cross Validation

This paper describes about the need for LOSO cross-validation in any EEG-related disease classification task. The results show that the traditional k-fold cross-validation can be misleading for correct diagnosis due to the high variability of EEG signals between subjects. In this paper, the authors have used an MLP to demonstrate that LOSO evaluation is critical if accurate and general results, such as those on schizophrenia diagnosis, are to be derived even with clean and class-balanced EEG data.

## Introduction
It talks about Schizophrenia ( affected : nearly 1 percent population ).
That's basically a mental disorder involving delusions, hallucinations, unusual physical behaviour, and disorganised thinking and speech. People suffering from this commonly hear random voices or have paranoid thoughts. They also suffer from lack of motivation and lack of movement. It can be genetically transferred and is a lifelong disease, with no real cure.

## Data EEG Acquisition and Pre Processing
#### Participants :
1. Recruited from Flinders Medical Centre.
2. 632 participants
3. Performed 10 different tasks while recording of data
4. For the paper, data of 30 subjects was taken ( 15 having Schizophrenia and 15 from matched control group ) focusing on eyes-closed and maze tasks

#### Data Acquisition :
1. 128 channels, sampled at 2000 Hz, 500Hz low pass filter
2. To minimize electromagnetic noise, participant were seated in Faraday cage
3. The data from the Quick-cap and the Easy-cap was interpolated to be analysed together along with a linked ear reference.
4. For the eyes-open task, participants looked at a black screen with a white cross for 40 seconds.
For the maze task, participants learned and memorized a hidden path in a maze on a computer screen, using directional buttons to move the cursor.

#### Artefact Removal
1. The data was resampled from 2000 Hz to 1000 Hz.
2. The recorded EEG data was detrended using the SIFT toolbox, which is a linear piecewise method with specific time segment length and step size.
3. They found the faulty channels using three different methods : probabilistic, kurtosis, and spectra.
4. Probabilistic method was used to permanently remove faulty EEG channels, while kurtosis and spectra methods were used for subsequent artifact correction steps.

Following this we have a detailed description of the dataset in tabular form

## Method

#### MLP ( Multi-Layer Perceptron )
1. Multi-Layer Perceptron (MLP) is a basic deep learning architecture 
2. Uses back propagation for training purposes.
3. A MLP with 4 layers was used in this study. An input layer, 2 hidden layers, with 12 and 8 hidden units respectively and an output layer were used for this study. The ReLU was used as the activation function for input and hidden layers. The sigmoid was the activation function used for the output layer. The Loss function used was the binary-cross entropy and Adam as the optimizer. The learning environment used Python (3.6.6), Anaconda (5.3.0), Keras (2.2.4), Tensorflow (1.11.0) and sklearn (0.22.1). 

#### Machine Learning Method
1. The study had two parts: the first part was testing the effectiveness of MLP and the second part evaluating the necessity of LOSO cross validation.
2. The experiment aimed to identify is a new subject had Schizophrenia
3. Standardisation of EEG data by removing the mean and scaling to unit variance
4. Randomization of training set
5. Best performance was found with: 2 hidden and output layers with, respectively, 12, 8 and 1 units, Adam optimizer, ReLU and sigmoid as activation functions for hidden layers and output layer, respectively, 150 epochs and a batch size of 10.
6. First Part

         The maze task was used in the first part of the study.
         The result was confirmed using RF,LR, LDA, SVM with linear and RBF kernel.
7. Second Part

         The eyes-cosed task was used for this part.
         Aim was to determine differences in performance of a classification algorithm when k-fold cross validation and LOSO cross validation were used.
8. Confirmation or Check

         The model was trained and tested with corrupted data also to verify whether it was learning the actual disease conditon or not.
         1) subject-wise data corruption, where the whole set of labels for each subject were interchanged,and
         2) sample-wise data corruption, where individual instance labels are randomly changed.

#### Training and Testing Statistics
1. The size of training and testing data-set is approximately 200:7, equal to the cumulative number of epochs (5 to 8) of the selected subject data (29 for training 1 for testing).
2. Done for 100 epochs
3. For K-fold cross validation data samples were separated in 9:1 ratio where 90% samples (approx. 185) were used for training and 10% (approx. 20) were used for testing in each epoch
4. 100 epochs for this one too

## Results
1. MLP had a fairly better mean accuracy ( 85% ) with RF and LR still being close (82 and 81). Significant improvement as compared to LDA
2. At first, it was a big shock to see k-fold perform better, but we can observe that its accuracy was still he same with corrupted data which is a clear sign that k-fold wasnt really doing what we wanted the model to do. On the other hand, LOSO's accuracy decreased shows that whatever it was doing was correct as hence, we can say that k-fold doesnt fit for the task and use of LOSO is necessary.
