**Introduction**

Schizophrenia, a neurological disorder with a strong hereditary
component, affects about 1% of the population. It is characterized by
positive symptoms such as delusions and hallucinations, and negative
symptoms like lack of motivation. Contributing factors include social,
psychological, neurobiological, early environmental, and genetic
influences. traditional machine learning algorithms like Support Vector
Machine (SVM), Bayesian Gaussian-process logistic regression models and
discriminant analysis are commonly used for classifying neurological
disorders.

This study uses a multilayer perceptron (MLP) to show the importance of
Leave-One-Subject-Out (LOSO) cross-validation in utilising clean EEG
data for disease classification. While many studies use LOSO, the
majority do not, limiting their real-life applicability. This study aims
to objectively show the necessity of LOSO in EEG classification,
particularly for disease diagnosis and safety-critical BCI applications.

## **Data EEG Acquisition and Pre-Processing**

### **Participants**

Data from a previous study (2003-2006) involving 632 participants from
the Flinders Medical Centre was used. Thirty subjects (15 schizophrenia,
15 control) were selected, focusing on eyes-closed and maze tasks due to
data quality.

### **Data Acquisition**

EEG data was recorded using a 128-channel system at 2000 Hz with a 500
Hz low-pass filter. Data from different caps was interpolated for
consistency. Participants performed mental tasks while EEG was recorded,
including eyes-open (looking at a screen) and maze (memorising a
pathway).The current study includes data from two mental tasks: resting
eyes-open and maze. For eyes-open task, participants were looking at a
black screen with a white cross positioned one metre in front of them
with their eyes-open for 40 s.the white for fixation and for maze task
participants were asked to remember the hidden pattern of the maze.

**Artefact Removal**

Artefacts were removed using methods such as probabilistic thresholding,
kurtosis, and spectral analysis. Faulty channels were identified and
data was detrended and re-referenced..EEG was re-referenced using the
common average reference. Faulty channels were determined using three
different methods: probabilistic (threshold = 5), kurtosis (threshold =
5), and spectra (threshold = 2, 20 75 Hz). this data is referred as
clean data.

### 

### **Data-Set for Current Study**

The study included data from 30 subjects performing the eyes-open task,
resulting in 207 data points from non-overlapping epochs. Data from 45
electrodes in 5 frequency bands was used and this results in 225
features This make the data-set dimension 207\*125 . Output classes are
0 (control) and 1 (disease), creating a high-dimensional dataset. LOSO
and K-fold(9:1) cross-validation were employed for classification
experiments.

<img src="media/image4.png" style="width:5.47163in;height:1.24998in" />

## **Method**

### **Multi-Layer Perceptron**

A 4-layer MLP where nodes of each hidden layer of an MLP are connected
to every node in the previous and following layer, was used, with ReLU
activation function for hidden layers and sigmoid activation function
for the output layer. The model was trained using backpropagation, with
Adam optimizer and the loss function used was the binary-cross entropy
loss. The learning environment used Python , Keras , Tensorflow and
sklearn. Experiments were conducted with various numbers of hidden
layers, hidden units and other activation functions like softmax, tanh
and Leaky ReLU, but the results were not in any way better than that of
ReLU and Sigmoid.

**Machine Learning Method**

The study aimed to evaluate MLP effectiveness in identifying
schizophrenia and to assess the necessity of LOSO cross-validation. The
maze task data was used to test MLP and compare it with other
classifiers like Random Forest (RF), Logistic Regression (LR), Linear
Discriminant Analysis (LDA), and SVM. The training set was randomized
for each round to ensure that model is not learning in particular order
of data input.

Best performance was found with: 2 hidden and output layers with,
respectively, 12, 8 and 1 units, Adam optimizer, ReLU and sigmoid as
activation functions for hidden layers and output layer, respectively,
150 epochs and a batch size of 10.

For LOSO cross-validation, each round involved training on data from all
subjects except one, which was reserved for testing. This constituted a
200:7 ratio of training to testing data, corresponding to the cumulative
epochs of the subject data (29 for training, 1 for testing). This
process was repeated 100 times to average accuracy.

For K-fold cross-validation, data was split into 9:1 ratios for training
and testing, repeated over 100 rounds with randomization of input data
in each round.

**Results**

### **Effectiveness of MLP in Disease Classification**

Using maze task data, MLP showed promising results in distinguishing
schizophrenia subjects from controls. MLP outperformed RF and LDA and
was comparable to LR, SVM-L, and SVM-RBF, despite the small dataset.

<img src="media/image3.png" style="width:4.56771in;height:1.79825in" />

### **LOSO Versus K-Fold Cross-Validation**

Using eyes-open task data, MLP with 10-fold cross-validation achieved
98% accuracy, indicating that the model might be learning
subject-specific features rather than disease-specific features. In
contrast, LOSO cross-validation showed a significant drop in accuracy
(70%), suggesting a more realistic scenario of classifying new, unseen
subjects. Experiments with corrupted labels confirmed that k-fold
validation can be misleading in disease diagnosis studies.

## <img src="media/image2.png" style="width:4.3125in;height:1.74122in" />

## **Discussion** 

MLP proved effective in disease classification with clean EEG data,
showing significant improvement over RF and LDA. The study highlighted
the necessity of LOSO cross-validation for evaluating disease
classification methods, as k-fold can yield misleadingly high accuracies
by learning subject-specific features. Therefore, LOSO should be used
for realistic assessments of EEG-based disease classification methods.
