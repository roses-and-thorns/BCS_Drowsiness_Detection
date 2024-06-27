# Importance of Leave one out Cross Validation

Most neurological disorder or disease classification studies use traditional ml algorithms like SVM, logistic regression models and discriminant analysis.

All eeg systems are interpolated to map the 10-20 positioning even with diff amounts of channels (how exactly?)

The study has data from two mental tasks: resting eyes-open and maze,

The recorded eeg was detrended using SIFT(to remove major long term trends in the data)

[https://www.statology.org/detrend-data/](https://www.statology.org/detrend-data/)

and then it was re-referenced(kyuki potential difference hai to after detrending the data wapas ham koi ek channel se baaki sab ko re-reference karenge[basically difference lenge?])

then faulty channels were determined, by three methods

probabilistic(entropy-mean/standarddev exceeding a threshold)

kurtosis(fourth central moment/sd^4 exceeding a threshlod) (itna specific kyu?)

spectra(spectreal analysis using welch’s modified periodogram)(go over)

probabilistic se permanently channel nikal diye, dusre do overlysensitive hai

Artefact contaminated data fir epoch karke flag kiya if two contiguous epochs exceeded a thresholD

There were 30 subjects, 5 s non overlapping unique epochs. sab kat ke 207

45 electrodes from 5 bands(delta, theta, alpha, beta, gamma) so 255 features. Output classes are 0(control ) and 1 (disease). LOSO(29:1) was used and K-fold was used with 9:1, train, test split.

an MLP with 4 layers was used, input, 2 hidden with 12 and 8 hidden units(?) and an output layer

relu for input and hidden layers, sigmoid for output, binary cross entropy loss function and adam optimizer.

To the goal is to find the effectiveness of each method to identify schizophrenia.

### Part 1 testing the effectiveness of MLP

the maze task was used primarily kyuki uske zyada samples the.

RF, LDA, SVM-L, SVM-RBF were used for comparing the result with MLP, LOSO was used for classification.

### Part 2 LOSO vs K-Fold cross validation

clean data from eyes open task was used,

first 10 fold cross validation was used to show the accuracy with which subjects were used, ensuring ki same subject don training aur test mai ho.

secondly, yeh ensure karne ki disease features learn kar raha ho na to subject features, class labels bhi change kiya(does it really help)

### Conclusion

MLP is significantly better than RF and LDA and same as LR, SVM-L and RBF.

from k-fold eval, we see 98% accuracy if data from all subjects are used. A more realistic scenario is to classify a new unseen subject. For that case LOSO accuracy came out to be 70%, which likely means k-fold mai subject dependency bohot zyada badh gayi rather than disease condition.

to confirm this, corruption try kiya. corrupt karke k-fold ka accuracy same raha, loso ka 30% ho gaya. LOSO ke case mai standard deviation bhi high hai, considering the cases that subjects are hard to classify. so we MUST use Loso to get an accurate cross validation results.
