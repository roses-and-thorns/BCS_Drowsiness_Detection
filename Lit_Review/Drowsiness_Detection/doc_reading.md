This paper is about  "EEG-based cross-subject driver drowsiness recognition with an interpretable convolutional neural network". It aims to develop a calibration-free system for detecting driver drowsiness using EEG signals. Previous methods faced problems due to the very large variations in EEG signals among different subjects and sessions, making it Less accurate and challenging to identify consistent drowsiness patterns.

cross-subject drowsiness recognition across 11 subjects, outperforming traditional and state-of-the-art methods. The model achieved an average accuracy of 78.35% in leave-one-out cross-subject drowsiness recognition across 11 subjects, outperforming traditional and state-of-the-art methods. The model's ability to learn biologically meaningful features, such as Alpha spindles, which are strong indicators of drowsiness, contributed to this success.


Results

The model achieved an average accuracy of 78.35% in leave-one-out cross-subject drowsiness recognition across 11 subjects, outperforming traditional and state-of-the-art methods. The model's ability to learn biologically meaningful features, such as Alpha spindles, which are strong indicators of drowsiness, contributed to this success.

Conclusion

The paper demonstrates the potential of interpretable deep learning models in uncovering meaningful patterns from complex EEG data. The proposed InterpretableCNN not only achieves high accuracy in driver drowsiness detection but also provides valuable interpretability, which is essential for refining and validating the model. This research 
### <a name="_ynol7q1ex7go"></a>**Challenges in EEG-based Drowsiness Recognition**
EEG signals are highly variable and sensitive to noise, which leads to complication in making a generalizable model for drowsiness detection. Traditional methods, which rely on hand-crafted features, often fail to generalise across different subjects because they are trained to specific datasets or conditions. So In Solution We use Interpretable CNN. 
###
### <a name="_q7fpbepsss8x"></a><a name="_srfx7eiiw25"></a>**InterpretableCNN ->**
A Better Convolutional Neural Network (CNN) designed to handle the spatial-temporal complexity of EEG signals. This network uses depthwise separable convolutions for efficient feature extraction and includes an interpretation technique that provides sample-wise analysis of important features for classification. 

This interpretability allows for understanding the specific EEG signal features contributing to drowsiness predictions.

### <a name="_db5i1a4bd0pn"></a>**Methods-**
#### <a name="_6rk8npn9r2j8"></a>**Data Collection**
In this paper we use a public EEG dataset from 27 drivers subject  in a virtual reality driving simulation game. The dataset reflects real-world driving by including situations that make drivers feel drowsy, like very long periods of boring driving and sudden lane changes, to see how sleepy they get.
#### <a name="_ktb5ic8hf47k"></a>**Pre-processing**

We recorded EEG signals at a rate of 500 Hz using 30 electrodes. After processing the signals with bandpass filters (1-50 Hz) and running artifact rejection, we extracted 3-second EEG samples just before the occurrence of car deviation events for each trial. Subsequently, we categorized the samples based on their reaction times.

EEG data also undergoes filtering to remove noise and artifacts and is normalized to ensure consistency. For the removal of artifacts, we primarily use Independent Component Analysis (ICA) to filter out eye blinks and muscle movements. Some more methods are Common Spatial Pattern (CSP) ,Minimal Energy Combination (of noise), Maximum ContrastCombination (MCC).



The proposed processing sequence of EEG signal is similar to the concept of depthwise separable convolution, which is an operation proposed as early as 2014 and adopted in many network structures, e.g., MobileNets [29, 30].

We use pointwise convolution to implement the first processing step of demixing the signals and depthwise convolution to implement the second processing step of learning temporal features from each demixed signal independently.This interpretability allows for understanding the specific EEG signal features contributing to drowsiness predictions.


The network consists of seven layers and is composed of pointwise convolution and depthwise convolution.

- In the first layer, we use N1 pointwise convolutional nodes to generate N1 new channels of signals with the same length as the input channels. The superscripts of the outputs and network parameters indicate which network layer they belong to.
- In the second layer, depthwise convolutions are used to extract features from the N1 obtained signals. There are 2N1 channels of new signals output from the first layer.
- We consider how to trace the discriminative locations of the input sample through the four layers of the proposed network, and find that the discriminative locations remain unchanged after the 3rd and 4th layers. 
- If the input sample X(m,n) generates activation hi(k2,)jk after the 2nd layer of the network at the discriminative location (ik , jk ), the discriminative location can be traced back to the centre of the strongest contributing episode in the input signal. 

  We use the benchmark **CNN model for EEG signal classification - EEGNet -** proposed by Lawhern

  We use the **Sinc-ShallowNet** model by Davide et al.
#### <a name="_1fhczodf9nwq"></a>**Labeling** 
EEG samples are labeled based on the drivers’ reaction times to lane departure events, with faster reactions indicating alertness and slower reactions indicating drowsiness. The continuous EEG signals are segmented into epochs, each labeled accordingly.
####
<a name="_e9erym25yxm5"></a>**Feature extraction**

### <a name="_2wngfy8mjubs"></a>We implement five baseline methods for feature extraction and test them on eight different classifiers for comparison.The first three baseline methods use different forms of band power features, while the fourth baseline method uses the **wavelet entropy features**. The fifth baseline method uses a combination of four entropies features.



**Implementation** :-Initial tests showed that all deep learning models had a fast converge followed by a significant drop on the accuracies for this dataset when using the default batch normalization layer provided by both the Pytorch and TensorFlow libraries. We conducted minor modifications on all comparison models by disabling estimation of the computed mean and variance.

**Evaluation on the proposed method:**

We trained each deep learning model from 1 to 50 epochs and repeated the process for 10 times. This created 110 folds for each epoch.

The proposed model InterpretableCNN has an overall better performance than the other four models, and it reaches the peak accuracy of 78.35% in 11 epochs. The EEGNet-4,2 and EEGNet-8,2 models have similar performance, and the Sinc-ShallowNet model has better performance than the EEGNet models.

The mean classification accuracies of different conventional methods range from 53.40%-72.68%. The highest mean accuracy is achieved by LogPower+GNB, and the best accuracy for the WaveletEntropy method is achieved with the LR classifier.

We have found that the EMG activities could be a common factor that affects the classification across different subjects. The model has falsely identified several episodes containing EMG activities from peripheral EEG channels as evidence for its classification.

We found that the model learned to recognize neurologically interpretable features from EEG signals to distinguish between alert and drowsy states, and that the proposed method has advantages over conventional methods by revealing how different components in EEG signals are related to different mental states directly.

We explored the reasons behind some wrongly classified samples to understand how the cross-subject classification accuracy can be further improved. We found that the sensor noise should be eliminated, and the eye movements and EMG activities should be discriminatively treated considering they have been learned by the model as typical indicators of alertness.The model achieved an average accuracy of 78.35% in leave-one-out cross-subject drowsiness recognition across 11 subjects, outperforming traditional and state-of-the-art methods. The model achieved an average accuracy of 78.35% in leave-one-out cross-subject drowsiness recognition across 11 subjects, outperforming traditional and state-of-the-art methods. The model's ability to learn biologically meaningful features, such as Alpha spindles, which are strong indicators of drowsiness, contributed to this success.

### <a name="_1y5shbtyo0tj"></a>**Results**
The model achieved an average accuracy of 78.35% in leave-one-out cross-subject drowsiness recognition across 11 subjects, outperforming traditional and state-of-the-art methods. The model's ability to learn biologically meaningful features, such as Alpha spindles, which are strong indicators of drowsiness, contributed to this success.
### <a name="_c141jmd6yl9"></a>**Conclusion**
The paper demonstrates the potential of interpretable deep learning models in uncovering meaningful patterns from complex EEG data. The proposed InterpretableCNN not only achieves high accuracy in driver drowsiness detection but also provides valuable interpretability, which is essential for refining and validating the model. This research represents a significant advancement in EEG-based mental state recognition and the development of reliable drowsiness detection systems.


