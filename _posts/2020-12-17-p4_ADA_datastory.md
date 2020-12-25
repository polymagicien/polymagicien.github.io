---
layout: post
title: Are aged white male police officers really more biased than the average ?
subtitle: Study of differences in the police stop schemes among police officers in the US
authors: [mickael_rey, jonas_schweizer, samuel_thirion]
tags: [Data analysis, EPFL]
excerpt: "In May 2020, a research team from Stanford University published a paper conducting a thorough analysis of the racial disparities in police stops in the US. Starting from the same data, we tried to highlight stop schemes based of the characteristics of the police officers."
comments: true
---
 
# Starting point
## Introduction
In May 2020, a research team from Stanford University published a [paper](https://www.nature.com/articles/s41562-020-0858-1) conducting a thorough analysis of the racial disparities in police stops in the US. The paper tackles a simple question: are police officers prejudiced against minorities? Supported by a ten-years span record of police stops across all the United States, the authors highlight that there is an overall police bias against minorities, targetting black people in particular.
{: .text-justify}

The original paper studies the overall racial bias in police stops across the US. As an extension, we propose a more detailed analysis of this bias among the police forces. In particular, the study of how the characteristics of a police officer influence the stops' pattern. This could shed light on more concrete social issues: for instance, finding that the time of service increases the degree of bias of an officer could show, under careful assumptions, that the very fact of working among the police forces enhances the bias. Solution could be to re-think the typical policer career plan to try to limit this effect. Due to a low computing power and a constrained dataset, it has been decided to focus on the race, gender, age and experience (years of service) of the police officers in Florida. The raw dataset is available [here](https://openpolicing.stanford.edu/data/). 
{: .text-justify}

# Bias score method

## Designing a bias score for each officer

To identify how the personal characteristics of an officer can influence the tendency of his arrests, a first approach consisted in computing a *bias score* for each officer yearly. Two approaches were submitted and studied: 
{: .text-justify}

### First approach

The first one is simple: for an officer o and a minority m, lets define its *raw* bias score in a year as ![](/assets/post_img/p4_ADA_datastory/eq1.svg). The *bias score* as we meant it is given by subtracting the median of this scores of all the officers. This relied on the strong assumption that the median of this score is the actual proportion of the minority m in the population. As a first shot, it was doomed to fail. And it has: the linear model trained to predict this bias for each officer yielded a poor R2 and way-too-high p-values.
{: .text-justify}

### Second approach
The second approach aimed at avoiding the fact that the racial proportions in the population can be variable according to geographical location.
If in a county there is a larger proportion of Hispanic people, we would expect that there are both more Hispanic people arrested and more Hispanic police officers. This would lead to an increase in the proportion of stops of Hispanic drivers by Hispanic officers. 
To avoid this effect, each stop used to compute the previous score was adjusted with the county in which it took place.
With this modified bias score, the score of an officer is taking into account the supposed racial proportions of the counties he visited, as well as the proportion of arrests he made in each county.
{: .text-justify} 

The exact formula is: ![](/assets/post_img/p4_ADA_datastory/eq2.svg),

where x is the global proportion of stops of the minority m in the county c; p is this proportion for the officer o; and N is the number of stops made by the officer; in the county (No,c) or in general (No).
{: .text-justify}

Once again, this score is defined *towards a certain population*

## Evaluate the influence of the officer's characteristics for prediction bias

### Visualization of the differences
To determine whether a specific characteristic of an officer has an impact on his bias score, we grouped officers according to the characteristic in question and visualized the distribution of scores for each possible value of the characteristic. 
The type of features we investigated were the race, the age, or the sex of the police officer.
These visualizations were made for the scores towards each population, while comparing the parameters listed above. In order to compare the ages, officers were separated in three categories: below 32 years old, above 50 years old, in between.
Below are available the distribution of scores towards Hispanic drivers for each race of the officer, for the first score defined:
{: .text-justify}

<div style="width:100%">
{% include p4_ADA_datastory/bias1_hispanic.html %}
</div>

As well as for the second one: 

<div style="width:100%">
{% include p4_ADA_datastory/bias2_hispanic.html %}
</div>

Visually, there seems to be a difference in the distribution of scores for each officer race regarding the arrests towards hispanic drivers. We can also look at the position of each officer in the 3D space defined by the bias scores toward each population:
{: .text-justify}

<div style="width:100%">
{% include p4_ADA_datastory/3d_bias_plot.html %}
</div>

### Linear regression coefficients

After visually evaluating a potential small link, we tried to numerically find a relation between the characteristics of officers and the previously defined scores. To quantitatively estimate the relation, we fitted for each bias score a linear regression predicting the bias from the characteristics of officers. 
The coefficients of this regression could indicate how a feature impacts the scores. (positively of negatively)
However, the linear regression conducted did not appear to well fit the bias score, and the results did not show a direct link between the officer characteristics and the bias scores we defined to describe a bias in the arrests.
{: .text-justify}

Consequently, we tried to find other tools to verify if this was due to our metrics, and to see if other methods show a relation between biases and the characteristics of officers.


# Veil of darkness study


The veil of darkness is a statistical approach proposed by [Grogger and Ridgeway]("https://www.rand.org/content/dam/rand/pubs/reprints/2007/RAND_RP1253.pdf"). The idea of this method is that officers are less able to identify a driver's race after dark than during the day, because the dark would be acting like a "veil-of-darkness" of the race. If we consider a given time (let's say, 19:00), there are periods during the year when this time is in the dark (during winter, dusk is at 17:00) and other periods during which it is the day (during summer, dusk is only occurring at 22:00). Therefore, if policers are effectively doing racial profiling, the percentage of black people stopped during day periods should be higher than the percentage of black people stopped during dark periods, this for a given time.
{: .text-justify}


## Previous results and replication

The [paper]("https://www.nature.com/articles/s41562-020-0858-1") from Stanford demonstrated in its first part that there seemed to be a bias against black people, as the percentage of black people stopped after dusk was lower than the one before dusk. Our goal was then to replicate these results, to apply the method for different officer populations, in order to see if the characteristics of the officer influence the veil-of-darkness bias of the police. 
{: .text-justify}

{% include p4_ADA_datastory/basic_1800.html %}

The figure above presents, for several given times, the percentage of black people stopped against the time between the stop and the dusk on the particular day. The dataset that we used is Florida State Police (because only this state police had enough data and the fields about the characteristics of the officer that we wanted to use) , which gives slightly different results than the one of Stanford's paper. The drop in percentage of black people stopped was, according to the paper, approximately 5% in Texas, while it is only 1.5% in Florida. One simple explanation could be that population from Florida and Texas are extremely different and the afro-american community is two times more represented in proportion in Florida than in Texas. The search rates we measured in Florida, as it will be explained later, were also different from what was measured in other states by the paper from Stanford.
{: .text-justify}

## Consistency of results

One thing that we found, while digging into the *VoD* rate drops at dusk, was that the value of the rate drop (the difference between the percentage of black people stopped before dusk and after dusk) could change from simple to double according to the hour in the evening. The next figure presents the rate-drop against the time period of the arrests. This shows that, beyond the fact that the rate-drop is almost always positive (which means that black people are stopped less after dark than before), that it seems to be variable: black people seemed to be even less searched when around 20:30.
{: .text-justify}

<div style="width:100%">
{% include p4_ADA_datastory/stop_rate_drop_at_eat.html %}
</div>

A hypothesis to explain this fact could be that police officers are, particularly in the evening, very likely to arrest someone that hangs around at this time of the day. This sentiment of insecurity that people have when in the dark, late during the day, could enhance further the effects of the veil-of-darkness
{: .text-justify}


## VoD applied with different officer populations

The idea of this study, beyond just reproducing Stanford's paper, was to apply the method to reduced subsets of officers, in order see if the effects of the *VoD* are influenced by the characteristics of of officers. The next parts describe the influence of four characteristics of officers: the race, the years of experience, the age and the sex. 
{: .text-justify}

### Officer race

By splitting the stops according to the race of the officer, we are able to reproduce the *VoD* figures, one for each officer race. We kept only the tree main races presents in our data: white, black and hispanic. The following figure presents results for stops at 19:15. We noted that the percentage of black drivers stopped was, in average of before and after dusk, higher in the black officer population. This may be linked with geographic issues: black officers may be patrolling more in areas where more black are present, leading to higher percentage of black drivers stopped. However, the change between before and after dusk is drastically different: white officers tend to stop less black people after dusk, while black officers tend to stop more. Here, the hypothesis of the veil of darkness seems valid for white officers, but not at all for black people. 
{: .text-justify}

White officers seem to be subject to the veil-of-darkness bias: 
<div style="width:100%">
{% include p4_ADA_datastory/white_1915_white.html %}
</div>

Black and hispanic officers don't seem to be subject to the veil-of-darkness bias: 
<div style="width:100%">
{% include p4_ADA_datastory/black_1915_white.html %}
</div>
<div style="width:100%">
{% include p4_ADA_datastory/hispanic_1915_white.html %}
</div>


### Officer years of experience

The years of experience influence is crucial for our analysis. Indeed, showing that experienced officers are more biased could indicate that the police itself encourages (or the atmosphere of the police) a bias against black people. We found some important fluctuations in the rate drops between different hours of the evening. Therefore, we plotted the stop rate drop in average for each period of time on the next figure. 
{: .text-justify}

{% include p4_ADA_datastory/officer_yos_struct.html %}


We did not notice any importance difference between the different populations of officer. The difference between 6-10 years of experience officers and the other categories may be simply a lack of data, as we found fewer stops after 21. We can also mention that the extremities of the lines (where timeslots are near 17:00 and 21:00) are also less reliable. There are only a few times during the year where either it is dark (for 17:00, only dark in the deep winter) or it is sunny (for 21:00, only in June-July), so subtracting the mean of a big dataset and the mean of a small dataset is sometimes unstable.
{: .text-justify}


### Officer age

Similar to the years of experience, the officer age could be an important parameter. It could indicate bias due to the generation (moment when they grew up) or due to the physical/mental aspect of being old. 
{: .text-justify}

{% include p4_ADA_datastory/officer_age_struct.html %}

The influence of the age is, as the years of experience, not so obvious. The average level of drop for > 55 (in orange) seems nonetheless higher than for other age categories. Very young officers (in blue) drop rate seems unstable, which can be explained by the lack of data (not many stops written in the database by such young officers). 
{: .text-justify}

### Officer sex

Finally, as we already got the scripts, it was easy to reapply this to the sex of the officer, in order to maybe see which sex had the biggest bias against black people.
{: .text-justify}

{% include p4_ADA_datastory/officer_sex_struct.html %}

Finally, we tried to evaluate the influence of the sex of the officer. At first sight, female drop rate seems to be higher and more variable, but this is only due to lack of data, as the sex proportions in the US state police is very imbalance. 
{: .text-justify}

# Search rate overview

The veil of darkness approach gave interesting results, so we wanted to push further the study of the characteristics of the officers. As we needed police officer characteristics, we were limited to a few datasets that included these informations. Florida dataset included the ``search_conducted`` information, which allowed us to examinate the search-rates. Unfortunately, this dataset did not include any information about the outcome of the searches, so we had no way to measure the hit-rate, so no way to use advanced techniques such as threshold or hit-rate as it was done in Stanford's paper. Therefore, we studied the influence of characteristics of police officers directly on the search-rate of a population. Even with this measure, we had troubles with the global search-rate of Florida: contrary to the range of search-rates that was explained in the paper, at about 5% in several locations (Texas, California), we only measured a search-rate of 0.5% in Florida. This had the impact that even for millions of stops, only a few dozen of thousands were positive searches, so we cruelly lacked positive search stops.
{: .text-justify}


## Search rates of minorities against search rate of whites

In the case of only one population of officers, this metric gives really bad results, as search-rates can only be compared between subject populations and this is subject to many geographic and population bias. Our goal was to compare officer populations, so the situation is different, as we can measure the search-rate of given a subject population for each officer characteristic and then compare the police population together. In order to reduce the geographic bias, we plotted the minority search-rate (black or hispanic people) against the white population search-rate, each point being a county (with size of the point scaled to the number of searches that happened in this county). On top of it, we can add some linear regression, that allows to see a relationship between the white-search-rate and the minority-search-rate, and compare the relationships of each police population. We first separated the data of white, black and hispanic officers to see the search-rates evolutions for black and hispanic people. 
{: .text-justify}

### Race of the officer

The first 3 figures represent the relationship between black search-rate and white search-rate for white officers, black officers and hispanic officers. The 3 next plots show the same but applied to the hispanic population as the minority.
{: .text-justify}

{% include p4_ADA_datastory/hispanic_black_white_hispanic_black_search_rate.html %}
{% include p4_ADA_datastory/hispanic_black_white_black_search_rate.html %}

First conclusion is that general search-rates are different between the police populations. White officers seem to search more than black and hispanic officers, and this whatever the race of the subject.
{: .text-justify}

The second conclusion is that the search-rate of the minority according to the white search-rate seems to increase faster for white officers, meaning that they have a tendency to search more often the minority people. White officers seem to have an important bias against hispanic people, while all officers seem to have the same bias against black people.
{: .text-justify}

### Years of service of the officer

If we examine the same relationship but compare it between police populations with different experience, we find that more experienced police officers seem to search more the minorities (the coefficient between balck search-rate and white search-rate is higher). 
{: .text-justify}

{% include p4_ADA_datastory/old_experienced_young_black_yos_search_rate.html %}

<!-- 
## Regression to predict if a search will be conducted

Finally, despite the lack of data, we tried to design a Logistic Model, which based on the characteristics of the officer, tried to predict if the stop would end up on a search or not. We did not have a lot of time to evaluate its performances nor tuning its parameters, but we still got interesting results:
{: .text-justify}

| Characteristic    | Influence |
|-------------------|-----------|
| officer_black     | -0.56     |
| officer_white     | 0.231     |
| officer_hispanic  | 0.294     |
| officer_female    | -1.95     |
| subject_female    | -1.09     |
| officer_age_45-55 | -2.08     | -->


# Conclusion

This study showed that many bias difference can be found between different police populations. The bias methods did not give interesting results, but applying the veil-of-darkness method to subsets of the police population enlightened bias within police populations. We regret the fact that most of the police data sources do not have the characteristics of officers that made the stop, which could be very useful for a large-scale analysis. As many data sources include hash ids of officers, they could implement easily a pipeline to give characteristics of officers for each stop, in order to have more datapoints. If we wanted to draw concrete conclusions about bias within the police, we would need less variability in the results so the actions could be adapted to the needs of each police department. We offer, in this study, tools that could be easily run on larger-scale datasets. Feel free to use them in order to spot sources of bias against minorities in the police and take action to improve the fairness of police interactions!
{: .text-justify}
