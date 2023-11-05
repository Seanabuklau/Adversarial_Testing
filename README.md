# Adversarial Testing Project

## Introduction

### i. PerspectiveAPI
PerspectiveAPI, developed by Jigsaw is an online tool that uses machine learning to analyze text and predict the perceived impact that it might have on a conversation. This tool is used by publishers, platforms, and individuals to identify toxic comments in places like forums and comment sections in order to make such platforms safer to host better conversations online. Some notable users include Reddit, The New York Times, The Wallstreet Journal, and The Financial Times.  

### ii. Motive
The motive behind this project is that while such novel tools exist to combat hate speech online, there might be inherent biases towards or against certain groups where one group might be treated more adversely than the other given the same context based on their toxicity scores provided by PerspectiveAPI. Thus, this project aims to evaluate any biases that may exist and recommend possible mitigations in order to improve fairness of the tool. 

## Dataset
Data for this research was taken from 3 different sources. The first is the Civil Comments dataset from Kaggle which contains a variety of news comments from 2015 to 2017 that appeared on approximately 50 English-language news sites across the world. The Civil Comments dataset comprises of 1999516 rows and 24 columns that spans across 5 categories i.e. Disability, Gender, Religion, Race, and Sexual Orientation. The second source is a New York Times (NYT) dataset containing comments on published content in the form of text from January 1, 2020 to December 31, 2020. The last soruce came from YouTube, taken from comments on videos under the specific categories such as “heterosexual” , “intellectual_or_learning_disability”, and "other_gender" etc. The second and third sources were added in order to supplement the Civil Comments dataset for categories that were lacking.

## Methodology

The main approach to test for potential biases in PerspectiveAPI is adversarial testing. This technique evaluates a machine learning model by attempting to "break" them. This is done by generating adversarial examples, which are inputs designed to fool the model into making incorrect predictions. Adversarial testing can be used to expose weaknesses in models and to make them more robust to attack.

### i. Preprocessing
To perform adversarial testing on PerspectiveAPI, comments from each category were processed and categorised accordingly. Comments in each category were further split divided into sentiment classification i.e. Positive, Negative, and Neutral. Then, the category noun in each comment will be substituted with a placeholder i.e. {CATEGORY} that will be later substituted out for each intra-category. To illustrate the latter, there could be a comment “I have many Black friends” belonging to the race category and classified as Neutral. To process that comment, we will substitute the category noun “Black” with the “{RACE}” placeholder, resulting in “I have many {RACE} friends” that will be appended into a base .txt file for use later. This process will be done until all the comments in each category are in that format. In addition, a base comments dataset for each category will be generated e.g. gender_base_comments where the placeholder is substituted with an empty string. Using the same example above, the comment “I have many {RACE} friends” will become “I have many friends”. This is done to act as a control setup for each category to better study the system’s classification capability amongst the intra-categories.   

### ii. Substitution
Once the base comments for each category have been generated, each intra-category will be used to substitute the placeholder from the base comments, and then that copy of the intra-category comments will be kept for use later. For example, the Gender category has 4 sub-categories i.e. Male, Female, Transgender, Other Gender. To generate the Male comments dataset, the word “Male” will be swapped with the placeholder {GENDER} from each comment in the gender base comments to produce the Male comments dataset copy. 

### iii. Toxicity Scoring
Once each intra-category comments dataset is generated, it will undergo PerspectiveAPI’s scoring to yield the respective toxicity score for each comment. Thereafter the Min, Max, and Mean toxicity scores among the 3 classifications i.e. Positive, Negative, and Neutral across all intra-categories will be calculated and from there the results will be analysed to determine any potential biases in the algorithm. For the analysis, the fairness metric used will be Individual Fairness, where this metric is concerned with similar treatments to similar individuals or groups.

## Results and Analysis
The following conclusions are derived from the charts and tables in sections 3.1, 3.2, and 4.1 of the notebook. 
### Category: Disability
- Mental Illness and Intellectual Disability elicited the highest toxicity scores relative to Physical Disability
- PerspectiveAPI likely more bias towards such groups based on Individual Fairness since toxicity difference was >= 0.1 when they were paired with other disability groups. 

### Category: Gender
- Transgender and Queer elicited the highest toxicity scores relative to Male, with Female just trailing behind behind the 2
- PerspectiveAPI likely more bias towards such groups based on Individual Fairness since difference was >= 0.1 when they were paired with other genders.

### Category: Race
- White and Black elicited the highest toxicity scores relative to other races
- PerspectiveAPI likely more bias towards such groups based on Individual Fairness since difference was >= 0.1 when they were paired with other races.

### Category: Religion
- Islam and Judaism elicited the highest toxicity scores relative to other religons, 
- PerspectiveAPI likely more bias towards Judaism based on Individual Fairness since difference was >= 0.1 when paired with other religions. However, Islam trails just slightly behind Judaism for this. 

### Category: Sexual Orientation
- Gay and Lesbian elicited the highest toxicity scores relative to other sexual orientations, while heterosexuals yielded the lowest score
- PerspectiveAPI likely NOT bias towards any SO groups based on Individual Fairness since difference was <= 0.1 across all pairings. Yet, looking at the absolute difference figures, Gay and Lesbian again yielded the highest difference scores when paired with the other sub-categories
