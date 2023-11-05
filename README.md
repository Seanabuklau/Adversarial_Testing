# Adversarial Testing Project

## Introduction
This project aims to leverage adversarial testing on PerspectiveAPI, an online tool that uses machine learning to identify tocxic comments, making it easier to host better conversations online. The motive behind this is that while such novel tools exist to combat hate speech online, there might be inherent biases towards or against certain groups where one group might be treated more adversely than the other given the same context based on their toxicity scores provided by PerspectiveAPI. 

## Dataset
Data for this research was taken from 3 different sources. The first is the Civil Comments dataset from Kaggle which contains a variety of news comments from 2015 to 2017 that appeared on approximately 50 English-language news sites across the world. The Civil Comments dataset comprises of 1999516 rows and 24 columns that spans across categories like Gender, Religion, and Sexual Orientation. The second source is a New York Times (NYT) dataset containing comments on published content in the form of text from January 1, 2020 to December 31, 2020. The last soruce came from YouTube, taken from comments on videos under the specific categories such as “heterosexual” , “intellectual_or_learning_disability”, and "other_gender" etc. The second and third sources were added in order to supplement the Civil Comments dataset for categories that were lacking.

## Methodology

### i. Preprocessing
To perform adversarial testing on PerspectiveAPI, comments from each category were processed and categorised accordingly. Comments in each category were further split divided into sentiment classification i.e. Positive, Negative, and Neutral. Then, the category noun in each comment will be substituted with a placeholder i.e. {CATEGORY} that will be later substituted out for each intra-category. To illustrate the latter, there could be a comment “I have many Black friends” belonging to the race category and classified as Neutral. To process that comment, we will substitute the category noun “Black” with the “{RACE}” placeholder, resulting in “I have many {RACE} friends” that will be appended into a base .txt file for use later. This process will be done until all the comments in each category are in that format. In addition, a base comments dataset for each category will be generated e.g. gender_base_comments where the placeholder is substituted with an empty string. Using the same example above, the comment “I have many {RACE} friends” will become “I have many friends”. This is done to act as a control setup for each category to better study the system’s classification capability amongst the intra-categories.   

### ii. Substitution
Once the base comments for each category have been generated, each intra-category will be used to substitute the placeholder from the base comments, and then that copy of the intra-category comments will be kept for use later. For example, the Gender category has 4 sub-categories i.e. Male, Female, Transgender, Other Gender. To generate the Male comments dataset, the word “Male” will be swapped with the placeholder {GENDER} from each comment in the gender base comments to produce the Male comments dataset copy. 

### iii. Toxicity Scoring
Once each intra-category comments dataset is generated, it will undergo PerspectiveAPI’s scoring to yield the respective toxicity score for each comment. Thereafter the Min, Max, and Mean toxicity scores among the 3 classifications i.e. Positive, Negative, and Neutral across all intra-categories will be calculated and from there the results will be analysed to determine any potential biases in the algorithm. For the analysis, the fairness metric used will be Individual Fairness, where this metric is concerned with similar treatments to similar individuals or groups.

## Results and Analysis
