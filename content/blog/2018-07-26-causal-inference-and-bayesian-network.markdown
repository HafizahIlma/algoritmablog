---
title: Causal Inference and Bayesian Network
author: Anggia
date: '2018-07-26'
slug: causal-inference-and-bayesian-network
categories:
  - R
tags:
  - Bayesian
  - Causal Inference
  - Machine Learning
description: ''
featured: ''
featuredalt: ''
featuredpath: ''
linktitle: ''
type: post
---

# Introduction
Cause has been people's curiosity for a long time, you can see that people often ask "Why" to things happening around them.
But do we actually know how to explain "cause"?  
Do we jump to conclusion of causal relationship often too quickly?  
Can association between factors that we call mathematically "correlation" tell us possible causal relationship? No. Correlation shows whether two variable go up or down together. But just because two variables go up together doesn't mean it affects each other.



<img src="/img/correlation.jpg" style="display: block; margin: auto;" />

Sometimes we tend to quickly "judge" consequences of action, whether  it is strong or weak, such as "the new rule causes riot", "the president's opinion affected public perception of him", or "chemical X has been linked to cancer".   
In recent years, diverse fields begin to use "evidence-based" decision - that knowledge of *outcomes*, gathered from scientific studies and other empirical sources, should inform our choices, and we expect that these *choices will cause the desired results*. But conducting ideal experiments are expensive and hard, and often we cannot make sure the experiments result will match reality.  
Nowadays, maybe causation is no longer a concern as tons of data and huge computing power allow us to make a model with such high accuracy, precision, and sensitivity. The thinking goes, if our model is representative for millions of cases in our data, our model already explains enough even if we don't know how or why. But we have to remember that models use a lot of correlation between variables.

Truth be told, causation is rarely as simple as we tend to assume and its complexities are often under-rated. This is a serious business as misunderstanding causal links can result in ineffective actions being chosen, harmful practices perpetuated, and beneficial alternatives overlooked. Understanding causation as best as we can is absolutely neccesary for interpreting data, whether big or small.[^1] In the first part of this session I will try to share some facts about causation.

# What is Causal Inference?
Before talking about causal inference, we need to understand first about causation. Causation is defined as "something that makes a difference, and the difference it makes must be a difference from what would have happened without it" [^2]

As Wikipedia said, causal inference is the process of drawing a conclusion about a causal connection based on the conditions of the occurrence of an effect. [^3]  

A few things to note about causal inference:[^4]   

1. Seeing vs doing (There is no causation without manipulation)
Let's say we see from data that plant's growth is highly correlated with sun light intensity. But, we cannot quickly conclude that light affects plant growth, as we only *see* that light and growth correlate. To prove that light affects plant growth, we need to conduct an experiment where we can *manipulate* the light intensity. If after we collect some data of the experiment we see that the correlation is still high, then we can more safely say that light affects growth.  

2. If X causes Y, does not mean Y wouldn't exist if X does not happen. It is just if X changed, Y will change accordingly.  
Example : Fertilizer affects grass growth, but does not mean grass cannot grow at all when there is no fertilizer.

3. If you know that, on average, A causes B and B causes C, this does not mean that you know that A causes C.
It depends on condition of each happenings.
Example is :   
If I feel hungry, I will eat.   
If I eat, I will be full.   
But, we cannot conclude If I feel hungry, I will be full.   

4. X can cause Y even if there is no "causal path" connecting X and Y. 
Think of it like you want to cook, but somebody call you and make you distracted thus the food burned out. That somebody "contributes" the food to burn out. 

5. Correlation *does not* imply causation.   
Back to example in the introduction.  

<img src="/img/correlation.jpg" style="display: block; margin: auto;" />

Let's say that we believe in the power of correlation and believe that there might be causal relationship between number of divorce in Maine and margarine consumption. But then we still questions to answer :  
  i. In what direction the causation happens? Number of divorce affects margarine consumption or margarine consumption affects number of divorce?   
  ii. To answer number one, and validate causation using manipulation, there have to be experiment/change of condition. For example, if there is trend change in which majority of people reduce margarine consumption, will divorce rate plummet too? Or if people in Maine are influenced to maintaining harmonious marriage, will the consumption of margarine go down too?  
Well, so causation alone is not enough to explain causation.  

Aside from above example (spurious correlation), I will introduce you to Simpson's Paradox. Simpson's paradox is the fact that the correlation between two variables can actually be reversed when additional factors are considered. So two variables which appear correlated can become anticorrelated when another factor is taken into account.[^5][^12]

See this example: 

<img src="/img/simpson.jpg" style="display: block; margin: auto;" />

If you see these table, and asked, which treatment was better, what will you answer? Without considering gender, drug treatment looks better but when we take gender into consideration non-drug treatment looks better.

Judea Pearl,a computer scientist who is championing probabilistic approach to artificial intelligence, proposed solutions to this problem:  
"do-calculus" (or causal calculus).   
Do-calculus is a mathematical approach to learn probabilistic relation in manipulated events. It is done by inspecting graphical causal relations and Bayesian probability of events and adding some extra assumptions. I won't discuss about it now, but you can learn it in some resources.[^6]

Now, let's discuss some real debate regarding correlation. So there was a high correlation between smoking and lung cancer, and people thought smoking causes lung cancer. However, some people argued against this and think there may be some hidden factor (genetic traits maybe?) which contribute to person's tendency to smoking and his high probability developing a cancer.  

<img src="/img/smoking_basic_causal_model.png" style="display: block; margin: auto;" />

Randomized experiment can help resolve this. Make an experiment where people are forced to smoke and record how much of them develop lung cancer. But, this kind of experiment is illegal, so usually we resort to the mathematical "do-calculus".[^5]

# Causal Inference in Machine Learning : Bayesian Network
When we have a decent causal relations of something, we can have an unsupervised (or seldom supervised) called Bayesian Network. 
Bayesian Network is a way we represent probability distribution in a graph, to be specific Directed Acyclic Graph.  
An interactive example : here[^7]   
Here you can see how things have far-impacting capability and it is based on data.

## Usage and Plus of Bayesian Network
To give you motivation to learn this, I'm going to introduce what is the use of Bayesian Network.

<img src="/img/use_bn.png" style="display: block; margin: auto;" />

So, Bayesian Network can be used for prediction, anomaly detection, decision making, etc.

There are benefits to using BNs compared to other unsupervised machine learning techniques. A few of these benefits are: [^8]  
1. It is easy to exploit expert knowledge in BN models.  
2. BN models have been found to be very robust in the sense of  
    i) noisy data,  
    ii) missing data and  
    iii) sparse data.   
3. Unlike many machine learning models (including Artificial Neural Network), which usually appear as a "black box," all the parameters in BNs have an understandable semantic interpretation.

## Before Jumping In, Probability
I will introduce you some terms of probability  
1. Marginal probability : it is probability of an event that we calculate considering the supporting factors.  
2. Join probability : probability of 2 or more events happening in the same time.  
3. Conditional probability: probability of and event happening given another event has happened  

Bayes rule can be interpreted as a learning/inference mechanism, i.e., what is the new probability of having cancer after we have known the evidence, ie, that the test as been positive. In this interpretation, the probabilities are named as follows:[^9]  
1. P(C), *the prior probability* (what we know before the evidence)  
2. P(+|C), the *likelihood* of the data given the hypothesis  
3. P(+), *the evidence* (the marginal probability of the test is positive)  
4. P(C|+), *the posterior probability*, the new belief after the evidence is processed  

## Directed Acyclic Graph (DAG)
To understand BN, first we need to understand about graph.[^13]
If we want to say A causes B, we draw a graph like this: A->B
It is a directed graph because we need to specify the direction of the line connecting, as A->B is different with A<-B.
Acyclic means there is no cycle in the graph. For example look at this graph:  

<img src="/img/graph_with_loops.png" style="display: block; margin: auto;" />

In this cyclic graph , we have X causes Y causes Z causes X, which is nonsense. Thus, we must have acyclic graph.  

Note : X,Y,Z are called node, and the arrows are called arc/edge.

## Connections

<img src="/img/graph.jpg" style="display: block; margin: auto;" />

Suppose we have the above DAG. We are going to learn some of important terms to explain the connections between the nodes.  
- We see that A -> E. Here, we can say that A is the parent of E and E is the descendant of A.  
- Every node *depends* only on its parents.  
- Mainly there are three types of connection between 3 nodes:[^10]  
  a. Serial connection, e.g. A -> E -> O  
      Example : Burglary -> Alarm -> Call Police   
      In this kind of connection, information of `Burglary` will affect the probability of `Alarm` and `Call Police`, and information of `Call Police` will affect the our belief of `Alarm` and `Burglary`. However, when we know information of `Alarm`, additional knowledge about `Burglary` does not improve the probability of `Call Police` and vice versa.  
      Thus the proposition goes : information may flow through a
serial connection X -> Y -> Z unless the state of Y is known.  
  b. Divergent connection, e.g. O <- E -> R  
      Example : Alarm <- Earthquake -> News  
      In this kind of connection, information of `Alarm` will affect the probability of `Earthquake` and `News`, vice versa. However, when we know information of `Earthquake`, additional knowledge about `Alarm` does not improve the probability of `News` and vice versa.  
      Similar to serial connection, the proposition goes : information may flow through a serial connection X <- Y -> Z unless the state of Y is known.  
  c. Convergent connection, e.g. A -> E <- S (also referred to as a v-structure)  
      Example : Burglary -> Alarm <- Earthquake  
      In this kind of connection, if we have no info about `Alarm `, known info about `Burglary` will not improve our knowledge on `Earthquake` and vice versa. However, if we have info about `Alarm`, info about `Burglary` will affect probability of `Earthquake` and vice versa.  
      This connection is the opposite of divergent and serial connections. For this connection the proposition goes :information may flow through a converging connection X -> Y <- Z if evidence on Y or one of its descendants is available.

## R Implementation
Actually you can have continous and discrete data on this model but to make it easier we will look into binary discrete data ( discrete data with 2 categories).
To implement Bayesian Network in R we need to use `gRain` package
and to make graph we need `Rgraphviz` package


Let's say we have a case of a disease and two tests.
We say test is negative if there is no evidence of disease, and vice versa.
After gathering some data, we know that:
1. Probability of acquiring disease is 0.01  
2. Probability of positive on test 1 given tested has disease is 0.9  
3. Probability of positive on test 1 given tested has no disease is 0.15  
4. Probability of positive on test 2 given tested has disease is 0.95  
5. Probability of positive on test 2 given tested has no disease is 0.2  

Or in math terms:  
1. P(D)=0.01 ; P(~D)=0.99  
2. P(T1=+|D)=0.9 ; P(T1=-|D)=0.1   
3. P(T1=+|~D)=0.15 ; P(T1=-|~D)=0.85  
4. P(T2=+|D)=0.95 ; P(T2=-|D)=0.05  
5. P(T2=+|~D)=0.2 ; P(T2=-|~D)=0.8  

First, we have to make assumption about the plausible causal relations between the variables. The plausible one will be, presence of disease affects the result of test. So, the DAG would be:


```r
disease.dag<-dag(~D:T1:T2)
plot(disease.dag)
```

<img src="/blog/2018-07-26-causal-inference-and-bayesian-network_files/figure-html/unnamed-chunk-9-1.png" width="672" style="display: block; margin: auto;" />

Now we are going to make the Bayesian Network model

```r
# the possible values (all nodes are boolean vars - discrete with two category)
yn <- c("yes","no")

# specify the CPTs
node.D <- cptable(~ D, values=c(1, 99), levels=yn)
node.T1 <- cptable(~ T1 + D, values=c(9,1,15,85), levels=yn)
node.T2 <- cptable(~ T2 + D, values=c(95,5,2,8), levels=yn)


# create an intermediate representation of the CPTs
plist <- compileCPT(list(node.D, node.T1, node.T2))
plist
```

```
#> CPTspec with probabilities:
#>  P( D )
#>  P( T1 | D )
#>  P( T2 | D )
```


```r
#kind of prop table
plist$D
```

```
#> D
#>  yes   no 
#> 0.01 0.99 
#> attr(,"class")
#> [1] "parray" "array"
```

```r
# create network
bn.disease <- grain(plist)
summary(bn.disease)
```

```
#> Independence network: Compiled: FALSE Propagated: FALSE 
#>  Nodes : chr [1:3] "D" "T1" "T2"
```

```r
bn.disease
```

```
#> Independence network: Compiled: FALSE Propagated: FALSE 
#>   Nodes: chr [1:3] "D" "T1" "T2"
```

```r
#We can gain the probability of events using query

#if we want to see the marginal probability (probability of individual event) of each node
querygrain(bn.disease, nodes=c("D", "T1", "T2"), type="marginal")
```

```
#> $D
#> D
#>  yes   no 
#> 0.01 0.99 
#> 
#> $T1
#> T1
#>    yes     no 
#> 0.1575 0.8425 
#> 
#> $T2
#> T2
#>    yes     no 
#> 0.2075 0.7925
```

```r
#or
querygrain(bn.disease, nodes=c("D", "T1", "T2"))
```

```
#> $D
#> D
#>  yes   no 
#> 0.01 0.99 
#> 
#> $T1
#> T1
#>    yes     no 
#> 0.1575 0.8425 
#> 
#> $T2
#> T2
#>    yes     no 
#> 0.2075 0.7925
```


```r
# or if we want to see joint probability (probability of 2 or more events happening), for example P(D,T1)
querygrain(bn.disease, nodes=c("D", "T1"), type="joint")
```

```
#>      T1
#> D        yes     no
#>   yes 0.0090 0.0010
#>   no  0.1485 0.8415
```

```r
# or if we want to see conditional probability of T1 given D
querygrain(bn.disease, nodes=c("T1","D"), type="conditional")
```

```
#>      T1
#> D      yes   no
#>   yes 0.90 0.10
#>   no  0.15 0.85
```

## CAD Dataset
We can also make Bayesian Network model from a dataset. This time we are going to use Cardio Artery Disease data from R dataset.

```r
data("cad1")
str(cad1)
```

```
#> 'data.frame':	236 obs. of  14 variables:
#>  $ Sex        : Factor w/ 2 levels "Female","Male": 2 2 1 2 2 2 2 2 1 2 ...
#>  $ AngPec     : Factor w/ 3 levels "Atypical","None",..: 2 1 2 2 2 2 2 2 2 1 ...
#>  $ AMI        : Factor w/ 2 levels "Definite","NotCertain": 2 2 1 2 2 2 2 2 2 2 ...
#>  $ QWave      : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 2 2 1 1 ...
#>  $ QWavecode  : Factor w/ 2 levels "Nonusable","Usable": 2 2 2 2 2 2 2 2 1 2 ...
#>  $ STcode     : Factor w/ 2 levels "Nonusable","Usable": 2 2 2 1 1 1 1 1 1 2 ...
#>  $ STchange   : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 2 ...
#>  $ SuffHeartF : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
#>  $ Hypertrophi: Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
#>  $ Hyperchol  : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
#>  $ Smoker     : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
#>  $ Inherit    : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
#>  $ Heartfail  : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
#>  $ CAD        : Factor w/ 2 levels "No","Yes": 1 1 1 1 1 1 1 1 1 1 ...
```

We are going to focus on`CAD` variable, which represent whether a person has CAD.
First, we need some assumptions about how the variables relate in the dataset. This is usually gained from expert opinions and some common sense.
Suppose we assume that `Smoker`,`Inherit`, and `Hyperchol` variables affect `CAD` and `CAD` affect `AngPec`,`Heartfail`,`QWave`. We create the DAG of the causal model we have.

```r
# create the DAG
dag.cad <- dag(~ CAD:Smoker:Inherit:Hyperchol + 
                 AngPec:CAD + 
                 Heartfail:CAD + 
                 QWave:CAD)
plot(dag.cad)
```

<img src="/blog/2018-07-26-causal-inference-and-bayesian-network_files/figure-html/unnamed-chunk-17-1.png" width="672" style="display: block; margin: auto;" />

```r
# smooth is a small positive number to avoid zero entries in the CPTs
# (cf. Additive smoothing, http://en.wikipedia.org/wiki/Additive_smoothing)
bn.cad <- grain(dag.cad, data = cad1, smooth = 0.1)
# Let's ask some questions...
querygrain(bn.cad, nodes=c("CAD", "Smoker"), type="conditional") 
```

```
#>       CAD
#> Smoker        No       Yes
#>    No  0.7172084 0.2827916
#>    Yes 0.4933771 0.5066229
```


```r
# add some evidence and see how things get affected.
#let say we know a person who is a smoker but he does not have hyperchol, and we want to know the probability of him gettinf CAD
bn.cad.1 <- setFinding(bn.cad, nodes=c("Smoker","Hyperchol"), states=c("Yes","No"))
querygrain(bn.cad.1, nodes=c("CAD"), type="marginal") 
```

```
#> $CAD
#> CAD
#>        No       Yes 
#> 0.6551764 0.3448236
```

## Weaknesses of Bayesian Network
1. It is computationally expensive for complex models to be implemented in personal computer.
2. The algebraic demands of the methods (e.g. the need for a full understanding of probability theory)
3. Making sure that the prior knowledge is reliable [^11], which means the probability gained from data or the distribution we choose to use.
4. Different experts can suggest different causal models.

# Annotations
[^1]: https://www.thenewatlantis.com/publications/correlation-causation-and-confusion  
[^2]: Lewis, David. "Causation." The journal of philosophy (1973): 556-567.  
[^3]: https://en.wikipedia.org/wiki/Causal_inference  
[^4]: https://egap.org/methods-guides/10-things-you-need-know-about-causal-inference   
[^5]: http://www.michaelnielsen.org/ddi/if-correlation-doesnt-imply-causation-then-what-does/  
[^6]: Pearl, J. (2003). Causality: models, reasoning, and inference. Econometric Theory.  
[^7]: https://www.bayesserver.com/examples/networks/asia  
[^8]: https://www.r-bloggers.com/bayesian-network-in-r-introduction/   
[^9]: http://www.di.fc.ul.pt/~jpn/r/bayesnets/bayesnets.html  
[^10]: https://pdfs.semanticscholar.org/3ef6/97a17a05930bdbb586cc12676e60bebb5bd1.pdf  
[^11]: http://niedermayer.ca/book/export/html/29  
[^12]: http://www.homepages.ucl.ac.uk/~ucgtrbd/talks/imperial_causality.pdf  
[^13]: http://www.ucdenver.edu/academics/colleges/PublicHealth/Academics/departments/Biostatistics/WorkingGroups/Documents/Networks%20Presentation%20With%20Sachs%20-%20032317.pdf  
