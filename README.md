# Evaluating Large Language Models (LLMs) in Identifying Code Smells

Ethan Diemer  
Student 
[ediemer0636@floridapoly.edu](mailto:ediemer0636@floridapoly.edu)

_Florida Polytechnic University_  
Lakeland, US

Daniel Dewar  
Student
[ddewar3190@floridapoly.edu](mailto:ddewar3190@floridapoly.edu)

_Florida Polytechnic University_  
Lakeland, US


**_Abstract_-Code smells are indicators of weakness in code that can lead to an increase in bugs and errors. Uncaught code smells can indicate the need to refactor the code to improve or further develop it. With the increased popularity and use of Large Language Models (LLMs) to write, correct, and test code, there is the question of whether LLMs can accurately identify code smells in software systems. We tested several popular LLMs against a database consisting of several types of code smells and assessed their precision, recall, and F1-score. We then compared their performance to a static analysis tool (SAT).**

**_Keywords- code smells, LLMs, SAT_**

# Introduction

The complexity and size of software have been increasing due to the increase in complexity and number of requirements. These complex requirements can be difficult to implement. Complex code can be difficult to understand and analyze, making it hard to maintain. Due to the difficulty and size of complex code, code smells are easy to accidentally implement. These code smells can make the code harder to maintain and further develop. Therefore, it is common to look for and remove code smells during and after development. One way is for people to manually look over the code for any problems. This is called a code review. It is normally done by a separate developer from the one who created the code. This can cost a lot of time and effort, depending on the code. Another common way to detect code smells is with SATs, which are programs that analyze source code to find bugs, security issues, and quality issues. They do not have the time and cost issue associated with manual review, so they are normally used for large and complex code.

SATs have been a standard way to detect code smells since they were created. They were also the most reliable method a machine could use to analyze code. However, that came into question with the introduction of LLMs. LLMs are large artificial intelligence models, trained on a vast amount of data to read and write human language. There has been a big increase in the use of LLMs to make and fix code. LLMS have been used to fix major coding problems that prevent the code from executing; however, it is not clear if LLMs can be used to detect underlying code problems.

# Literature Review

SATs work by parsing the code file and comparing it to a predefined set of rules. These rules state what the SAT should look for to find vulnerabilities, bugs, and related issues. Studies like the one conducted by M. Alqaradaghi and T. Kosik \[7\] have shown that SATs are effective in detecting code vulnerabilities and problems. SATs are not able to detect every problem, but they are able to find most.

Studies have also found that there is a correlation between warnings given by SATs and the presence of code smells \[8\]. While the study did not conclude that SATs do not directly detect code smells but they did state that code with a significant amount of warnings tends to have code smells.

S. Dewangan, R. S. Rao, A. Mishra, and M. Gupta \[1\] had applied ML models, such as Decision Tree, Logistic Regression, and Random Forest, to detect God-class, Data-class, Feature-envy, and Long-method code smells. These ML models performed well, each performing with at least an 80% accuracy in detection.

Similarly, T. Sharma, V. Efstathiou, P. Louridas, and D. Spinellis \[2\] used Deep Learning Models (DLMs), such as Convolution Neural Networks and Recurrent Neural Networks, to tackle smell detection. They were able to find that it is feasible to detect smells using deep learning models without extensive feature engineering.

Once we had a better understanding, we looked for some closely related studies that had a similar alignment with our goal. A. R. Sadik and S. Govind \[3\] evaluated the effectiveness of LLMs in detecting a vast range of code smells across a multilingual dataset, specifically OpenAI's GPT-4.0 and DeepSeek-V3. While similar, we wish to test more LLMs and see if they can perform better than our current methods for code smell detection. Also, we will be using only an English dataset as well. Finally, we are just reviewing their performance on a small set of code smells rather than a wide set. M. S. Poulaei and S. Ala\[4\] used a range of LLMs, including GPT2-large, gemma 2-9B, and mistral 7B, to detect a wide range of code smells. While closely related, we wish to use more popular and widely used models, as most people would turn to these models for code smell detection. Also, similar to the previous study, we wish to review their performance on a small set of code smells rather than a wide set.

# Data Sources

## Dataset

We will be testing the LLMs and SAT on the DacosX dataset \[5\]. These are publicly available datasets featuring a large amount of code smell samples for three smells\[6\]:

- _Multifaceted Abstraction_: This smell occurs when an abstraction, or unit of software, breaks the single responsibility principle and the elements inside are not cohesive, or are unrelated to one another.
- _Complex Method_: This smell occurs when a method has a high cyclomatic complexity, or in simpler terms, has too many paths a code can split to, leading to a more complex logic chain
- _Long Parameter List_: This smell occurs when a method accepts a long list of parameters

The reason for picking this dataset is that it was a manually and peer-reviewed dataset with 110 annotators at some phases of developing this dataset. Unlike their other dataset, DacosMain, we chose DacosX because it contained samples that are either only harmless or smelly, unlike DacosMain, which contains more ambiguous code smell cases that needed a refined set of human analysis. Also, using a manually reviewed dataset allows us to evaluate the performance of LLMs and SAT fairly. If we used a different SAT to evaluate the dataset to provide us with labels, then it would take away from the main SAT's performance while also leaving us unsure of the labels' accuracy. The dataset comprises 10 projects, each with several thousand class files and standalone method files.

## Preprocessing Labeled Dataset

First, we stripped all unnecessary labels we didn't need for this study, then merged all identified code smells under a single label of smell type, and made a new label for a boolean identification of whether an entry was a code smell or not. Then added another label indicating if an entry was a class or a method. Finally added a label for the name of the file. Next, we checked for any null entries, and there were none in this case. Finally, we checked for any duplicate entries, based on the name and file path labels, and there were some. After reviewing the duplicates to ensure there were no false positives, we removed any duplicates from the dataset. Finally, this was the labeled data set we would use to help evaluate the responses from the LLMs and SAT.

# Tools

For this study, we selected 5 popular LLMs. Using multiple models will give us a general approximation of how any LLM will perform. We are using OpenAI's GPT-5.1 Thinking, xAI Grok 4.1, Gemini 3 Pro, Claude Sonnet 4.5, and DeepSeek 3.2 DeepThink. These are the most recent versions of the most popular LLMs. They are similar but differ in training data and output data. The SAT we will be using is SonarQube. SonarQube is a popular open source SAT that works to analyse code during development and when it is committed. The performance of SonarQube will give us a baseline to evaluate the results of the LLMs. We will be using Google Co-Lab for processing files and analysis.

# Methodology

Given how most LLMs typically use an internal virtual environment for file processing, and most have a file size and count limit, the best way to perform testing would be to instruct them to create a versatile Python program that can identify code smells in a given string of a Java file.

Thus, we instructed each LLM, using a standardized prompt, to create such a versatile program capable of identifying and classifying code smells. They were told which code smells to look for in order to prevent labeling with other code smells. Since these programs are often non-deterministic, simply telling them to identify code smells and not tell them which will lead to many varying results and many added extra labels that go beyond what the original dataset identified. The output is also fixed and structured to make processing their answers streamlined and easy.

When using SonarQube, we set up a new project with default settings and quality gates. After running SonarScanner on the project files, sort the resulting issues by rule to get only relevant issues. The rule "Classes should not depend on an excessive number of classes" will be used to find Multifaceted Abstraction, "Methods should not have too many parameters" will be used to find Long Parameter List, and "Cognitive Complexity of methods should not be too high" will be used to find Complex Method.

Lastly, using Google Co-Lab, we ran the file of each project into the Python program of each LLM and recorded the response and which LLM it came from. Then, using the labels from the DacosX dataset, we compared the responses to the actual labels and collected the results. Then we analyze them to determine how good LLMs are at detecting and categorizing different types of code smells, and if the LLMs can outperform SonarQube. We will evaluate performance using precision, recall, and the F1 score using True Positives (TP), False Positives (FP), and False Negatives (FN). Within the context of our study, these metrics represent the following:

- _Precision (Predictive Accuracy)_: How accurate the LLMs/SAT's predictions were.
- _Recall (Total Accuracy)_: How many code smells were caught by the LLMs/SAT.
- _F1 (Overall Performance)_: If the LLMs/SAT caught all code smells without error.

However, our evaluation will span across 2 categories: Identification and Classification of code smells. The success or failure of a given metric will be determined by whether the LLM outperforms or approximately equals the SAT for that metric.

- For Identification
  - TP is when a file is correctly identified as having/not having a code smell.
  - FP is when a non-smelly file is identified as smelly.
  - FN is when a smelly file is identified as not smelly.
- For classification
  - TP is when a file is correctly labeled.
  - FP is when a smell type is labeled as present, but is not there.
  - FN is when a smell type is present but is not labeled or mislabeled.

Using the results, we will be able to calculate precision, recall, and the F1 score using these equations.

For Classification scores, we will compute the precision, recall, and F1 for each label, which would be for None (No Smell), Multifaceted Abstraction, Complex Method, and Long Parameter List.

Lastly, the Disparity for each metric will be calculated for each metric with the following equation:

Using this, we will calculate the disparity at each level, starting with the Metric Disparity, which is the Disparity of Precision, Recall, and F1 Score within Identification and Classification. Then, the Categorical Disparity, which would be the Disparity of the average Metric Disparity within Identification and Classification. Finally, the Overall Disparity, which is the average of all Metric Disparities. This will allow us to quantify how close the LLMs' performance is to that of the SAT, helping us interpret by how much they did or didn't outperform.

# Results

## Full Dataset Analysis

After performing the study on all files of the project, we got the following raw and performance data of each LLM:

#####

#####

#####

#####

#####

#####

#####

#####

#####

#####

#####

##### Table I - Gemini

![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Gemini%20Results.png "Gemini Results")

Fig. A

##### Table II - Grok
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Grok%20Results.png)

Fig. B


##### Table III - ChatGPT
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/ChatGPT%20Results.png)

Fig. C


##### Table IV - Claude
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Claude%20Results.png)

Fig.D


##### Table V - DeepSeek
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/DeepSeek%20Results.png)

Fig. E


## LLM vs SonarQube Results

The following data is only from the class instances of the dataset, since SonarQube was unable to analyze the standalone method files; the class files only had the Complex Method smell:

##### Table I - Gemini
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Gemini%20Results%20-%20Class%20Only.png)

Fig.F


##### Table II - Grok
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Grok%20Results%20-%20Class%20Only.png)

Fig.G


##### Table III - ChatGPT
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/ChatGPT%20Results%20-%20Class%20Only.png)

Fig.H


##### Table IV - Claude
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Claude%20Results%20-%20Class%20Only.png)

Fig.I


##### Table V - DeepSeek
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/DeepSeek%20Results%20-%20Class%20Only.png)

Fig.J


##### Table VI - SonarQube
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/SonarQube%20Results%20-%20Class%20Only.png)

Fig.K


##### Table VII - Disparity
![](https://github.com/Mangled-Night/Evaluating-LLMs-in-Identifying-Code-Smells/blob/main/Outputs/Disparity%20Results.png)

Fig.L

# Analysis

The scores for recall, precision, and F1 all range from \[0, 1\], where a value of 1 indicates high performance, while 0 is poor performance within that metric

Overall, before a deep analysis of the LLMs, we can pick apart these ideas of performance:

- In general, for the full database analysis, all LLMs performed well in identifying code smells, as reflected in their Identification scores.
- However, the perfect scores from identification must have been heavily propped up from all the method cases, as it is not as consistently high in the class-only evaluation.
- The LLMs all seemed to struggle in classifying, in both full and class-only evaluation, smelly files. Especially Multifaceted Abstraction, where nearly all LLMs had no TP cases and resulted in 0's for precision, recall, and F1 score.

## Identification

Nearly all of the LLMs had a perfect score for identification, with scores higher than 0.95 for precision, recall, and F1. Meaning, they were really great at predicting if a specific file was smelly or not, and identified nearly all of the files correctly, leading to a very great overall performance in identifying a smelly file and a non-smelly file.

However, the same cannot be said when evaluating solely the class instances; here, the scores start to vary. DeepSeek kept its high scores from the full evaluation, Claude struggled to keep it up as high, and the rest of the models fell to roughly the 0.7 range in some metrics. DeepSeek had the highest predictive accuracy at 0.94, Grok had the highest total accuracy at 0.94, and DeepSeek had the best overall performance at 0.91. ChatGPT and Grok had the worst predictive accuracy at 0.69, DeepSeek had the worst total accuracy at 0.88, and ChatGPT had the worst overall performance at 0.78

Overall, it's much different than the full database evaluation, and so the higher scores must have been the LLMs performing much better in the method cases than the class cases, and there being many more method cases to evaluate than classes, leading to a better overall score.

## Classification - None

Similarly to identification, when it came to classifying a file, all the models excelled with non-smelly files and nearly classified all of those files correctly, leading to great precision, recall, and F1 scores.

The same can almost be said for when the models evaluated the class instances only. Their score did not drop as badly as it did in Identification; their precision stayed relatively high at roughly 0.9. However, their recall fell for most except Claude and DeepSeek, falling to either 0.67 or 0.76. This led to a broader range of F1 scores from 0.77 to 0.91, still relatively high scores across the board. This leads to Grok taking the highest predictive accuracy at 0.93 and DeepSeek taking the best total accuracy and best performance overall at 0.94 and 0.91, respectively. For the ones who fell the hardest, DeepSeek had the worst predictive accuracy at 0.88, Grok had the worst total accuracy at 0.67, and ChatGPT had the worst performance at 0.77

## Classification - Complex Method

This is where the performance starts to decline when it comes to classifying a smelly file. Nearly all of the scores dropped to below 0.1 across all metrics. Gemini has the best predictive and total accuracy, along with the best performance out of all of the models, the only one achieving a score higher than 0.1 at 0.07, 0.2, and 0.1, respectively. However, it is still a very low score. DeepSeek had the worst total accuracy and overall performance at 0.03 for each. Meanwhile, Grok had the worst predictive accuracy at 0.02 and tied with DeepSeek for the worst total accuracy.

All the models had very low TP and a nearly tied amount of FP/FN cases for most LLMs, meaning when they were trying to classify a file, they were failing to correctly label a case of Complex Method and consistently mislabeled other cases as Complex Method.

Similarly, just in the full database, the performance also began to drastically fall, with most metrics falling below 0.1. However, the models were able to keep their precision roughly around 0.1. Gemini had the best predictive and total accuracy, along with the best performance at 0.12, 0.2, and 0.15, respectively. ChatGPT had the worst predictive accuracy, instead of Grok at 0.09, only by a slight margin. Everything else remains the same, with Grok having the worst performance overall at 0.04 and Grok and DeepSeek being tied for worst total accuracy at 0.03.

## Classification - Long Parameter List

The performance is still very low with classifying smelly files, with scores similar to Complex Method, although slightly better, pushing past 0.1 and even pushing past 0.2 in a few instances. This time around, Grok performed much better than in Complex Method, achieving the highest predictive and total accuracy, and overall performance, consistently reaching past 0.2 with scores at 0.31, 0.22, and 0.26. This time around, Claude had the worst scores in total accuracy and overall performance, reaching roughly 0.1, at 0.05 and 0.08, respectively, and Gemini had the worst predictive accuracy at 0.12.

Most Models had a very low amount of TP cases; however, unlike Complex Method, the cases were more skewed towards FN than FP, meaning that most of the models failed to correctly identify when a case of Complex Method was present.

## Classification - Multifaceted Abstraction

The performance here was the worst out of all of the smell types. Nearly every LLM had a precision, recall, and F1 of 0 because every model, except for ChatGPT, had a TP of 0. They were never able to correctly identify a case of Multifaceted Abstraction. Leading nearly all the models to have the same number of FN cases. As for FP, or labeling files as Multifaceted when they weren't, the biggest offenders were Grok and ChatGPT, with more FP cases than FN. However, DeepSeek hardly has any FP cases, with only 4.

This leaves ChatGPT as the best performing, but only by an extremely small margin, as it only had 2 correct cases; all other models are tied for worst performing in all measures.

## SonarQube

SonarQube had performed differently compared to the LLMs; it had greatly struggled in identifying code smells as seen in Fig. K, reaching roughly 0.54 in predictive accuracy, 0.87 in total accuracy, and 0.67 in overall performance. Then, when classifying non-smelly cases, the overall performance was mostly preserved; however, the predictive accuracy improved to 0.85, but the total accuracy fell to 0.51. We can even see that the FP and FN had even swapped when moving from identification to classification, but the TP had dropped, leading to a slightly lower performance in classifying non-smelly cases. Then, similarly to the LLMs, the metrics had fallen very low, to roughly the 0.1 - 0.3 range. Which was much better compared to the LLMs.

## Evaluation

Using the Disparities calculated in Fig. I, we are able to evaluate who performed better and to what extent they performed better. The Disparity metric ranges from \[-1, 1\] with 1 meaning LLMs were strongly performant, -1 meaning SAT were strongly performant, and 0 meaning neither was dominantly performant.

Firstly, with the _Metric Disparity of Identification_:

- In Precision, or Predictive Accuracy, LLMs had a strong performance over SonarQube, with a score of 0.41. Meaning, these LLM models, on average, were able to accurately predict more cases in identifying code smells.
- In Recall, or Total Accuracy, LLMs have a very weak, but better performance than SonarQube, with a very small margin at 0.08. Meaning, these LLM models, on average, were able to catch more code smells than SonarQube.
- In F1, or Overall Performance, LLMs had a weaker, but better performance at 0.28. Meaning, overall, the average LLM had performed better in identifying code smells than SonarQube.

Next, _Metric Disparity in Classification of Non-Smelly Cases_:

- In Precision, SonarQube had a very strong performance over the LLMs at -0.68. Meaning that, on average, the LLM had far less accuracy in predicting a correct label for Non-Smelly cases
- In Recall, SonarQube had a weaker but better performance than the LLMs at -0.32. Meaning that, on average, LLMs were less able to correctly label clean code compared to SonarQube.
- In F1, SonarQube had a strong performance over the LLMs at -0.46. Meaning, overall, the average LLM had performed worse at labeling correct instances of non-smelly cases over SonarQube.

Next, _Metric Disparity in Classification of Complex Method Cases_:

- In Precision, SonarQube had performed marginally better than LLMs at -0.09.
- In Recall, SonarQube had performed moderately better than LLMs at -0.27.
- In F1, SonarQube had also performed moderately better than LLMs at -0.15.

Next, _Categorical Disparity of Identification_:

- LLMs had performed moderately better than SonarQube at 0.26. Meaning that, as a whole, the average LLM had a better time identifying code smells than SonarQube.

Next, _Categorical Disparity of Classification_:

- SonarQube this time had performed moderately better than the LLMs at -0.20. Meaning that, as a whole, SonarQube had a better time classifying code smells than the average LLM.

Finally, the _Overall Disparity_:

- Overall, as a complete whole, in identifying and classifying code smells with the classes in the DacosX dataset. SonarQube had performed marginally better than the average LLM at -0.04.

# Limitations

## SonarQube

SonarQube is limited in what it can analyze and the rules it works on. SonarQube is not made to solely locate code smells. It also finds software vulnerabilities and provides options to improve readability. This means that it is not able to locate every code smell. There are some code smells that are closely related, and in these cases, the developers behind SonarQube opted not to detect some of these. This will not cause problems in actual use, as fixing one closely related code smell will likely fix the others. However, it does cause problems with this study as multifaceted abstraction is closely related to monster class. It is also designed to analyze a project or a connected group of code classes. This can cause problems when analyzing code files separately or when analyzing methods outside a class file. This is why we analyzed all the class files at once and did not analyze the method files.

## LLMs

Analyzing the full code dataset directly with LLMs proved extremely difficult. The dataset contained hundreds of thousands of files, and without paid API access or higher-tier model subscriptions, it was not feasible to send each file to an LLM for true, direct evaluation. Especially with the chat limits frequently placed on these models, such as Claude. Because of this limitation, we were only able to simulate how the LLMs might analyze the code, rather than have them perform their own native reasoning on the real source files.

As a result, our analysis does not fully capture the deeper interpretive abilities that LLMs typically demonstrate when allowed to read and reason about code directly. This limitation may help explain why the programs appeared to struggle with detecting Multifaceted Abstraction. That particular smell requires a more nuanced, context-heavy understanding of code structure-something that simple Python processing scripts cannot replicate. Without true LLM reasoning applied to the raw source files, identifying such complex smells becomes significantly harder.

## General

There are no clear guidelines on what qualifies as a code smell or not. What the Dracos dataset considers a code smell might not be considered one by the LLMs or SonarQube, and vice versa. However, we will be using the assumption that the Dracos dataset was marked as accurately as possible for every code smell, and anything it did not mark is not a code smell.

# Conclusion

Overall, SonarQube and all LLMs had performed very similarly. The LLMs performed well when identifying smelly code, but were not good at categorizing code smells. On average, the LLMs performed better than SonarQube in identifying code smells, while SonarQube performed better in classifying code smells. However, SonarQube is only dominant in class-based analysis, and it wasn't able to analyze standalone method files, while all the LLMs had no issues analyzing those.

In future works, we should analyze a bigger dataset with different code smells. This dataset only contained one code smell in the class files; future datasets should contain more code smells in the class and be more diverse. We could also test how well the LLMs categorize these different code smells. It would be good to test multiple SATs against the LLMs to see if they all perform better or just SonarQube. Also, we can see how the LLMs improve as better models are created by these large AI companies that are trained on more and more data to become better in code analysis and generation, and become more intelligent.

##### References

- S. Dewangan, R. S. Rao, A. Mishra, and M. Gupta, "A Novel Approach for Code Smell Detection: An Empirical Study," IEEE Access, vol. 9, pp. 162869-162883, 2021, doi: <https://doi.org/10.1109/access.2021.3133810>.
- T. Sharma, V. Efstathiou, P. Louridas, and D. Spinellis, "Code smell detection by deep direct-learning and transfer-learning," Journal of Systems and Software, vol. 176, p. 110936, Jun. 2021, doi: <https://doi.org/10.1016/j.jss.2021.110936>.
- A. R. Sadik and S. Govind, "Benchmarking LLM for Code Smells Detection: OpenAI GPT-4.0 vs DeepSeek-V3," arXiv.org, 2025. <https://arxiv.org/abs/2504.16027>
- M. S. Poulaei and S. Ala, "Code Smell Detection With LLM," Winter 9AD. <https://github.com/MSPoulaei/code-smell-detection-with-LLM>
- H. Nandani, M. Saad, and T. Sharma, "DACOS-A Manually Annotated Dataset of Code Smells," _arXiv (Cornell University)_, Jan. 2023, doi: <https://doi.org/10.48550/arxiv.2303.08729>.
- "A Taxonomy of Software Smells," _tusharma.in_. <https://tusharma.in/smells/>
- M. Alqaradaghi and T. Kosik, "Comprehensive Evaluation of Static Analysis Tools for Their Performance in Finding Vulnerabilities in Java Code," _Ieee.org_, 2025. <https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=10500698>.
- "On the Correlation Between Architectural Smells and Static Analysis Warnings," _Arxiv.org_, 2017. <https://arxiv.org/html/2406.17354v2> (accessed Dec. 02, 2025).
