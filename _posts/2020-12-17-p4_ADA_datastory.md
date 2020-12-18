---
layout: post
title: Are aged white male police officers really more biased than the average ?
subtitle: Study of differences in the police stop schemes among police officers in the US
tags: [Data analysis, EPFL]
comments: true
---

# Example part
## Instructions
- This is a [link](https://www.youtube.com/watch?v=dQw4w9WgXcQ&list=PLahKLy8pQdCM0SiXNn3EfGIXX19QGzUG3&ab_channel=RickAstleyVEVO)
- Images have to be stored in ``assets\img\p4_ADA_datastory\``
- html includes (interactive plots, ...) have to be stored in ``_includes\p4_ADA_datastory\``
- Formulas : use [this site](https://latex.codecogs.com/eqneditor/editor.php) to generate a svg 10px and put it ``assets\img\p4_ADA_datastory\``. Include it as an image
- Once pushed, the result can be seen [here](https://polymagicien.github.io/2020-12-17-p4_ADA_datastory/) after ~5 minutes

## Examples
![](/assets/post_img/p4_ADA_datastory/eq1.svg) 
![](/assets/post_img/p4_ADA_datastory/EPFL.png){: width="10%"}

# Graphs to put




# A starting point
## Introduction
In May 2020, a research team from Stanford university published a [paper](https://www.nature.com/articles/s41562-020-0858-1) conducting a thorough analysis of the racial disparities in police stops in the US. The paper tackles a simple question: are police officers prejudiced against minorities ? Supported by a ten-year span record of police stops accross all united states, the authors highlight that there is an overall police bias against minorities, targetting black people in particular.

Yet, questions remain: how does these results evolve when the scale changes (statewide or countywide) ? Do they

The original paper studies the overall racial bias in the police stops across the US. As an extension, we propose a more detailed analysis of this bias among the police forces. In particular, the study of how the characteristics of a police officer  influence this bias. This could shed light on more concrete social issues : for instance, finding that the time of service increases the degree of bias of an officer could show, under careful assumptions, that the very fact of working among the police enhances the bias. Solution could be to re-think the typical policer career plan to try to limit this effect. Due to a low computing power and a constrained dataset, it has been decided to focus on the race, gender, age and experience (years of service) of the police officers in Florida. The raw dataset is available [here](https://openpolicing.stanford.edu/data/).{: .text-justify}

- main ideas 
- limits of paper
- questions we ask

## A first insight of the data
1. Introduce the data (talk + plots)
   1. Plot : bar plot with repartition sex\|ethnicity\|age
   2.  ?



# Bias score method

## Designing a bias score for each officer

To identify how the personnal characteristics of an officer can influence the tendency of his arrests, a first approach consisted in computing a *bias score* for each officer yearly. Two approaches were submitted and studied: 

### First approach

The first one is simple: for an officer o and a minority m, lets define its *raw* bias score on a year as ![](/assets/post_img/p4_ADA_datastory/eq1.svg). 
This scores is in fact the proportion in the stops made by an officer that concerned a type of population. 
As a result, this score is defined as a bias score *towards a certain population*. In order to have enough data, the only populations used for computing this score were the same ones as in the original paper : *white*, *black*, and *hispanic*. 
The median of the scores is substracted in the end to obtain a relative score.

### Second approach
<div style="text-align: justify"> 
The second approach aimed at avoiding the fact that the racial proportions in the population can be variable according to geographical location.
If in a county there is a larger proportion of hispanic people, we would expect that there are both more hispanic people arrested and more hispanic police officers. This would lead to an increase in the proportion of stops of hispanic drivers by hispanic officers. 
To avoir this effect, each stop used to compute the previous score was adjusted with the county in which it took place.
With this modified bias score, the score of an officer is taking into acount the supposed racial proportions of the counties he visited, as well as the proportion of arrests he made in each county. </div>
The exact formula is : ![](/assets/post_img/p4_ADA_datastory/eq2.svg),
<div style="text-align: justify"> 
where x is the global proportion of stops of the minority m in the county c; p is this proportion for the officer o; and N is the number of stops made by the officer; in the county  (No,c) or in general (No). </div>

Once again, this score is defined *towards a certain population*. 

## Evaluate the influence of the officer's characteristics for prediction bias

### Visualization of the differences
<div style="text-align: justify"> 
To determine whether a specific characteristic of an officer has an impact on his bias score, we grouped officers according to the characteristic in question, and visualized the distribution of scores for each possible value of the characteristic. 
The type of features we investigated were the race, the age, or the sex of the police officer.
These visualizations were made for the scores towards each population, while comparing the parameters listed above. In order to compare the ages, officers were separated in 3 categories : below 32 years old, above 50 years old, in between.
Below are available the distribution of scores towards hispanic drivers for each race of officer, for the first score defined:
</div>

<div style="width:100%">
{% include p4_ADA_datastory/bias1_hispanic.html %}
</div>

As well as for the second one: 

<div style="width:100%">
{% include p4_ADA_datastory/bias2_hispanic.html %}
</div>

<div style="text-align: justify"> 
Visualy, there seems to be a difference in the distribution of scores for each officer race regarding the arrests towards hispanic drivers. We can also look at the position of each officer in the 3D space defined by the bias scores toward each population:
</div>

<div style="width:100%">
{% include p4_ADA_datastory/3d_bias_plot.html %}
</div>

### Linear regression coefficients

<div style="text-align: justify"> 
After visualy evaluating a potential small link, we tried to numericaly find a relation between the characteristics of officers and the previously defined scores. To quantitatively estimate the relation, we fited for each bias score a linear regression predicting the bias from the characteristics of officers. 
The coefficients of this regression could indicate how a feature impacts the scores.
</div>

# Veil of darkness study

<div style="text-align: justify"> 
The veil of darkness is a statistical approach proposed by <a href="https://www.rand.org/content/dam/rand/pubs/reprints/2007/RAND_RP1253.pdf">Grogger and Ridgeway</a>. The idea of this method is that officers are less able to identify a driver's race after dark than during the day, because the dark would be acting like a "veil-of-darkness" of the race. If we consider a given time (let's say, 19:00), there are periods during the year when this time is in the dark (during winter, dusk is at 17:00) and other periods during which it is the day (during summer, dusk is only occuring at 22:00). Therefore, if policers are effectively doing racial profiling, the percentage of black people stopped during day periods should be higher than the percentage of black people stopped during dark periods, this for a given time.
</div> 


## Previous results and replication

<div style="text-align: justify"> 
The <a href="https://www.nature.com/articles/s41562-020-0858-1">paper</a> from Stanford demonstrated in its first part that there seemed to be a bias against black people, as the percentage of black people stopped after dusk was lower than the one before dusk. Our goal was then to replicate these results, to apply the method for different officer populations, in order to see if the characteristics of the officer influence the veil-of-darkness bias of the police. 
</div>

<div style="width:100%">
{% include p4_ADA_datastory/basic_1800.html %}
</div>

The figure above presents, for several given times, the percentage of black people stopped against the time between the stop and the dusk on the particular day. The dataset that we used is Florida State Police (because only this state police had enough data and the fields about the characteristics of the officer that we wanted to use), with gives slightly different results than the one of Stanford's paper. The drop in percentage of black people stopped was, acording to the paper, approximately 5% in Texas, while it is only 1.5% in Florida. One simple explanation could be that population from Florida and Texas are extremely different and the afro-american community is two times more reprensented in proportion in Florida than in Texas. The search rates we measured in Florida, as it will be explained later, were also different from what was measured in other states by the paper from Stanford.

## Consistency of results

One thing that we found, while digging into the vod rate drops at dusk, was that the value of the rate drop (the difference between the percentage of black people stopped before dusk and after dusk) could change from simple to double according to the hour in the evening. The next figure presents the rate-drop against the time period of the arrests. This shows that, beyond the fact that the rate-drop is almost always positive (which means that black people are stopped less after dark than before), that it seems to be variable: black people seemed to be even less searched when around 20:30.

<div style="width:100%">
{% include p4_ADA_datastory/stop_rate_drop_at_eat.html %}
</div>

A hypothesis to explain this fact could be that police officers are, particularly in the evening, very likely to arrest someone that hangs around at this time of the day. This sentiment of insecurity that people have when in the dark, late during the day, could enhance further the effects of the veil-of-darkness


## VoD applied with different officer populations

The idea of this study, beyond just reproducing Stanford's paper, was to apply the method to reduced subsets of officers, in order see if the effects of the VoD are influenced by the characteristics of of officers. The next parts describe the influence of 4 characteristics of officers: the race, the years of experience, the age and the sex. 

### Officer race

By splitting the stops according to the race of the officer, we are able to reproduce the VoD figures, one for each officer race. We kept only the 3 main races presents in our data: white, black and hispanic. The following figure presents results for stops at 19:15. We noted that the percentage of black driver stopped are, in average of before and after dusk, higher in the black officer population. This may be linked with geographic issues: black officers may be patrolling more in areas where more black are present, leading to higher percentage of black drivers stopped. However, the change between before and after dusk is drastically different: white officers tend to stop less black people after dusk, while black officers tend to stop more. Here, the hypothesis of the veil of darkness seems valid for white officers, but not at all for black people. 

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

The years of experience influence is crutial for our analysis. Indeed, showing that experienced officers are more biased could indicate that the police itself encourages (or the atmosphere of the police) a bias against black people. We found some important fluctuations in the rate drops between different hours of the evening. Therefore, we plotted the stop rate drop in average for each period of time on the next figure. 


<div style="width:100%">
{% include p4_ADA_datastory/officer_yos_struct.html %}
</div>

We did not motice any importance difference between the different populations of officer. The difference between 6-10 years of experience officers and the other categories may be simply a lack of data, as we found less stops after 21. 


### Officer age

Similar to the years of experience, the officer age could be an important parameter. It could indicate bias due to the generation (moment when they grew up) or due to the physical/mental aspect of being old. 


<div style="width:100%">
{% include p4_ADA_datastory/officer_age_struct.html %}
</div>

<!-- We did not notice a real  fDO THE EXPLICATION -->


### Officer sex

Finally, as we already got the scripts, it was easy to reapply this to the sex of the officer, in order to maybe see which sex had the biggest bias against black people.


<div style="width:100%">
{% include p4_ADA_datastory/officer_sex_struct.html %}
</div>

<!--  fDO THE EXPLICATION -->


# Search rate overview

The veil of darkness approach gave interesting results, so we wanted to push further the study of the characteristics of the officers. As we needed police officer characteristics, we were limited to a few datasets that included these informations. Florida dataset included the ``search_conducted`` information, which allowed us to examinate the search-rates. Unfortunately, this dataset did not include any information about the outcome of the searches, so we had no way to measure the hit-rate, so no way to use advanced techniques such as threshold or hit-rate as it was done in Stanford's paper. Therefore, we studied the influence of characteristics of police officers directly on the search-rate of a population. Even with this measure, we had troubles with the global search-rate of Florida: contrary to the range of search-rates that was explained in the paper, at about 5% in several locations (Texas, California), we only measured a search-rate of 0.5% in Florida. This had the impact that even for millions of stops, only a few dozen of thousand were positive searches, so we crually lacked positive search stops. 


## Search rates of minorities against search rate of whites

In the case of only one population of officers, this metrix gives really bad results, as search-rates can only be compared between subject populations and this is subject to many geographic and population bias. Our goal was to compare officer populations, so the situation is different, as we can measure the search-rate of given a subject population for each officer characteristic and then compare the police population together. In order to reduce the geographic bias, we plotted the minority search-rate (black or hispanic people) against the white population search-rate, each point being a county (with size of the point scaled to the number of searches that happened in this county). On top of it, we can add some linear regression, that allows to see a relationship between the white-search-rate and the minority-search-rate, and compare the relationships of each part of the police population. We first separated the data of white, black and hipsanic officers to see the search-rates evolutions for black and hispanic people.*bias

<!--  first plot que t'avais mis Samuel -->


## Regression to predict if a search will be conducted

Finally, despite the lack of data, we tried to design a Logistic Model, which based on the characteristics of the officer, tried to predict if the stop would end up on a search or not. We did not have a lot of time to evaluate its performances nor tuning its parameters, but we still got interesting results:

| Characteristic    | Influence |
|-------------------|-----------|
| officer_black     | -0.56     |
| officer_white     | 0.231     |
| officer_hispanic  | 0.294     |
| officer_female    | -1.95     |
| subject_female    | -1.09     |
| officer_age_45-55 | -2.08     |



