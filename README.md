# How-Strategies-can-create-bias
*Keywords: Causal Model, Directed Acyclic Graphs(DAG), Collider Bias, Bayesian Statistics, Python  PYMC3*<br/>

Sometimes Business Strategies can create bias. If we are after the causal relationship we need to consider such biases too. Here we go through the intuitive process of how such biases arise and how we can control for such biases. The code to replicate this is [here!](https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/strategy_analysis.ipynb).

## Data Set
The dataset we will be working with is a Marketing Campaign report with daily campaign spending, daily closing retail sales, and Daily Closing Profit at a company’s different retail stores around the nation over few weeks.

* Campaign Spendings - (in standard unit)
* Retail Sales - (effect of some proportion)
* Profit Sales - (effect of some proportion)
* Region - (categorical)

### This can be further explained with the help of Directed Acyclic Graph(DAG)
<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/dag1.png?raw=true" width="820">

Naturally, as an analyst, we would want to find the direct effect of the campaign to profit. Or a residual effect of the campaign to profit after controlling for Retail sales. As companies can make a profit from other sources too except for Retail Sales; Also marketing campaigns usually increase sales on other platforms too. For example; A campaign targeted to increase retail sales can also increase online sales during the same campaign period.

### Intuition Block>>
But this company has no other sales channels except for retail sales. Therefore, we can already guess the answer that there should not be any residual effect of Campaign to Profit after controlling for Retails Sales.Business cases are also full of such spurious associations; an example of a spurious association in Business can be found [here!](https://github.com/roesta07/A-simple-Business-Question/blob/main/README.md).

But anyway let’s run the model to see the bigger picture<br/>

## Model
*Model Assumtions with it's Mathematical Notations:*<br/>
<br/>
&nbsp;&nbsp;&nbsp;- Intercept&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![intercept](https://latex.codecogs.com/png.latex?a%5Csim%20Normal%280%2C1%29)<br/>
&nbsp;&nbsp;&nbsp;- Beta(Retail Sales)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Beta(Retail Sales)](https://latex.codecogs.com/png.latex?b_R%20%5Csim%20Normal%280%2C1%29)<br/>
&nbsp;&nbsp;&nbsp;- Beta(Campaign)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Beta(Campaign)](https://latex.codecogs.com/png.latex?b_C_P%20%5Csim%20Normal%280%2C1%29)<br/>
&nbsp;&nbsp;&nbsp;- Mean&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Beta(mu)](https://latex.codecogs.com/png.latex?mu%20%3Da&plus;b_R_P*R%28std%29&plus;b_C*C%28std%29)<br/>
&nbsp;&nbsp;&nbsp;- Profit&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Profit](https://latex.codecogs.com/png.latex?profit%5Csim%20Normal%28mu%2Csigma%29)<br/>

## Model Summary
<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/summary-05.png?raw=true" width="820">

To our surprise; our Model is quite sure that the association between campaign and profit is negative once we know Retail Sales. We could interpret that every penny spent on the campaign is not only ineffective but it's hurting our profit.
We can take a deeper look with the plot below:

<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/grph1.png?raw=true" width="820" >

### Intuition Block >>
This can be true but most of the time Marketing campaigns are designed to increase sales rather than hurt profits. If we report this to the company; the marketing agencies or department responsible for this marketing campaign is going to yell at us and we cannot say anything back because we would be wrong.

> It is the glory of God to conceal things, but the glory of kings is to search things out
> -Proverbs 25:2 ESV  

So, what just could have happened; the Model is not wrong it did its job but the causal interpretation of that association would be. The association was wrong because our Directed Acyclic Graph(DAG) is incomplete and there are possibly that our DAG is haunted.

<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/dag2.png?raw=true" width="820" >


This is one of the possible dags which could be true.
We can project our DAG as a series of implied functional relationships. The DAG above implies that: 
- Retail Sales(R) is some function of Campaign(C) and an Unobserved Variable(U) 
- Profit(P) is some function of Campaign(C), Retail Sales(R), and an Unobserved Variable(U) 
- Campaign(C) and an Unobserved Variable(U) are not functions of any other known variables.<br/>

Our Retail Sales and Profit must have been affected by some unmeasured Variable, thus creating a bias. This kind of bias has the name as "Collider Bias" in the industry and is caused by conditioning on a common consequence.

## Strategy As an Unmeasured Variable
<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/dag3.png?raw=true" width="820">

A Business has to make a lot of strategies thus increasing the chance of biased interpretation. One of the main Culprits for biased interpretation and analysis in business is failing to consider the effects of strategies. But as an analyst; especially if one is outsourced he/she is unaware of these company strategies which can create biased inferences. 

Here in this project the company recently opened new retail Stores at new regions but the campaign was targeted to all regions including old. And since the company has added new stores it has strategized for new stores by compromising margins, adding region-specefic services, and attractions. Which then affected Sales and Profit.

And What else could be best other than to include Regions(New and Old) to get the effect of Strategies. Well, you could have also included area-specific-Margins, Cost, and other strategy-related variables; but adding Regions includes all the added effects ot the strategy as the strategy was region-specific.

<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/grph2.png?raw=true" width="820" >


## So what can we get out of this??
When we stratify by Retail Sales; Retail Sales in the new region must have high Campaign Spending and Retail Sales in the old regions must have less campaign spending; otherwise they wouldn’t fall into the same quantile. And this is what regression model did above; it stratified Retail Sales, opened the path between campaign to retail sales to Profit. Hence creating a Spurious association between campaign and profit. The fact that you haven’t measured it does not mean that it does not exists. 

## Note 
Business analysis are not this simple. It takes iterations and testing different cases, scenarios, and reasoning. Plus cause and effect are harder to detect with empirical studies with direct observational data. Also, there can be a lot of unmeasured variables in any business cases; here we considered only one. For instance, another running campaign could have affected sales and profit too creating bias.At last, We could have never solved this inference without intuition and the help of DAGs.
