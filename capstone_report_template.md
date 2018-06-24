# Machine Learning Engineer Nanodegree
## Capstone Project
### Rachael Mahon
June 2018

## I. Definition

### Project Overview

The intention of this project is to explore UK criminal custodial sentencing data in order to evaluate whether predictions can be made about the duration of custodial sentence based on age of the defendant, severity of the crime and time taken for the matter to be processed by the criminal courts.  

To do this, I will evaluate the performance and predictive power of a model that has been trained and tested on data made available by the British Ministry for Justice in November 2011. The Ministry released 1.2 million records of criminal sentencing data for the majority of courts in England and Wales. The dataset is anonymised but does contain the age ranges, sex and ethnicities of the subjects. It also contains the sentencing court, the type of offence and the police force who dealt with the matter. The dataset also includes, which is why I am chiefly interested, the ultimate sentence handed down to the defendant.

To achieve this stated aim, I will construct a working model which has the capability of predicting the sentence, I will separate the data into features and a target variable. I will need to convert the features and target variable into continuous values prior to training the model.

The intention of this project is not to create a method of suggested durations of sentence. Some companies have created software for the purposes of recommending sentences to judges based on algorithms which, in my opinion, violate the right to due process as defendants and their advocates are unable to scrutinise or challenge the algorithm due to it's protection by intellectual property law. Furthermore, they are based on historically sentence and are so destined to repeat the biases already ingrained in the criminal justice system.

Cathy O'Neil, author of [Weapons of Math Destruction][1], has roundly pointed out how problematic the use of these completely unknown and sealed algorithms are, for example in the area of predictive policing software. These predictive models should not be treated as neutral as they are clearly influenced by the systemically problematic underpinning data and the goals and ideology of those who create and commission them.

[1]: https://weaponsofmathdestructionbook.com/

[2]:






In this section, look to provide a high-level overview of the project in layman’s terms. Questions to ask yourself when writing this section:
- _Has an overview of the project been provided, such as the problem domain, project origin, and related datasets or input data?_
- _Has enough background information been given so that an uninformed reader would understand the problem domain and following problem statement?_




### Problem Statement

When accused of a crime, you do not have enough data to make decisions about whether you should plead guilty or go to trial, if you need to make arrangements about your employment, property or child care or whether you should appeal a sentence you feel is too harsh. There are too many unknowns and it is very difficult to get an accurate answer on these things.

If the problem is that the criminal justice system is stressful and alienating for many people being processed by it, the solution would be to use previous data to provide some insight for managing expectations or decision-making on whether or not to go to trial or to appeal a sentence. The output when this project is completed should be the capacity for someone to plug in some their personal details, the offence, the court etc, and receive a reasonably accurate sentencing figure based on a regression analysis.



In this section, you will want to clearly define the problem that you are trying to solve, including the strategy (outline of tasks) you will use to achieve the desired solution. You should also thoroughly discuss what the intended solution will be for this problem. Questions to ask yourself when writing this section:
- _Is the problem statement clearly defined? Will the reader understand what you are expecting to solve?_
- _Have you thoroughly discussed how you will attempt to solve the problem?_
- _Is an anticipated solution clearly defined? Will the reader understand what results you are looking for?_

### Metrics




In this section, you will need to clearly define the metrics or calculations you will use to measure performance of a model or result in your project. These calculations and metrics should be justified based on the characteristics of the problem and problem domain. Questions to ask yourself when writing this section:
- _Are the metrics you’ve chosen to measure the performance of your models clearly discussed and defined?_
- _Have you provided reasonable justification for the metrics chosen based on the problem and solution?_


## II. Analysis
_(approx. 2-4 pages)_

### Data Exploration

#### Overview

The data comprises of sentencing data for a courts in Wales and England. The dataset is anonymised but does contain the age ranges, sex and ethnicities of the subjects. It also contains the sentencing court, the type of offence and the police force who dealt with the matter. The dataset also includes, which is why I am chiefly interested, the ultimate sentence handed down to the defendant.

Age - categorical - Age of the defendant split into three categories: 18-24, 25-34 and 35+
Court - categorical - The Court that heard the cases
Offence Type - categorical - The category of offence split into 15 categories
Sentencing Outcome - categorical - There are a number of outcomes but we are only interested in those that resulted in the immediate custody of the defendant. All other categories, such as fines or unknowns are removed at the pre-processing stage.
Amount - categorical - The duration of the sentence split into 12 categories
Force - categorical - The Police force responsible for the cases
Sex - categorical - the sex of the defendant - male, female or not stated
Ethnicity - categorical - The Ethnicity of the defendant - white, black, Asian, unknown or other
Offence to completion - categorical - the time taken from the offence date to the sentencing date

Because the dataset contains only categorical variables, some preprocessing will be done to turn them into continuous variables.


#### Supplementary Data

I have also made use of the Crime Severity Score data tool released by the Office of National Statistics in June 2017. This data is described by the Office of National Statistics as a:
  "list of weights as a reflection of the legislation set by Parliament on behalf of the public and the courts in passing sentences in line with legislation and sentencing guidelines. It is not intended to be a pure ranking of severity of offences; it provides the basis for deriving a Severity Score rather than comparing weights for individual offences."

For each offence type, I have created an average "weight" by getting the average of all the offences listed in that category from this list. It is a crude measurement of severity of the crime given the Office of National Statistics quote above and the means by which I have gotten it but we can still use it to explore the correlation between these weights and sentence duration.

The data is available here: https://www.ons.gov.uk/peoplepopulationandcommunity/crimeandjustice/datasets/crimeseverityscoredatatool

#### Outliers

Upon exploring the data, I chose not to remove any outliers as, due to the nature of the data, it is unlikely that there are an incorrect entries in the data. I trust that the data presented by the Ministry of Justice is accurate. Furthermore, the data is mostly categorical and has been altered into pseudo-continuous variables using dictionaries. Because of this, there are big steps in the data and it is very prone to bunching around small sentences with a few very big sentences. Because of this, I do not believe that the removal of a particularly extreme sentence will be helpful. However, it is considerably effecting the results for predictions. This will be discussed further at the evaluation section.


#### The Data at a Glance

- There are 97814 records
- Minimum sentence: 2 months
- Maximum sentence: 180 months
- Mean sentence: 12.9878340524 months
- Median sentence 3.0 months
- Standard deviation: 24.8257878998
- The full dataset is 97814 rows x 9 columns


### Exploratory Visualization

TODO Insert screenshots taken from notebook

##### Visual Explorations of the Data

- Age
The age categories in which people were convicted of offences were quite evenly spread amoung the three categories but was slightly higher in the 25-34 category. It seems that convictions drop off significantly after the age of 35 as there are fewer people in the entire bracket of 35+ than there are in 25-34 and about the same as is in 18-24.

- Offence Category
There were far more people being convicted of lower level offences and the more serious the offence, the fewer convictions there were. This may mean that there were fewer of these types of offences or that they are harder to prove beyond reasonable doubt and defendants are less likely to plead guilty to them.

- Offence to Completion
Most defendants were processed by the criminal justice system relatively quickly. Most cases moved from offence to completion in less than 6 months.

- Duration of Sentence
A large amount more defendants were sent to prison for very short sentences of "up to and including 3 months." This type of sentence can be very disruptive to a persons life. This will be discussed further in the report.

- Ethnicity
A much higher proportion of ethnically white people were convicted of crimes and sentenced to custodial sentences than any other ethnicity.

- Sex
A much higher proportion of men received custodial sentences than women.

##### Linear Correlation

TODO insert the linear correlation here




### Algorithms and Techniques

Linear regression

Decision Tree Regression with one parameter but with different depths

Decision Tree Regression with multiple parameters



In this section, you will need to discuss the algorithms and techniques you intend to use for solving the problem. You should justify the use of each one based on the characteristics of the problem and the problem domain. Questions to ask yourself when writing this section:
- _Are the algorithms you will use, including any default variables/parameters in the project clearly defined?_
- _Are the techniques to be used thoroughly discussed and justified?_
- _Is it made clear how the input data or datasets will be handled by the algorithms and techniques chosen?_

### Benchmark

Due to the outputs of the linear visuals above



In this section, you will need to provide a clearly defined benchmark result or threshold for comparing across performances obtained by your solution. The reasoning behind the benchmark (in the case where it is not an established result) should be discussed. Questions to ask yourself when writing this section:
- _Has some result or value been provided that acts as a benchmark for measuring performance?_
- _Is it clear how this result or value was obtained (whether by data or by hypothesis)?_


## III. Methodology
_(approx. 3-5 pages)_

### Data Preprocessing


For the purposes of this project, we are only interested in custodial sentences ie. when a defendant was actually deprived of their liberty. This data additionally contains fine data which is also a very interesting area of study but unfortunately goes beyond the scope of this project. Removing records where the defendant was sentenced to only a fine or where the sentence duration is missing for some other reason removes a very large number of records from the dataset. While it is not ideal to reduce the dataset by this amount, the original dataset was so large as to be unwieldy on my personal machine and I would have had difficulty processing it in depth. The original data contained records and the data with fines and empty sentences removed has 97814 records.

#### Dictionaries to Convert Categorical Variables to Continuous

To convert the categorical data into continuous variables, I created a number of dictionaries which are explained below. They are crude measures but this was required to work with the data as more nuanced data is not available.

##### 1. Age Groups

This only has three distinct categories so I am using the average age of all the ages in each category. For 35+ I am assuming the top record is 65. This is an arbitrary distinction.

##### 2. Offence to Completion in Months

I wanted to retain this variable as I believe it is likely highly correlated with the severity or complexity of the crime, whether or not the defendant pleaded guilty (a longer time to completion may indicate a guilty plea and a full trial). Because of this I converted the categorical to the minimum amount in months indicated by the string.

##### 3. Offence Severity

The dataset used to get the weighting of each crime is very interesting and available here:

https://www.ons.gov.uk/peoplepopulationandcommunity/crimeandjustice/datasets/crimeseverityscoredatatool

To create the dictionary used to turn the type of crime from a distinct category into a weighted indicator of severity, I used the Crime Severity Score data tool released by the Office of National Statistics as discussed above.

For each offence type, I have created an average "weight" by getting the average of all the offences in that category in the supplementary dataset.

##### 4. Amount

Similarly, for amount, I used a dictionary created from mapping the categories to the minimum number of months to fit into the category.


#### Encoding Other Categories to Binary Variables

I assume that the dimensionality gained by retaining sex and ethnicity as features will be useful so I encoded these features to binaries variables. I think sex is very unlikely to a useful as the number of women represented in the data is almost vanishingly small. Similarly, ethnicities that are non-white are not represented very much in the data but these are significantly larger than the proportion of women. I retained these so that I could explore if my assumptions are correct.

### Implementation




I reused some code provided during the Boston Housing project earlier on in the Nanodegree. This was for the purpose of visualising the selection of the optimal hyperparameter. This was very helpful.

Discussion of bias variance trade off  




In this section, the process for which metrics, algorithms, and techniques that you implemented for the given data will need to be clearly documented. It should be abundantly clear how the implementation was carried out, and discussion should be made regarding any complications that occurred during this process. Questions to ask yourself when writing this section:
- _Is it made clear how the algorithms and techniques were implemented with the given datasets or input data?_
- _Were there any complications with the original metrics or techniques that required changing prior to acquiring a solution?_
- _Was there any part of the coding process (e.g., writing complicated functions) that should be documented?_



### Refinement

A great many things could be improved about this algorithm. I feel the project would benefit from an expanded range of features and more nuanced continuous variables which may be available as this data is supposed to be publicly available. My approach to making the variables continuous was a bit of a sledgehammer.





In this section, you will need to discuss the process of improvement you made upon the algorithms and techniques you used in your implementation. For example, adjusting parameters for certain models to acquire improved solutions would fall under the refinement category. Your initial and final solutions should be reported, as well as any significant intermediate results as necessary. Questions to ask yourself when writing this section:
- _Has an initial solution been found and clearly reported?_
- _Is the process of improvement clearly documented, such as what techniques were used?_
- _Are intermediate and final solutions clearly reported as the process is improved?_


## IV. Results
_(approx. 2-3 pages)_

### Model Evaluation and Validation
In this section, the final model and any supporting qualities should be evaluated in detail. It should be clear how the final model was derived and why this model was chosen. In addition, some type of analysis should be used to validate the robustness of this model and its solution, such as manipulating the input data or environment to see how the model’s solution is affected (this is called sensitivity analysis). Questions to ask yourself when writing this section:
- _Is the final model reasonable and aligning with solution expectations? Are the final parameters of the model appropriate?_
- _Has the final model been tested with various inputs to evaluate whether the model generalizes well to unseen data?_
- _Is the model robust enough for the problem? Do small perturbations (changes) in training data or the input space greatly affect the results?_
- _Can results found from the model be trusted?_

### Justification

The ultimate model performs much better than the bench mark models.

R2

Mean squared error



In this section, your model’s final solution and its results should be compared to the benchmark you established earlier in the project using some type of statistical analysis. You should also justify whether these results and the solution are significant enough to have solved the problem posed in the project. Questions to ask yourself when writing this section:
- _Are the final results found stronger than the benchmark result reported earlier?_
- _Have you thoroughly analyzed and discussed the final solution?_
- _Is the final solution significant enough to have solved the problem?_


## V. Conclusion
_(approx. 1-2 pages)_

### Free-Form Visualization
In this section, you will need to provide some form of visualization that emphasizes an important quality about the project. It is much more free-form, but should reasonably support a significant result or characteristic about the problem that you want to discuss. Questions to ask yourself when writing this section:
- _Have you visualized a relevant or important quality about the problem, dataset, input data, or results?_
- _Is the visualization thoroughly analyzed and discussed?_
- _If a plot is provided, are the axes, title, and datum clearly defined?_


### Reflection

While I feel that I failed spectacularly in relation to my stated aim, I am pleased with my results as I fully comprehend now the specific issues with the data and my approach. I stumbled into a number of pitfalls from the get go.


It is hard to ignore the considerable issues in this dataset. I did not fully appreciate how limiting the total absence of nuanced continuous variables would be when I decided to undertake a regression based task. I feel the data is actually much more suited to a classification based task but I was too wrapped up in my desire to solve a very specific problem rather than solving the problems the data lends itself to. I am not particularly satisfied with my handling of the categorical variables through dictionaries but I think it is the best option given the data and the task. I think if I were to write the proposal again, I would focus on a classification task.

Having said that, some other sentencing data in a similar format that may at any time be released by the Ministry of Justice or could be requested as a Freedom of Information request. A more modern dataset may have more nuance. Additionally, some other supplementary data could be very useful like the crime severity data tool from the Office of National Statistics.




In this section, you will summarize the entire end-to-end problem solution and discuss one or two particular aspects of the project you found interesting or difficult. You are expected to reflect on the project as a whole to show that you have a firm understanding of the entire process employed in your work. Questions to ask yourself when writing this section:
- _Have you thoroughly summarized the entire process you used for this project?_
- _Were there any interesting aspects of the project?_
- _Were there any difficult aspects of the project?_
- _Does the final model and solution fit your expectations for the problem, and should it be used in a general setting to solve these types of problems?_

### Improvement



As said above, I think the use of dictionaries is unsatisfactory and for an improvement, I would like to seek out other datasets that might be supplementary to this one. Perhaps around the locations of the courts from large cities, the mean household income in the city of the court, the percentage of school finishers or NEET defendants in the city of the court (ie not in education, employment or training) or any other relevant datasets.

I'd also like to explore the problem from a classification point of view perhaps predicting the category of crime or otherwise.


In this section, you will need to provide discussion as to how one aspect of the implementation you designed could be improved. As an example, consider ways your implementation can be made more general, and what would need to be modified. You do not need to make this improvement, but the potential solutions resulting from these changes are considered and compared/contrasted to your current solution. Questions to ask yourself when writing this section:
- _Are there further improvements that could be made on the algorithms or techniques you used in this project?_
- _Were there algorithms or techniques you researched that you did not know how to implement, but would consider using if you knew how?_
- _If you used your final solution as the new benchmark, do you think an even better solution exists?_

-----------

**Before submitting, ask yourself. . .**

- Does the project report you’ve written follow a well-organized structure similar to that of the project template?
- Is each section (particularly **Analysis** and **Methodology**) written in a clear, concise and specific fashion? Are there any ambiguous terms or phrases that need clarification?
- Would the intended audience of your project be able to understand your analysis, methods, and results?
- Have you properly proof-read your project report to assure there are minimal grammatical and spelling mistakes?
- Are all the resources used for this project correctly cited and referenced?
- Is the code that implements your solution easily readable and properly commented?
- Does the code execute without error and produce results similar to those reported?
