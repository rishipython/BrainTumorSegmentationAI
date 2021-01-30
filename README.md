# Information on Project:

This project is a brain tumor segmentor that uses a 2D U-Net to segment a brain tumor in an MRI scan of a brain. 
The dataset used for this is from the Decathlon 10 Challenge. The dataset can be found at this link: https://decathlon-10.grand-challenge.org/. The dataset includes 484 244x244x155x4 MRI scans in its training set. 
Since only the training set includes labeled values, the dataset used in this project is from the training set in the imageTr folder of the Task01_BrainTumour.tar 
file. The labeled value are in the labelsTr folder, and are of dimensions 244x244x155x1. The values in the labels are either 0, 1, 2, or 3, with 0 meaning 
background, 1 meaning edema, 2 meaning non-enhancing tumor, and 3 meaning echancing tumour. During the extraction of the labels, they are one-hot-encoded into 244x244x155x4 numpy arrays. The "background" class is then removed (it is redundent, as if every other class is 0 then we know that it is background), which changes the number of classes from 4 to 3.

1. In the "Data Preproccessing" section, the code loads the dataset using nibabel.load() (in my code, nib is used as the name for nibabel). 
2. The data that is then loaded into the "images" and "labels" array are 2D slices of the 3D MRI scans where no more than 92.5% of the data is labeled as background. 
3. The data is then split into a training, validation, and testing set. 
4. The data is then normalized by subtracting the mean of the training set and dividing by the standard deviation of the training set (note that the training set is used for the 
mean and the standard deviation to prevent data leakage). The training, validation, and testing sets as well as the means and standard deviations are saved as .npy 
files. The model architecture is a 2D U-Net, which is created with the unet function. The function can create U-Nets with depths from 2 to 5. The code with the 
unet function is in the "Model Construction" section. 4 models are then created, trained, and saved. The models are trained with the soft_dice_loss function as 
its loss function and dice_coefficient as its metric. model0 has a depth of 2, model1 has a depth of 3, model2 has a depth of 4, and model3 has a depth of 5. The 
models are trained on the training set with 20 epochs and a batch size of 20. The history from the training is also stored saved in a .csv file. The different 
models are then evaluated in the "Model Evaluation" section, which evaluates the models by first plotting the history from the training, which includes soft dice 
loss on the training set per epoch, the dice coefficent on the training set per epoch, the soft dice loss on the validation set per epoch, and the dice coefficient 
on the validation set per epoch. The models are then evaluated on validation set based on the metrics of True Positives (TP), True Negatives (TN), False Positives 
(FP), Talse Negatives (FN), Accuracy, Prevalence, Sensitivity, Specificity, PPV, NPV, AUC, and F1 Score. These metrics are evaluated for each model for each class, 
and are stored in pandas DataFrames and displayed using the display function from IPython.display. The ROC curves are the plotted. The final section, "Segment MRI 
Scan", then randomly chooses an image from the validation set and displays the models prediction along with the actual value. The model currently used for 
predictions is model2 (models[2]), and can be changed by changing the index (for example, to use the prediction of model1 change models[2] to models[1]).
5. The best model is then determined based on performance on the validation set (I determined the best one to be model2) and is then evaluated on the test set.

The evaluations for each model can be found in the Google Colab Notebook for the project.

Background:
I am a high school freshman studying computer science, machine learning, and computer vision. I found this topic intriguing because a doctor's time is extremely 
valuable because that is time they could be using to save lives. There are also many places around the world that lack enough skilled doctors, and so increasing 
the efficiency of the medical process can help the doctors these places deal with more pacients. This project segments brain tumors in MRI scans of brains, and so 
could be used by doctors to more quickly analyze an MRI scan. An A.I. can also process data from all over the world extremely quickly and at consistent level of 
accuracy, whereas a doctor is restricted to being in one place, may take a long time to analyze thousands of MRI scans, and may lack consistency of accuracy due to 
things such as getting tired. This projects uses the 2D U-Net architecture to segment the MRI scan, which is a popular segmentation architecture for medical 
applications. A U-Net consists of a contraction path and an expanding path. The contraction path has a series of convolutional layers and max-pooling layers, and 
expanding path has a series of up-convolutional layers, concatination, and convolutional layers. U-Nets can have different depths. My code makes a model with a 
depth of 2 (model0), a model with a depth of 3 (model1), a model with a depth of 4 (model2), and a model with a depth of 5 (model3). 

Possible Ways This Project Could Be Improved:
- Using a pre-trained model
- Using a 3D U-Net
- Larger Batch Size
- Larger dataset
