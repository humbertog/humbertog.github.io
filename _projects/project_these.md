---
layout: distill
title: PhD thesis
description: Study of the choice behaviour of travellers in a transport network via a simulation game
img: assets/img/thesis/MDG_main2.png
importance: 1
category: data science
date: 2020-06-01
bibliography: thesis.bib

authors:
  - name: Humberto González R.
    affiliations:
      name: LICIT, Univ. Lyon - ENTPE
  
toc:
  - name: Introduction
  - name: Methods
    subsections:
        - name: Experiment
        - name: Model
  - name: Results
    subsections:
        - name: Route choices at urban network level
        - name: Travel time and bounded rationality
        - name: Bounded rational choice set generation
       

---


**Keywords:** travellers choice behaviour, discrete choice models, experimental economics


## Introduction

#### Context
When travelling in a city, travellers face several decisions regarding the (i) activity to engage, (ii) the destination of the trip, (iii) the mode of transportation, (iv) the departure time, and (v) the route to complete the trip. These individual decisions altogether shape the traffic patterns in the transportation network at a given time. 

Travellers' choice behaviour is a process which involves psychological and cognitive mechanisms through which travellers perceive and evaluate the states of the network, and then make decisions accordingly <d-cite key="Ramos2015"></d-cite><d-cite key="Ben-Akiva1999"></d-cite>. Although this definition may appear simple, there are many factors, associated to both the traveller and the environment, that intervene in this process. These factors are heterogeneous (as heterogeneous as individuals can be), and they interact in ways that are not easily observable to produce the choices. 

The studies on travellers' behaviour are predominantly based on laboratory-like experiments, where participants' choices are observed on simple scenarios (two or three routes in few OD pair configurations) that do not cover the multiplicity of situations that are found in a city-scale transportation network. These simplifications are made in order to guarantee the internal validity of the experiments avoiding the introduction of confounding factors into the. For example, if the objective is to measure the impact of travel time reliability in the route choices of travellers, then it suffices to present choice problems to the participants with only two alternative routes; the shape and attributes of the routes are irrelevant. Since the objective of these studies is to understand the determinants of travellers' behaviour, the choice models play a mainly interpretative role. Moreover, these choice models will not generalise to the variety of situations that can be found in a real urban network, in the sense that their predictions may not be accurate in OD pairs with different enough characteristics. 


#### Objectives
Here, we are interested in the `predictions of choice models for an entire road network`, where the choices of travellers are made over `thousands of OD pair configurations that consist of short and long trips as well as routes that differ in their attributes.` In the case concerning this thesis, the city of Lyon in France, the network has 19,967 links and 19,697 nodes. However, gathering data to estimate the choice models in such a large amount and variety of situations is technically impossible. For example, if we assume that there is at least one route connecting each origin to each destination we would have more than 697$^2=$ 485,809 routes in Lyon. 


The idea in this thesis is to `gather data on travellers choices using computer experiments on a reduced, but representative, subset of scenarios.` The choice models estimated with this data need to generalise to every situation found in the entire network. In other words, models that make accurate predictions in all OD pairs in an urban transportation network, but that are estimated with observations on a limited number of OD pairs. More specifically, the objectives of my thesis are:
1. `Design of experiments`: sample of OD pairs and routes to use in the experiments, such that the choice models estimated with this data accurately predict the choices of travellers at network level.
2. `Model selection`: finding of route and departure time models based on the collected empirical evidence. 

## Methods

### Experiment
The approach in this thesis to investigate travellers' behaviour in transportation networks is through `computer-based experiments at large scale`, for which a platform named the `Mobility Decision Game (MDG)` has been developed. The MDG allows to observe the choices of many participants in the experiments under different situations (see Fig.~\ref{fig:intro_mdg_conditions}), which is essential for inferring choice models at full-scale network level. 

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/maps_attributes_O17D17.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/maps_attributes_O19D19.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Two OD pairs joined by three routes for the MDG experiment. The experiments consist in choosing one out of three alternative routes to complete a trip. The varying traffic conditions and route attributes allow to observe the choices of travellers in different situations over the city of Lyon in France.
</div>

The MDG is a web-based computer game designed to confront the participants in the experiments with decision problems regarding the departure time and the route choices to complete a trip. The decision problems are placed under different hypothetical scenarios, consisting of OD pairs joined by three alternative routes with contrasting attributes and varying traffic conditions. `The scenarios occur in a dynamically simulated environment of the real network of the city of Lyon, France`, in which the `traffic conditions are generated in real-time by a single dynamic microscopic simulator`. During an MDG experiment, multiple OD pairs are assigned to the participants, allowing to observe the choices of the same participants in different OD pairs. Furthermore, some participants may receive traffic information in the form of travel time. 

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/MDG_main.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    MDG interface. An OD pair and three alternative routes are shown over a section of the Lyon network in the MDG. The left menu allows participants to choose the mode of transportation, the departure time and the route choice. 
</div>


### Modelling
The model adopted in this thesis to predict the choices of travellers is the `mixed logit model (MXL)` <d-cite key="Mcfadden1984"></d-cite><d-cite key="mcfaddentrainMMNL"></d-cite>, which belongs to the `random utility maximisation framework` <d-cite key="Mcfadden1972"></d-cite>. This framework assumes that individuals obtain a certain level of utility from each alternative in a choice situation, and that they choose the alternative with the maximum utility, i.e., `individuals are utility maximisers`. In random utility maximisation models, the utility obtained from an alternative is related to the attributes of both the alternative and the decision-maker. As a consequence, the choices of the decision-makers are, to some extent, explained by variables that can be measured and that depend on the OD pair and on the situation (OD pair and traffic conditions). Moreover, the number of assumptions about the parameters of the model is small, and these can be easily inferred from the data, obtaining succinct representations of the utility and the choice probabilities. Therefore, random utility maximisation models are well-suited for the prediction of travellers' choices at full-scale network. A careful selection of the scenarios over which the MDG experiments evolve, permits to estimate choice models that generalise well to the full-scale network level, and to test and select the model specification that best represent the choices of the participants in the experiments.

In the MXL model, the coefficients $\beta$ (the preferences of individuals) are considered random variables. Conditioning on $\beta$, the probability that individual $i$ chooses alternative $j$ is

$$
    Pr(y_{ij} = 1 \,|\, \beta) = \frac{e^{V(x_{ij};\beta)}}{\sum_{k=1}^{J} e^{V(x_{ik};\beta)}},
$$

where $x_{ij}$ is the vector of attributes of the individual $i$ and the alternative $j$. Since $Pr(y_{ij} = 1 \|\beta)$ is a function of the random vector $\beta$, the conditional probability in this last expression is a random variable. Thus, to obtain the (unconditional) choice probability, the above expression has to be integrated over all the possible values of $\beta$, i.e.,  

$$
\begin{align*}
    Pr(y_{ij} = 1) &= \int_{\Omega(\beta)} Pr(y_{ij} = 1 \,|\, \beta=u) Pr(\beta=u)du\\
                   &= \int_{\Omega(\beta)} \frac{e^{V(x_{ij};u)}}{\sum_{k=1}^{J} e^{V(x_{ik};u)}} f_{\beta}(u;\theta)du\,,
\end{align*}
$$

where $f_{\beta}(\cdot;\theta)$ is the probability density function of $\beta$ parametrised by $\theta$. This last expression is the general form of the MXL models. 


## Results
### Route choices at urban network level <d-cite key="GONZALEZRAMIREZ202159pone.0225069"></d-cite>

In a city-scale network, trips are made in thousands of origin-destination (OD) pairs connected by multiple routes, resulting in many alternatives with diverse characteristics that influence the route choice behavior of the travellers. As a consequence, to accurately predict user choices at full network scale, a route choice model should be scalable to suit all possible configurations that may be encountered. In this chapter, a new methodology to obtain such a model is proposed. The main idea is to use clustering analysis to obtain a small set of representative OD pairs and routes that can be investigated in detail through computer route choice experiments to collect observations on travellers behaviour. The results are then scaled-up to all other OD pairs in the network.

It was found that `9 OD pair configurations are sufficient to represent the network of Lyon, France, composed of 96,096 OD pairs and 559,423 routes`. The observations, collected over these nine representative OD pair configurations, were used to estimate three mixed logit models.

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/mapLyon-full-centroids.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Representative OD pairs connected by three routes. Clustering analysis is used to find groups of OD pairs and routes with simmilar characteristics (length, number of truns per km, directness, ...). The center of each cluster (its mean observation) is picked as representative of the group. Nine clusters were used to group the observations. The distributions of the attributes of the selected OD pairs are similar to the distributions of the attributes of OD pairs in the whole network.
</div>

 The predictive accuracy of the three models was tested against the predictive accuracy of the same models (with the same specification), but estimated over randomly selected OD pair configurations. `The models estimated with the representative OD pairs are superior in predictive accuracy, thus suggesting the scaling-up to the entire network of the choices of the participants over the representative OD pair configurations, and validating the methodology in this study.`

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/MPE_INI_d.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Mean predictive errors (MPEs). The MPEs of the model trained with the representative OD pairs, $M^*$, are smaller than the MPEs of models trained with randomly selected OD pairs, $M_r$, in the majority of the cases (blue dots). Thus, models trained with data on the representrative OD pairs perform better in non-observed OD pairs; they generalise better. 
</div>


### Travel time and bounded rationality <d-cite key="GONZALEZRAMIREZ202159"></d-cite>

Recent empirical studies have found that travellers route choices deviate from perfect rationality, by showing that urban trips do not necessarily follow the shortest-time routes <d-cite key="PAPINSKI2009347"></d-cite><d-cite key="thomas2010route"></d-cite><d-cite key="Zhu2015"></d-cite><d-cite key="Hadjidimitriou2015"></d-cite><d-cite key="Yildirimoglu2018"></d-cite>. However, there is no consensus on how much the travellers' route choice behaviour deviates from the perfect rational assumption. The objective of this study is to contribute to the understanding on how travellers process travel time when making route choices, and to quantify to what extent users are strict travel time minimisers or if bounded rationality is observed. Whether travellers evaluate travel time differences in absolute or relative terms is also addressed, and the heterogeneity in the route choice behaviour of travellers investigated. The results of route choice computer experiments, focused on the route choices in diverse OD pairs and traffic conditions, are analysed. In total, 496 participants recorded 5,535 route choices over 41 OD pairs. 

It was found that `travellers evaluate relative rather than absolute differences in travel time`. In 60.5% of the trips participants chose the fastest route, but this percentage is 80% when the travel time of the alternatives is at least 30% higher than the fastest route. Only 10% of the individuals chose the fastest route in all trips, confirming the hypothesis of bounded rationality. The participants exhibited heterogeneous travel time indifference bands: at least 70% of them would not consider routes with travel times 1.5 times slower than the fastest alternative; the average participant was indifferent to relative travel time differences of less than 31%.


<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/MRvsITT2-ITT1_abs.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/MRvsITT2-ITT1_rel.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The percentage of choices for the fastest route, $F_(1)$, as a function of the difference in travel time between the fastest and the second fastest routes $\alpha$. (a) the absolute difference in travel time information, and (b) the relative difference in travel time information. The red lines are the predictions of the logistic models; the ribbon corresponds to the 95% confidence interval. 
</div>



### Bounded rational choice set generation

I develop a choice model that considers bounded rational behaviour in the individuals' choice set generation for route choice (BRCS model). In the BRCS, the distribution of the indifference bands is inferred endogenously by jointly estimating the choice set generation and route choices. The model is proposed as an alternative to the MXL model, where the individually estimated indifference bands are exogenously estimated and thus enter the model as independent variables. The BRCS model is compared, in terms of predictive accuracy, to the MXL model using synthetic and real data, obtained from the experiments with the MDG platform. The results show that the BRCS model is capable of inferring the distribution of the indifference bands for the synthetic generated data. Moreover, for this data, the BRCS shows higher predictive accuracy than the MXL model. In the case of the MDG data, the BRCS model exhibits higher predictive accuracy than both the MXL and the MXL with exogenously estimated indifference bands. 

#### Model
Given the set of all available alternatives, $R$, probabilistic choice set models assume two steps in the choice process: firstly, the individual $i$ constitutes her/his own choice set $S_i \subseteq R$ and, secondly, she/he chooses an alternative $j\in S_i$ following a discrete choice model with probability $Pr(y_{ij} = 1  \, |\, S_i )$, where $y_{ij} = 1$ denotes the event of decision-maker $i$ choosing the alternative $j$. Since the subset $S_i$ is not observable from the point of view of a bystander (it is latent), a probability measure, $Pr(S_i)$, is assigned to it to express the uncertainty on the actual choice set considered by individual $i$, and the unconditional choice probability is then given by
\begin{equation}\label{eq:choice_set_gen}
    Pr(y_{ij} = 1) = \sum_{S \in \mathcal{P}(R)} Pr(y_{ij} = 1  \, |\, S ) \cdot Pr(S)\,,
\end{equation}
where $\mathcal{P}(R)$ is the power set of $R$ <d-cite key="BenAkivalatentCS1995"></d-cite>.


We consider the case in which individuals constitute their choice set $S_i$ by including only the alternatives whose attribute $z$ (the travel time diiference with respect to the fastest available route in our case) lies below an individual-specific threshold $\eta_i$, i.e., individual $i$ considers alternative $j$ if and only if $z_j \leq \eta_i$. Without loss of generality, we assume that the alternatives are ordered with respect to the continuous attribute $z$, such that $z_1 < z_2 < \cdots < z_J$, where $\|R\| = J$. This implies that the only elements of $\mathcal{P}(R)$ that have non-zero probability of occurring are $\{1\}, \{1, 2\},\dots ,\{1,\dots, J\}$, thus reducing the number of choice sets that could be considered by an individual from $\|\mathcal{P}(R)\|=2^J$ to $\|R\| = J$. Since the choice set $S_i$ is fully determined by the value of $\eta_i$, then the probability of observing the choice set $S_i(k) = \{1, 2, \dots, k\}$ is also fully determined by $\eta_i$ and it is given by

$$ \label{eq:choice_set_prob}
\begin{aligned}
    Pr(S_i(k)) &= Pr(\{1,2,\dots,k\})\\  
    &=Pr(z_{k} \leq \eta_i < z_{k+1})\\
    &=F_{\eta}(z_{k+1};\theta_{\eta}) - F_{\eta}(z_{k};\theta_{\eta})\,,
\end{aligned}
$$

where $F_{\eta}(\,\cdot\,; \theta_{\eta})$ is the cumulative distribution function of $\eta$, parametrised by the unknown parameters $\theta_{\eta}$. We make explicit the assumption that the  distribution $F_{\eta}$ does not depend on the individual (the parameters $\theta_{\eta}$ are constant across the population), meaning that the probability of observing the choice set $S(k)$ is the same for all individuals in the population. This choice generation process is analogous to the boundedly rational model, in which alternatives with values of $z$ above the indifference band are not considered by the decision-maker in the decision process (their choice probability are equal to zero). 

We obtain an expression for the unconditional choice probability by substituting this last expression in Eq. \eqref{eq:choice_set_gen} and observing that the probability of choosing an alternative not belonging to the choice set is zero, i.e., $Pr(y_{ij} = 1  \, \|\, S(k) ) = 0$ for all $j > k$. Then,

$$
\begin{aligned}
    Pr(y_{ij} = 1) &= \sum_{k = j}^{J} Pr(y_{ij} = 1  \, |\, S(k) ) \cdot Pr(S(k))\\
    &=\sum_{k = j}^{J-1} \left\{Pr(y_{ij} = 1  \, |\, S(k) ) \cdot Pr(z_{k} \leq \eta < z_{k+1})\right\} + Pr(y_{ij} = 1  \, |\, S(J) ) \cdot Pr(z_{J} \leq \eta )\\
    &=\sum_{k = j}^{J-1} \left\{Pr(y_{ij} = 1  \, |\, S(k) ) \cdot [F_{\eta}(z_{k+1};\theta_{\eta}) - F_{\eta}(z_{k};\theta_{\eta})] \right\} \\
    &\quad\quad  + Pr(y_{ij} = 1  \, |\, S(J) ) \cdot [1 - F_{\eta}(z_{J};\theta_{\eta})]\,.
\end{aligned}
$$

If we assume that the second stage in the decision process, i.e., $Pr(y_{ij} = 1  \, \|\, S(k) )$, is given by a mixed logit model, then the above expression, conditional on the coefficients $\beta_i$ and the vector of attributes $x_{ij}$, becomes

$$
\begin{split}
     Pr(y_{ij} = 1 \,|\, \beta_i, \theta_{\eta}; x_{ik}\, \forall\,k) &= 
    \sum_{k=j}^{J-1} \left\{ \frac{e^{V(x_{ij};\beta_i)} }{\sum_{m=1}^{k} e^{V(\mathbf{x_{im}};\beta_i)}} \cdot [F_{\eta}(z_{k+1};\theta_{\eta}) - F_{\eta}(z_{k};\theta_{\eta})] \right\} \\
    &\quad\quad  +\frac{e^{V(x_{ij};\beta_i)} }{\sum_{m=1}^{J} e^{V(\mathbf{x_{im}};\beta_i)}} \cdot [1 - F_{\eta}(z_{J};\theta_{\eta})]\,,
\end{split}
$$

and the unconditional choice probability is therefore given by

$$
     Pr(y_{ij} = 1 \,|\,\theta_{\beta}, \theta_{\eta}; x_{ik}\, \forall\,k) = \int_{\Omega_{\beta}} Pr(y_{ij} = 1 \,|\, \beta, \theta_{\eta}; x_{ik}\, \forall\,k) \cdot f_{\beta}(\beta;\theta_{\beta})\,d\beta\,.
$$

$f_{\beta}(\,\cdot\,, \theta_{\beta})$ is the probability density function (parametrised by $\theta_{\beta}$ and with support in $\Omega_{\beta}$) of the distribution of the individual coefficients $\beta_i$, that accounts for the variability on the preferences of individuals for each of the attributes of the alternatives. 


#### Results
The results show that the BRCS model outperforms the MXL model: the BRCS model performed better than the MXL model in more than 80% of the test cases with synthetic (generated) data.

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/err_synth.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thesis/err_synth_dm.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Comparison of the prediction error of the MXL model against the BRCS model for a synthetic data set. (a) cross validation results removing observations (choices). (b) cross validation results removing individuals.
    Each point represents the error of each of the 60 test sets (10 for each of the 6 generated data sets). The dotted lines are the average for each model. 
</div>


