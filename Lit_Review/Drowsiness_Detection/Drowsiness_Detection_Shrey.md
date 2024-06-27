It is extremely hard to design a calibration free system because EEG signals vary significantly among test subject.

Variability comes about due to many factors like, electrode displacements, skin electrode impedance, different head shapes etc.

InterpretableCNN, doesn’t treat the model like a black box but makes the model “explain”(?) its decisions, there also also techniques for the model to reveal regions important for predictions

Past approaches include tracking the driver’s face via infrared detectors, measuring the vehicle’s position wrt to the lane, a biometric seat to monitor physiological signals, tracking the driver’s steering pattern etc.

EEG detects drowsiness at an early stage before it is reflected from the driver’s behaviour.

EEG measure the voltage difference on the scalp resulting from ion currents, systems have 1-256 electrodes following 10-20 system.

The channel name starts with one- or two-letter acronym and ends with a number or letter "z", e.g., AF3, Cz, and T4. Letter Fp, F, T, P, O, and C stand for pre-frontal, frontal, temporal, parietal, occipital and central, respectively. “z” denotes the center of coronal line. imp stuff

Drowsiness causes an increase in power of theta and alpha freq bands, although lower alpha only increases when drivers are struggling to not fall asleep, as they become drowsier alpha power decreases.

There have been attempts at one channel eeg recognition, and even multichannel recognition called EEG-Conv EEG Conv-R(conv plus deep residual learning) all of them have great accuracy but give no information as to what characteristics they have learned.

### Data Preparation

They used a dataset of 27 subjects driving in a VR simulator, where lane departure events were randomly introduced and the drowsiness level can be reflected by how fast the subjects respond to these events(accurate desc?)

Why did they downsample the data to 128 hz?

packets of 3 second length before car deviation events were taken (3 second arbitrary?)

so our sample has a dimension of 30 (channels) x 128*3(sample points)

Local-RT[L](time taken for one instance), GlobalRT[G](average of Local-RT’s in a 90 sec window before the drift) and alert RT[A] (5th percentile of local RTs) were calculated.

samples with both L and G shorter than 1.5 times A were labeled as alert(ok states)

L and G greater than 2.5 times A were labelled drowsy state. (doesn’t this also depend on driving proficiency, current mood, health etc.)

sessions with less than 50 samples were discarded, if there were multiple sessions, the session with the most balanced class distribution was taken(?)

### Network design

EEG data has noise, first we use the spatial filtering techniques by extracting a set of new signals from raw multichannel recordings.

the new set recordings(s) are a linear combination of original m signals(x’s)(?), the weights being a set of spatial filters. ICA filter jo mne mai use kiya tha, finds the weights by the statistical independency hypothesis.

Pointwise convolution matlab same depth but 1x1 receptive field, depthwise matlab we split the input and kernels into groups

depthwise separable matlab dono saath mai karna, the benefits are that it independently learn spatial correlations and cross channel correlations

pointwise conv demixes the signals and depthwise to learn temporal features

the first layer is N1 pointwise conv nodes, to generate n1 new channels(S’s)

N1 is taken to be 16,(half the input channels to reduce redundancy?)

each new h is convoluted with two nodes(?) in the second layer. odd even pe aisa lagane se kya fayda?

3rd layer relu, 4th batch normalisation

5th Global Average Pooling layer, reduces parameters

6th dense layer, and 7th softmax(h^6)

### interpretation technique

in the past for intepretation attempts have been made to visualize kernel weights, calculating single trial feature relevance, reconstructing sample that leads to maximal activations etc. inse global patterns to pata chali but har ek eeg sample mai specifically kya relevant laga woh pata nahi chala

Class Activation Map was also devised jo important regions bataye, har ek input ke liye last conv layer ke baad heatmap generate hota tha(heatmap of wut). us map ko input sample ke size ka interpolate karte the to figure out kaunse regions zyada contribute karte hai.

Mij^c is the activation map of class c(0 or 1,alert or drowsy) for sample X(m,n)

matlab the output of the kernels multiplied to the original X’s only specifically for the class c

if there are n filters in the last conv layer, there are n feature maps, for a particular class activation map is a weighted combination of all these feature maps.

to hame Mij^c ke most important(glowing)positions dhundni hai, firstly we rank the values in a decending order, lets say it gives us N positions, for each of these locations we want the center of areas the in the input sample that contribute the most.

suppose we find N locations to final heatmap sab ke gaussian function add karke milega(more or less) where sd used decides the influential area of each disc point.

this feature map Sp,q^c can also be normalised for visulation (-1 se 1)

to ab trace back kaise kare?

3-4th layer to sirf activation or normalisation layes hai, they wont affect the topology(kaunsa position kitna influence kar raha), main kaam how can we trace these locations through depthwise and pointwise conv layers.

h^2 for a position is just the weighted sum of the conv signals of all the m channels, jaha woh value maximum wohi position maximum contributing hogi(?)

sigma ko 32 liya what does the whole episode(puri timeline aa jaye) of strongest contributing signal mean?

aur N=100 liya to account for top 1% entries

### comparison

hamare model ko previous several models se compare karna padega to better understand how each component influences overall performance

DL methods:

EEGNet - 2d conv layer to filter the raw signals, spatial depthwise conv layer aur temporal depthwise conv layer

Sinc-ShallowNet - sinc conv layer(sinc functions meaning band pass filters are used as kernels of the layer), depthwise conv layers in temporal spatial seq

Conv-ShallowNet-same as sinc just with standard conv layer instead of a sinc conv layer.

Convential baseline methods:

sirf features(preconceived hai) unko extract karke classify karna

5 different baseline methods are used for extraction and 8 diff classifiers pe use kiya comparison ke ilye

Band power features have been regarded as a golden standard(hamne bhi to wohi dekha hai)

so 3 methods are relative bandpower featues, log of band power features, ratio of band power features. 4th wavelet entropy features(?) 5th uses combination of 4 entropy features

khud hi ke variations ke saath bhi compare kiya, standard 1-d conv use kiya rather than separable, then no depthwise, no pointwise, no batchnorm

### comparison results

to check efficiency we se LOSO

1.  for balanced data set: apne method ka best accuracy tha although shallow net was close, results indicate that shallow neural networks are better at capturing features than deeper ones. (lesser layers)

khud ke iterations se bhi hamara best tha, around 2% higher, NoPointwise and No Depthwise ki performance 1D Conv se same hi rahi, but well speed kam, hence separable convolution can boost the performance of the model.

Batch normalisation bhi kaafi important that without it the performance falls.

conventional baseline methods se accuracy around 53-72% tak rahi, highest being achieved by logpower +GNB?

2)in case of unbalanced dataset(more closer to reality) iske liye accuracy ke saath saath we use two other metrics precision(correctly classified drowsy samples/total drowsy classified drowsy samples) kitna sahi alert de paa raa hai and recall score(correctly classified drowsy samples/total og drowsy samples)low recall score matlab, bohot sare drowsy instances ko alert classify kar raha hai.

to well, the proposed model has the highest mean accuracy, ek subject ki low accuracy thi har ek classifier ke liye.

### insights

Drowsy signals of high confidence levels have a high portion of theta or alpha waves. There can also be bursts of theta waves(the period between you losing awareness and becoming aware again). Alpha spindles(narrow frequency peaks) are early indicator of drowsiness. Cz plays a more important role than peripheral channels, probably because it contains the cleanest signals with the least artifacts.

peripheral channels se high power beta bands se drowsi classify kiya, but they are mixtures of cortical beta waves and emg activities. Large voltage change is also taken to be an evidence of alertness, well these are caused by eye movement activities.

woh specific subject ki accuracy low isliye thi ki uske signals mai noise bohot zyada tha, to noise ko hi alert state ka evidence maan liya. Peripheral channels ki EMG activities ki wajah se bhi inaccuracy build up ho rahi thi.

matlab eye movements, blinks ko bhi classify karna seekh liya(which can also be used)

reaction time may also not faithfully reflect the actual mental states(as I said).
