# How Do Activites of Daily Living Affect Sleep Quality?
In this pages environment, a white paper detailing the results of my own personal analysis of daily activites and sleep is displayed. Each step of the statsticall process is outline and detailed. The data used in this study was pulled from a Garmin Instinct 2 smartwatch, an I have no brand affilication with GARMIN Inc. or any of its subsidaries. Based on the results of the investigation, a basic user interface has been created at the end of this environment where the reader can input some specific metrics based on their own activity levels and the program will provide a basic recomendation, based on the white paper results, on how sleep might be improved. It is important to note at this time that these specific recomendations are based off of my own data and may or may not be applicable to all individuals.

## White Paper: How Do Activites of Daily Living Affect Sleep Quality?

### Abstract
Sleep is vitally important for proper functioning of the body and the mind. Sleep of sufficient quantity and quality is necessary for cognitive function, stable mood, and learning as well as cardiovascular, cerebrovascular, and metabolic health [1]. Unsurprisingly, insufficient sleep is associated with an increased risk of mortality and contributes to the individual risk of cardiovascular disease, diabetes, obesity, and cancer [2]. It is well established in current scientific literature that exercise promotes increased sleep efficiency and duration and suggests that sleep and exercise exert substantial positive effects on one another [3]. However, differences in exercise type and duration have variable impacts on sleep across individuals such that there is no prescriptive exercise regimen to improve sleep. This data analysis study seeks to determine what physical activities I as an individual can reasonably include into my exercise regiment for the purpose of improving my sleep. Here, using the sleep and activity data gathered from my Garmin Instinct 2 smartwatch I construct individualized insights into how to optimize my sleep by integrating specific, data backed daily activities.  This analysis involves extensive data cleaning and preprocessing to mitigate the influence of potential confounders to ensure the reliability of the findings. I developed a multivariate model that integrates key parameters of physical activity to predict sleep metrics. Through analysis of the regression results, I produce an equation that explains the dynamics of exercise and sleep quality. I have found that the intensity of physical activity, measured in normalized intensity minutes, was found to have a significant positive impact on both sleep quality (score) and duration. This suggests that engaging in more intense physical activities could potentially enhance sleep quality. However, the overall impact, as indicated by the R-squared values, was modest. Additionally, swimming was identified as a potentially beneficial activity for sleep quality. The quantity of physical activity and heart rate parameters (average and maximum) did not show a significant impact on sleep metrics when the intensity of activity was also considered. The goal is to establish evidence-based recommendations for my physical activity that are conducive to enhancing the quality of my sleep.

### Introduction
Sleep is a critical component of overall health and well-being, impacting various aspects of human life. Research has consistently shown that sufficient sleep duration and quality are fundamental for maintaining physical, mental, and emotional health. Conversely, inadequate sleep can have detrimental effects, leading to a range of health issues and impairing daily functioning.

Adequate sleep is essential for the healing and repair of heart and blood vessels, and chronic sleep deficiency is linked to an increased risk of heart disease, kidney disease, high blood pressure, diabetes, and stroke [4]. Additionally, sleep supports healthy growth and development, including muscle repair and growth hormone release [5]. Sleep also significantly impacts brain function, affecting learning, memory, decision-making, and problem-solving skills. A study by Killgore et al. (2008) demonstrated that sleep deprivation impairs cognitive performance and decision-making ability, similar to alcohol intoxication [6]. Similarly, sufficient sleep is also crucial for mental health. A lack of sleep has been linked to an increased risk of depression, anxiety, and other mental health disorders [7].

As an undergraduate student, I can see first-hand the benefits that come with a great night’s sleep characterized by both sufficient quantity and quality. As a result, this study is designed to explore the types of activities in my own daily life that can improve sleep quality and duration. Understanding the relationship between daily activities, particularly physical exercise, and sleep can provide actionable insights into lifestyle adjustments that can enhance sleep quality. This study seeks to contribute to the growing body of research emphasizing the importance of lifestyle choices in sleep health, offering practical advice for individuals looking to improve their sleep through daily activities and perhaps inform other undergraduates of steps they can take to improve their own sleep.

### Methods
#### Data Aquisition
The data was manually downloaded from the Garmin Instinct 2 Connect database which includes all the watch recorded data the is synced to the cloud. The watch is synchronized twice a day to ensure that the database contains the most up to date data storage. The watch underwent a total of 560 synchronization procedures spanning from 08/26/22 to 11/20/23 (280 days). The data pulled from the database was collated into 4 independent comma delimited data files: Activities Log, Intensity Minutes, Sleep Data, and Daily Step Count. The data in each of these files is described below:

*Activites Log*

This dataset is structured with multiple columns, each providing specific information about the activities logged. Key columns include “Activity Type”, “Date”, “Distance”, “Calories”, “Time”, “Avg HR” (Average Heart Rate), “Max HR” (Maximum Heart Rate), “Aerobic TE” (Training Effect), and several others related to performance metrics like “Avg Bike Cadence”, “Max Bike Cadence”, “Min Temp”, “Surface Interval”, and “Decompression”. The “Date” column identifies the day of the activity, while “Activity Type” specifies the kind of exercise, such as Indoor Cycling, Running, or Pool Swimming. “Distance” and “Calories” columns quantify the length of the activity in units like kilometers or miles and the calories burned, respectively. “Time” logs the duration of the activity, and heart rate metrics provide insights into cardiovascular exertion. Other columns record specific data relevant to the type of activity, like lap times and temperature conditions. . It is important to note that an endurance activity was not completed every day for the recording period and most of these recorded events occurred between 06/01/23 and 11/20/23. Additionally, it is important to mention here that each type of activity records different specific metrics, but all include time of duration and average heart rate which will become relevant during the data processing stage.

*Intensity Minutes*

This dataset is organized into two columns: “Date” and “Intensity Minutes”. The “Date” column represents each day for which the physical activity data is recorded, formatted in a month/day/year structure. The “Intensity Minutes” column quantifies the duration of intense physical activity performed by an individual on each respective day, measured in minutes. Intensity Minutes are defined by time spent above a heart rate of 125 beats per minute. In the case where a recorded endurance activity was also present on the same day the Intensity Minutes metric includes the time spent during that activity if a heart rate above 125 beats per minute was sustained.

*Sleep Data*

This dataset is structured into six primary columns: “Date”, “Score”, “Quality”, “Duration”, “Bedtime”, and “Wake Time”. Each entry under the “Date” column corresponds to a specific day for which the sleep data was recorded. The “Score” column quantitatively reflects the quality of sleep on a numerical scale, while the “Quality” column provides a descriptive evaluation of the sleep experience, categorizing it as “Good”, “Excellent”, “Poor”, or other similar descriptors. The “Duration” column details the total time spent sleeping, presented in hours and minutes. The “Bedtime” and “Wake Time” columns record the specific times at which I went to bed and woke up, respectively. This dataset is instrumental in this analysis, allowing for an in-depth examination of sleep patterns, duration, and overall quality over an extended period.

*Daily Step Count*

This dataset is formatted into three columns: “Date”, “Actual”, and “Goal”. The “Date” column notes the specific day for which the step count data is recorded. The “Actual” column quantifies the actual number of steps taken by the individual on that day. The “Goal” column represents the targeted step count goal set for each day. This dataset provides a day-by-day breakdown of physical activity in terms of steps taken, allowing for a detailed examination of the individual’s adherence to their daily step count goals.

#### Data Preprocessing
After all the data was collated and downloaded from the Garmin Instinct 2 database it was merged into a master comma-delimited spreadsheet that organized each of the data parameters by date. In relation to the activity’s parameters, data that did not provide insight into duration or heart rate was also eliminated as it is highly specific and only applicable to certain activities. There is not a great enough sample size of these such activities to draw conclusions based on these more nuanced metrics. Now that the data has been fully gathered and ready for analysis, the following sections will outline the data cleaning process to account for possible confounding variables.

#### Data Cleaning
*Eliminating Outliers*

There were days in the collected set that listed “-“ under the duration of time spent asleep. In this data set these represented days in which I was not asleep. This action aligns with the goal of the study to analyze data that reflects a more regular routine, thereby reducing potential confounding factors like social activities that might significantly skew sleep patterns. It is determined that days on which I did not sleep at all or had a total sleep duration of less than 4 hours would be outliers and removed from analysis. 62 outliers were identified and after removing these outliers, the cleaned dataset for the first analysis now consists of 414 rows and 13 columns. This dataset more accurately reflects days with regular sleep patterns, excluding those with extremely short or non-existent sleep durations.

*Quantifying Sleep Quality*

The Garmin Instinct 2 Smartwatch displays many sleep-related data parameters including a “Score”, “Quality”, “Duration”, “Bedtime”, and “Wake Time”. To simplify the analysis, the correlation between each of these parameters is analyzed. To develop a quantitative relationship, “Quality”, “Bedtime”, and “Wake Time” were converted to numerical representations and the heatmap was produced below.

![image](https://github.com/Cjcapiola/Activities-of-daily-living-and-sleep/assets/143909827/31fb1842-6ae2-4750-883f-00576a6773ef)
* Score and Quality Numeric: These two parameters have a very high positive correlation of 0.92, suggesting that as the score increases, the quality numeric tends to increase as well, and vice versa.
* Score and Duration Minutes: There is a strong positive correlation of 0.74, indicating that longer sleep duration is associated with a higher score.
* Quality Numeric and Duration Minutes: Also, a strong positive correlation of 0.74, indicating that higher quality numeric is associated with longer sleep duration.
* Bedtime Hour and Score/Quality Numeric/Duration Minutes: These correlations are weaker (0.42, 0.39, 0.43 respectively), suggesting less association between the hour one goes to bed and the other three parameters.
* Wake Time Hour and Score/Quality Numeric/Duration Minutes: The correlations are 0.21, 0.23, and 0.51 respectively. The correlation with duration of minutes is moderate, suggesting that the time one wakes up is more related to the duration of sleep than to the score or quality of sleep.
* Bedtime Hour and Wake Time Hour: There is a negative correlation of -0.28, which suggests that those who go to bed later tend to wake up later, although the relationship is not very strong.

This heatmap indicates that “Score” is strongly positively correlated with “Quality” and “Duration” which indicates that Garmin Instinct 2 uses both parameters in determining the overall sleep “Score”. There was less of or no correlation in the span of “Bedtime” or “Wake Time”. Going forward in the analysis, these parameters will be removed such that the activity of daily living data will be analyzed against “Score” and “Duration”.

*Quantifying Activity Data*

In the parameters that will eventually become the independent variables in this analysis, there is some necessary reformatting that will be necessary. Currently “Quality” is given a descriptive quality as discussed above, this variable will be reformatted to map to a numerical ranking result. “Sleep Duration” will be converted to minutes to ensure that the data is more easily mapped in a linear way. Additionally, “Intensity Minutes” and “Step Count” will be normalized to account for relative changes rather than absolute. This will make the conclusions, once drawn, easier to generalize to a new or updated data set. The python environment written to generate these new data sets is depicted below:

``` python
import pandas as pd
from sklearn.preprocessing import MinMaxScaler

def clean_data(file_path):
    # Load the dataset
    data = pd.read_excel(file_path)

    # Fill missing values for 'Activity Type' and 'Time' with 'No Activity'
    data['Activity Type'].fillna('No Activity', inplace=True)
    data['Time'].fillna('No Activity', inplace=True)

    # Convert 'Score' to numeric
    data['Score'] = pd.to_numeric(data['Score'], errors='coerce')

    # Remove rows with '--' in 'Duration' (indicating no sleep)
    data = data[data['Duration'] != '--']

    # Convert 'Duration' to total minutes
    duration_minutes = data['Duration'].str.split(' ', expand=True)
    duration_minutes[0] = duration_minutes[0].str.replace('h', '').astype(int) * 60  # Hours to minutes
    duration_minutes[1] = duration_minutes[1].str.replace('m', '').astype(int)  # Minutes
    data['Duration Minutes'] = duration_minutes[0] + duration_minutes[1]

    # Remove rows with sleep duration less than 4 hours
    data = data[data['Duration Minutes'] >= 240]

    # Normalize 'Intensity Minutes' and 'Step Count'
    scaler = MinMaxScaler()
    data[['Intensity Minutes', 'Step Count']] = scaler.fit_transform(data[['Intensity Minutes', 'Step Count']])

    return data
```

*Separating Analysis*

After looking at the complete and merged data set, it was found that there were many days that did not have a recorded activity and therefore did not have the following parameters “Activity Type”, “Time”, “Avg HR”, and “Max HR”. To try to extract the most generalizable result from this analysis, the data was split into three separate analyses:
* Analysis 1: Using more data points with less parameters.
    * Objective: Examine how step count and intensity minutes affect sleep quality and duration.
    * Method: Regression analysis can be used to model the relationship between 'Step Count', 'Intensity Minutes', and sleep metrics ('Score', 'Duration').
    * Process:
        * Define the dependent variables (e.g., 'Score', 'Duration') and independent variables ('Step Count', 'Intensity Minutes').
        * Perform regression analysis to determine the influence of physical activity on sleep metrics.
        * Evaluate the model's effectiveness through metrics like R-squared and p-values.
* Analysis 2: Using less data points with more nuanced parameters.
    * Objective: Explore the impact of detailed activity data (including heart rate) on sleep quality.
    * Method: Multivariate regression to integrate more specific parameters, including 'Avg HR', 'Max HR', alongside 'Step Count' and 'Intensity Minutes'.
    * Process:
        * Identify dependent variables (sleep metrics) and a broader range of independent variables (including heart rate data).
        * Conduct multivariate regression to understand the combined effect of these variables on sleep quality.
        * Analyze the results for significant patterns and relationships.
* Analysis 3: Effect of the presence of specific exercise on sleep.
    * Objective: Explore a general impact of recorded exercise on sleep in a qualitative fashion where the independent parameters are No Exercise, Cycling, Swimming, or Running.
    * Method: An ANOVA and a Tukey Post Hoc can also be used to determine if there is a significant difference related to the type of exercise.
    * Process:
        * Summarize the data with descriptive statistics: Mean, median, and standard deviation of sleep scores for days with and without exercise.
        * Visualize the data using box plots or histograms to compare the distribution of sleep scores on days with exercise versus days without. This will enable us to determine if the data can be approximated as a normal distribution.
        * If sleep score data is normally distributed, use a ANOVA with a Tukey Post-Hoc.

### Results
#### Results of Analysis 1: How does step count and intensity minutes correlate with sleep quality?

The results of the first analysis, using normalized values, were plotted pair wise to first assess the impact of each independent variable (Step Count and Intensity Minutes) on each of the chosen dependent variables (Score and Duration (minutes)). These plots are shown in the figure below.

![image](https://github.com/Cjcapiola/Activities-of-daily-living-and-sleep/assets/143909827/7287d677-4319-436a-81e6-502e39f428cf)
* Normalized Step Count vs. Sleep Score: This relationship saw an r-squared of 0.001 which indicates that a negligible proportion of the variance in Sleep Score is explained by step count. The coefficient was found to be -0.0253, but with a p-value of 0.602, suggesting no significant relationship.
* Normalized Step Count vs. Sleep Duration: Similarly, step count exerts a small influence on Duration with an r-squared of 0.001. No significant relationship was seen with a coefficient of -0.0194 and a p-value of 0.633.
* Normalized Intensity Minutes vs. Sleep Score: Intensity minutes showed a greater effect on Sleep Score with an r-squared of 0.037, but this is still representative of a minor contribution. However, with a coefficient of 0.1836 and a p-value less than 0.001 there was a statistically significant relationship seen.
* Normalized Intensity Minutes vs. Sleep Duration: Again, intensity minutes provided more of an explanation for sleep duration than step count, although still marginal, with a r-squared of 0.027, a coefficient of 0.1585, and a p-value of 0.001.

The analysis suggests that the normalized intensity of physical activity has a significant positive impact on both sleep quality (score) and Duration. However, the overall impact is modest, as seen from the R-squared values. On the other hand, the normalized step count does not appear to have a significant influence on either sleep quality or Duration.

With these parameters considered in isolation, the analysis can extend to a multivariate regression to try to determine if the combined effect of both independent variables exerts an actionable impact on Sleep Score or Duration. 

* Sleep Score as Dependent Variable: The r-squared for the multivariate regression was found to be 0.044 suggestion that 4.4% of the variance is explained by this new model.
    * Step Count: Coefficient = 0.0054, p-value = 0.910 (not significant).
    * Intensity Minutes: Coefficient = 0.2086, p-value < 0.001 (significant).
* Sleep Duration as Dependent Variable: The r-square for this analysis was found to be 0.031 suggesting that 3.1% of the variance is explained.
    * Step Count: Coefficient = 0.0621, p-value = 0.185 (not significant).
    * Intensity Minutes: Coefficient = 0.1675, p-value < 0.001 (significant).

The multivariate regression analysis further reinforces the earlier findings. The intensity of physical activity (measured in normalized intensity minutes) significantly influences both sleep quality and duration. In contrast, the quantity of physical activity (measured in normalized step count) does not show a significant impact on sleep metrics when the intensity of activity is also considered.

#### Results of Analysis 2: How does specific exercise affect sleep metrics?
Similarly, the more detailed parameters considered in this assessment were plotted pair wise against the sleep metrics. The details of these relationships can be seen in the following figure.

![image](https://github.com/Cjcapiola/Activities-of-daily-living-and-sleep/assets/143909827/47a93668-1506-48d6-9a29-6e5ac46dd81a)
* Average Heart Rate (Avg HR) vs. Sleep Score: With an r-square of 0.003, a very low proportion of sleep score can be explained by the average sustained heart rate during the completed exercise. A coefficient of -0.0331 and a p-value of 0.537 suggests that there is no significant relationship between average HR and sleep score.
* Maximum Heart Rate (Max HR) vs. Sleep Score: The r-squared of 0.016, still represents low explanatory power and the coefficient of -0.0824 and p-value of 0.144 displays a non-significant relationship.
* Average Heart Rate (Avg HR) vs. Sleep Duration: Minimal variance in sleep duration is explained by average sustained heart rate as evidenced by a r-squared of 0.006. The coefficient of -0.3429 and the p-vale of 0.219 also indicates a non-significant negative relationship.
* Maximum Heart Rate (Max HR) vs. Sleep Duration: An r-square of 0.012 represents a slightly higher but still minimal effect of max heart rate. The coefficient of -0.6968 and p-value represents a highly negative relationship but cannot be considered significant.

The analysis of heart rate parameters (average and maximum) in relation to sleep score and duration does not reveal any significant relationships. Both average and maximum heart rates have minimal impact on sleep quality and duration in this dataset, as indicated by low R-squared values and non-significant p-values.

Step Count and Intensity Minutes can be reintroduced into the multivariate relationship to see if all four variables considered can exert any explanatory power on the dependent variables Sleep Duration and Sleep Score.

* Sleep Score as Dependent Variable: The multivariate model produced by the inclusion of all four independent variables resulted in an r-squared of 0.042 which indicates that a very minimal variance in sleep score can be explained.
    * Avg HR: Coefficient = -0.0262, p-value = 0.806 (not significant).
    * Max HR: Coefficient = -0.0073, p-value = 0.958 (not significant).
    * Intensity Minutes: Coefficient = 0.0538, p-value = 0.039 (significant).
    * Step Count: Coefficient = -6.713×10−56.713×10−5, p-value = 0.796 (not significant).
* Sleep Duration as Dependent Variable: The model produced a r-squared of 0.077 which implies there is more of an explanatory effect on Sleep Duration rather than Sleep Score, however this effect is still minimal.
    * Avg HR: Coefficient = -0.3687, p-value = 0.673 (not significant).
    * Max HR: Coefficient = -0.0660, p-value = 0.953 (not significant).
    * Intensity Minutes: Coefficient = 0.6879, p-value = 0.001 (highly significant).
    * Step Count: Coefficient = -0.0008, p-value = 0.713 (not significant).

This suggest that while heart rate data (both average and maximum) do not significantly impact sleep metrics in this dataset, the intensity of physical activity (measured in intensity minutes) plays a more influential role. This aligns with the previous findings indicating the importance of the intensity of physical activity for better sleep quality and duration. Step count, on the other hand, remains a less significant predictor in this multivariate setup.

#### Results of Analysis 3: Does the sheer presence of specific exercise exert an effect on sleep quality?
The following descriptive statistics can be considered based on the four exercise conditions: No Exercise, Cycling, Swimming, and Running can be shown with consideration for both dependent variables important to consider for sleep quality: Score and Duration. The box-plot representation is shown below.

![image](https://github.com/Cjcapiola/Activities-of-daily-living-and-sleep/assets/143909827/3452d29d-176a-42af-8ed9-67f5f7aba7cd)
* Sleep Scores Descriptive Statistics:
    * Cycling: Mean = 79.98, Median = 82.0, Standard Deviation = 11.86
    * No Exercise: Mean = 74.73, Median = 78.0, Standard Deviation = 16.06
    * Running: Mean = 79.21, Median = 83.0, Standard Deviation = 14.67
    * Swimming: Mean = 81.56, Median = 85.0, Standard Deviation = 13.93
* Sleep Duration Descriptive Statistics:
    * Cycling: Mean = 379.11 minutes, Median = 405 minutes, Standard Deviation = 144.58
    * No Exercise: Mean = 396.35 minutes, Median = 423 minutes, Standard Deviation = 128.50
    * Running: Mean = 391.10 minutes, Median = 411 minutes, Standard Deviation = 128.61
    * Swimming: Mean = 415.56 minutes, Median = 453 minutes, Standard Deviation = 140.58

The exact results of the ANOVA and the Tukey Post-Hoc are shown below in the table comparing the treatment groups across both dependent variables.

![image](https://github.com/Cjcapiola/Activities-of-daily-living-and-sleep/assets/143909827/7e45e61e-4a0c-4fa5-b9b6-a416c5de101c)
* Sleep Score as Dependent Variable:
    * ANOVA Results Summary:
        * Sum of Squares (SS) for Groups: 2.8261x10¬¬3
        * Degrees of Freedom (df) for Groups: 3
        * Mean Square (MS) for Groups: 942.0441
        * F-value: 3.9888
        * P-value: 0.0080 (indicates there is a significant difference between two the assigned groups).
    * Tukey Post Hoc Test Results (Significant Comparisons):
        * No Exercise vs. Swimming: p-value = 0.0378 (indicates there is a significant difference between these two specific treatment groups)
        * All other comparisons have p-values greater than 0.05.

The ANOVA results for sleep score indicate that there is a statistically significant difference in sleep scores across different exercise conditions (p-value < 0.05). Specifically, the Tukey Post Hoc test suggests that days with swimming result in significantly higher sleep scores compared to days with no exercise. This implies that engaging in swimming may have a beneficial effect on sleep quality. However, no significant differences were found between no exercise and other forms of exercise (cycling and running) or between the different types of exercises themselves.

* Sleep Duration as Dependent Variable:
    * ANOVA Results Summary:
        * Sum of Squares (SS) for Groups: 2.9491x10--4
        * Degrees of Freedom (df) for Groups: 3
        * Mean Square (MS) for Groups: 9.8304x103
        * F-value: 0.5692
        * P-value: 0.6355 (indicates that there is no significant difference between any of the groups)
    * Tukey Post Hoc Test Results (All Comparisons):
        * All comparisons have p-values greater than 0.05.

The ANOVA results for sleep duration suggest that there is no statistically significant difference in sleep duration across different exercise conditions (p-value > 0.05). Therefore, based on this data, we cannot conclude that the type of exercise performed has an impact on the duration of sleep.

While the presence of specific exercise types does not significantly affect sleep duration, swimming is associated with an improvement in sleep quality (score). This suggests that incorporating swimming into the routine could be beneficial for sleep quality. However, since cycling and running did not show significant differences in sleep score or duration, they may not need to be the focus of changes aimed at improving sleep.

### Disscusion
#### Interpreting the Findings: Implications for Daily Life
The comprehensive analysis conducted in this study aimed to identify actionable steps that can be integrated into daily life to improve sleep quality. Out of all the factors assessed in this investigation, it was found that intensity minutes provided the only statistically significant effect on sleep quality and duration. This suggests that daily activities or exercises that get my heart rate sustained above 125 bpm correlate positively with better sleep outcomes. While this effect is statistically significant, it must be noted that the leverage of this variable is still quite minimal and cannot be leveraged for wholesale improvements in sleep. However, it could be used to provide a marginal improvement on sleep quality if other aspects like stress and sleep hygiene are already optimized.

Interestingly, the study found that the total step count does not significantly influence sleep quality or duration when considering the intensity of activities. This indicates that merely achieving a high step count without sufficient intensity may not be effective in improving sleep. Additionally, the type of exercise, such as cycling or running, did not show a significant difference in improving sleep quality or duration, with the exception of swimming, which showed a significant positive impact on sleep quality compared to days with no exercise. This finding suggests that incorporating swimming into regular exercise routines could be particularly beneficial for enhancing sleep quality.

Given these insights, here are some actionable steps that can be considered. First it might prove beneficial to schedule more vigorous workouts which sustain an elevated heart rate for greater periods of time. These could include activities like high-intensity interval training (HIIT), aerobic classes, or sports that require substantial physical effort. Including more swimming workouts into my weekly exercise routine may also provide a unique benefit for sleep quality potentially due to its whole-body workout nature and the relaxing effect of water.

This investigation has laid algorithmic groundwork for taking the data from a Garmin Instinct 2 smartwatch and leveraging it to provide insights into sleep quality. By taking the two actionable steps identified in this analysis, I can continue to monitor my activities and sleep and determine at some future timepoint if these steps resulted in a positive change in my sleep habits.

#### Limitations and Future Research
It's important to note that these findings and recommendations are based on individual data and may not be generalized to everyone. Future research could expand to include a more diverse sample size and explore other factors that might influence the relationship between physical activity and sleep, such as diet, stress levels, and environmental factors.

### Conclusion
In conclusion, the study underscores the importance of the intensity of physical activities in improving sleep quality and highlights swimming as a particularly beneficial exercise. While increasing step count is generally healthy, its direct impact on sleep improvement appears to be limited in the context of this study. Regular monitoring and adjustments to the exercise regimen are recommended to optimize sleep quality.

### References
[1] Ramar K, Malhotra RK, Carden KA, et al. Sleep is essential to health: an American Academy of Sleep Medicine position statement. J Clin Sleep Med. 2021;17(10):2115–2119.

[2] Luyster FS, Strollo PJ, Zee PC, Walsh JK; Boards of Directors of the American Academy of Sleep Medicine and the Sleep Research Society. Sleep: a health imperative. Sleep. 2012;35(6):727–734.

[3] Dolezal BA, Neufeld EV, Boland DM, Martin JL, Cooper CB. Interrelationship between Sleep and Exercise: A Systematic Review. Adv Prev Med. 2017;2017:1364387. doi: 10.1155/2017/1364387. Epub 2017 Mar 26. Erratum in: Adv Prev Med. 2017;2017:5979510. PMID: 28458924; PMCID: PMC5385214.

[4] Walker, M. (2017). Why We Sleep: Unlocking the Power of Sleep and Dreams. Scribner.

[5] Hirshkowitz M, Whiton K, Albert SM, Alessi C, Bruni O, DonCarlos L, Hazen N, Herman J, Katz ES, Kheirandish-Gozal L, Neubauer DN, O'Donnell AE, Ohayon M, Peever J, Rawding R, Sachdeva RC, Setters B, Vitiello MV, Ware JC, Adams Hillard PJ. National Sleep Foundation's sleep time duration recommendations: methodology and results summary. Sleep Health. 2015 Mar;1(1):40-43. doi: 10.1016/j.sleh.2014.12.010. Epub 2015 Jan 8. PMID: 29073412.

[6] Killgore WD. Effects of sleep deprivation on cognition. Prog Brain Res. 2010;185:105-29. doi: 10.1016/B978-0-444-53702-7.00007-5. PMID: 21075236.

[7] Baglioni C, Nanovska S, Regen W, Spiegelhalder K, Feige B, Nissen C, Reynolds CF, Riemann D. Sleep and mental disorders: A meta-analysis of polysomnographic research. Psychol Bull. 2016 Sep;142(9):969-990. doi: 10.1037/bul0000053. Epub 2016 Jul 14. PMID: 27416139; PMCID: PMC5110386.



## Enter Your Activity Data
<!--
<form id="activityForm">
  <label for="intensityMinutes">Intensity Minutes (per day):</label><br>
  <input type="number" id="intensityMinutes" name="intensityMinutes" required><br>

  <label for="swimming">Number of Swimming Sessions (per week):</label><br>
  <input type="number" id="swimming" name="swimming" required><br><br>

  <input type="button" value="Get Recommendations" onclick="getRecommendations()">
</form>
-->

<div id="recommendations">
  <!-- Recommendations will be displayed here -->

</div>
<script>
  function getRecommendations() {
    var intensityMinutes = document.getElementById('intensityMinutes').value;
    var swimming = document.getElementById('swimming').value;
    var recommendations = '';

    // Based on research findings
    if (intensityMinutes >= 30) {
      recommendations += 'Your intensity minutes are good. Keep it up!<br>';
    } else {
      recommendations += 'Increase your daily intensity minutes to at least 30.<br>';
    }

    if (swimming >= 2) {
      recommendations += 'Great amount of swimming sessions!';
    } else {
      recommendations += 'Consider adding more swimming sessions to your routine, at least 2 per week.';
    }

    document.getElementById('recommendations').innerHTML = recommendations;
  }
</script>
