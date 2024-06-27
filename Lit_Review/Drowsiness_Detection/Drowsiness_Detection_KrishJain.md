# EEG-based Cross-Subject Driver Drowsiness Recognition with an Interpretable Convolutional Neural Network

## Inspiration or Idea
The basic idea or actually one of the major use case is to prevent accidents by application of these systems in cars to detect attention and brain activity of the driver

So, this paper proposes a CNN called "InterpretableCNN" for the task

## Contributions in the Past
1. We have driver monitoring systems that use cameras, LED detectors, biometric seats to monitor various aspects about the car and the driver equipped with advanced driver alert systems
2. EEG bases systems that monitor the EEG signals are much more capable. They use the increasing and decreasing trends of the signals to judge the mental state of the driver and hence, can exactly predict if the driver is even about to sleep and is trying to not.

## Interpretable CNN

### Data Preparation
1. The dataset used in the paper was collected from 27 subjects ( age : 22 to 28 ) in VR simulator.
2. Number of electrodes : 30
3. Sampling Rate : 500 Hz
4. Band Pass : 1 - 50 Hz with artifact rejection
5. Was down-sampled to 128 Hz and 3 sec length samples were taken
6. Reaction time was recorded by introducing the driver to a state of immediate drifting. Two benchmarks ( 1.5s and 2.5s ) were used for categorisation into alert and drowsy states
7. We get a balanced as well as an unbalanced dataset, consisting data from 11 people. 
8. Balanced : for training
9. Unbalanced : more realistic

### Network Design
1. Use of spatial filtering techniques to filter out artifacts completely and create a new set of EEG signals
2. We have the mathematical equation for the filteration process in the paper. We can get the weights for the filteration process by using ICA
3. We use pointwise convolution for demixing the signals and depthwise convolution for learning features from each demixed signal

### Network Structure
1. Consists of seven layers, pointwise convolution, depthwise convolution, ReLU activation, Batch Normalization,Pooling, Dense layer and at last Softmax Activation
2. At last, we have two states, alert and drowsy as the output

### Interpretation Technique
1. We have existing methods for interpreting EEG signal classification models include visualizing kernel weights, summarizing hidden unit activations, and calculating feature relevance. However, these techniques often fail to explain specific characteristics of individual EEG samples or address noise impact.
2. Class Activation Map ( CAM ) is a powerful interpretation technique. But, CAM was designed for CNNs with only standard convolution unlike our model and hence cannot be directly used.
3. FIX: we trace only those positions in the activation map that contribute the most to class activation. We then find out the corresponding discriminative locations in the input samples for the construction of the final heatmap.

### Methods for Comparison
1. Deep Learning Methods:
        
            EEGNET : 2d convolution layer, tested on BCI datasets, got two baseline models.
            Sinc-ShallowNet : sinc-convolutional layer and depthwise convolution, Conv-ShallowNet version as a more advanced model
2. Conventional Baseline Methods:

            RelativePower : uses relative band power features for comparison
            LogPower : uses natural log of the band power instead of the relative power
            PowerRatio : uses four band power ratios
            WaveletEntropy : uses wavelet entropy features from EEG signals
            FourEntropies : uses sample entropy, fuzzy entropy, approximate entropy, spectral entropy
            Classifiers : DT, RF, kNeighbours, GNB, LR, LDA, QDA and SVM
3. Variations of model:

            We create different variations by removing or altering the features of the model like convolution type, batch normalization.


### Implementation Details 
This section consists of the stats they used for implementation like the computer stats, batch size, Adam optimization parameters, etc.

### Evaluation of the model
Discusses the performance statistics of the model.
1. Mean Accuracy on Balanced Dataset: The model was trained for 1 to 50 epochs, with the training process repeated 10 times for each epoch, resulting in 110 folds per epoch.
InterpretableCNN consistently outperformed its variations, demonstrating the positive impact of separable convolution and batch normalization on the model's performance.
2. Mean Accuracy of Conventional Methods: The best performing conventional method was LogPower+GNB, achieving a mean accuracy of 72.68%.
SVM was the best classifier for RelativePower, PowerRatio, and FourEntropies.
Logistic Regression performed best with WaveletEntropy, achieving 60.40%.
Overall, band power methods generally performed better than entropy-based methods.
3. Interpretation of Learned Characteristics: The study explored the characteristics of EEG signals learned by the model, identifying Alpha spindles and Theta band bursts as indicators of drowsiness.
Beta waves from peripheral channels and EMG activities were identified as indicators of alertness.
The interpretation technique revealed that artifacts occasionally present in drowsy signals can mislead the model.