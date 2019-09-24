# Unblind Your Apps: Predicting Natural-Language Labels for Mobile GUI Components by Deep Learning

## INTRODUCTION
According to the World Health Organization(WHO), it is estimated that <b> approximately 1.3 billion people live with some form of vision impairment globally, of whom 36 million are blind.</b>
Due to their disability, engaging these minority into the society is a challenging problem.

The recent rise of smart mobile phones provides a new solution by enabling blind users' convenient access to the information and service for understanding the world. Users with vision impairment can adopt the screen reader embedded in the mobile operating systems to read the content of each screen within the app, and use gestures to interact with the phone.

However, the prerequisite of using screen readers is that developers have to add natural-language labels to the image-based components when they are developing the app. Unfortunately, <b>more than 77\% apps have issues of missing labels, according to our analysis of 10,408 Android apps.</b> Most of these issues are caused by developers' lack of awareness and knowledge in considering the minority. And even if developers want to add the labels to UI components, they may not come up with concise and clear description as most of them are of no visual issues. 

To overcome these challenges, we develop a deep-learning based model to <b> automatically predict the labels of image-based buttons by learning from large-scale commercial apps in Google Play.</b>
The experiment results show that our model can make accurate predictions and the generated labels are of higher quality than that from real Android developers.
We also submit our predicted labels of buttons of some apps to their development teams, and successfully get some positive feedback.


## Details
To achieve our goal, we first show some examples to illustrate what is a content description and how to apply labels for a component.

(1) Figure 1 shows an example of UI components and corresponding natural-language labels. For example, the label for the top-right image-based button of this UI screenshot is ''more options''

<img src="./Introduction/Figure1.png" alt="Example of UI components and labels"  width="500"/>

(2) Figure 2 shows how to set up an image-based button within the source code
<img src="./Introduction/Figure2.png" alt="Source code for setting up labels for 'add playlist' button (which is indeed a clickable ImageView)"   width="500"/>

We only focus on image-based buttons because these buttons give no hints to screen reader when developers fail to label them, while for other components, such as TextView and EditText, screen reader could read the content directly.

(3) Figure 3 gives some examples of image-based buttons, including clickable ImageView and ImageButton
<img src="./Introduction/Figure3.png" alt="Examples of image-based buttons 1:clickable ImageView; 2/3:ImageButton"   width="500"/>

## MOTIVATIONAL MINING STUDY

To investigate the severity of accessibility issues in mobile applications, we conduct a motivational mining study of <b>15,087</b> apps. Among these apps, we collected <b>394,489 GUI screenshots</b>, and 70.53% of them contain image-based buttons.</b>

As shown in Table 1, <b>77.38% of applications</b> have at least one UI component lacking labels. In details, <b>60.79% screenshots</b> have at least one UI component missing labels, which means that low-vision/blind people will meet some problems when browsing every two screen of application.

<img src="./Motivational_mining_study/Table1.png" alt="Statistics of label missing situation"  width="500" />

As seen in Figure 4, the accessibility issues exist in all categories, especially serious in PERSONALIZATION and GAME, with over 70% applications having 80%-100% components lacking lables.

<img src="./Motivational_mining_study/Figure4.png" alt="The distribution of the category of applications with different rate of image-based buttons missing content description"   width="500"/>

In addition, we plot a box-plot regarding to different download number (as seen in Figure 5). Surprisingly, there is no significant difference between applications with different download numbers. Even applications with over 50M download number have a severe accessibility problem.

<img src="./Motivational_mining_study/Figure5.png" alt="Box-plot for missing rate distribution of all apps with different download number"   width="500"/>

## APPROACH
Figure 6 shows the overview of our approach. We first encode a component image via ResNet101 model, and then feed the extracted features into Transformer encoder-decoder model and finally generate the natural-language labels for this component.

<img src="./Approach/Figure6.png" alt="Overview of our approach" />

## DATA PREPROCESSING

Preprocessing:
1. Filter duplicate xml
2. Filter duplicate elements by comparing both their screenshots and the content descriptions
3. Filter low-quality labels
(1) Labels contain the class of elments, e.g, "ImageView"
(2) Labels contain the app's name, e.g., "ringtone maker" for App Ringtone Maker
(3) Unfinished labels, e.g., "content description"

The full list of meaningless labels can be seen at [Data_preprocessing/missing_label.txt](./Dataset/meaningless_label.txt)


## DATASET

<img src="./Dataset/Figure7.png" alt="Examples of our dataset" width="500"/>

<b>Statistics of our dataset:</b>

<img src="./Dataset/Table2.png" alt="Dataset Statistics" width="500"/>

*Dataset can be download at <https://drive.google.com/open?id=18BV1oDsvEVY1xvefLe0QpGBPgpvNGY43>*

## EVALUATION
We evaluate our model in three aspects, i.e., accuracy with
automated testing, generality and usefulness with user study. We
also shows the practical value of LabelDroid by submitting the
labels to app development teams.

### Accuracy

Overall accuracy results
![Accuracy Results](Accuracy/Table3.png)

Results by different category
![Accuracy Results](Accuracy/Figure8.png)

Qualitative Performance with Baselines
![Accuracy Results](Accuracy/Table4.png)

Common causes for generation failure
![Accuracy Results](Accuracy/Table5.png)


### Generalization & Usefulness

App details and results:
<img src="Generalization&Usefulness/app_details.png" alt="The acceptability score (AS) and the standard deviation for 12 completely unseen apps. * denotes p < 0.05."/>

<img src="Generalization&Usefulness/boxplot.png" alt="Distribution of app acceptability scores by human
annotators (A1, A2, A3) and the model (M)" width="500"/>

<img src="Generalization&Usefulness/Table7.png" alt="Examples of generalization" />

[Generalization&Usefulness](https://github.com/icse2020Accessibility/icse2020Accessibility/blob/master/Generalization%26Usefulness) contains all data we used in this part and the results from model(M), developers(A1,A2,A3) and Evaluator.
