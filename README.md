# How-Strategies-can-create-bias
*Keywords: Causal Model, Directed Acyclic Graphs(DAG), Collider Bias, Bayesian Statistics, Python  PYMC3*<br/>

Sometimes Business Strategies can create bias. If we are after the causal relationship we need to condition on the effects of strategies too. Here we go through the intuitive process of how such biases arise and how we can control for such biases.

## Data Set
The dataset we will be working with is a Marketing Campaign report with daily campaign spending, daily closing retail sales, and Daily Closing Profit at a company’s different retail stores around the nation over few weeks.
This 
<img src="https://github.com/roesta07/How-Business-Strategies-can-create-bias/blob/main/img/dagitty-model.png?raw=true" width="700" height="700">

Naturally, as an analyst, we would want to find the direct effect of the campaign to profit. Or a residual effect of the campaign to profit after controlling for Retail sales. As companies can make a profit from other sources too except for Retail Sales; Also marketing campaigns usually increase sales on other platforms too. For example; A campaign targeted to increase retail sales can also increase online sales during the same campaign period.

### First Intuition
But this company has no other sales channels except for retail sales. Therefore, we can already guess the answer that there should not be any residual effect of Campaign to Profit after controlling for Retails Sales. i.e. after controlling for Retail sales association between Campaign and Profit is Spurious (an example of a spurious association in Business).

But anyway let’s run the model to see the bigger picture<br/>

*Model Assumtions with it's Mathematical Notations:*<br/>
<br/>
&nbsp;&nbsp;&nbsp;- Intercept&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![intercept](https://latex.codecogs.com/png.latex?a%5Csim%20Normal%280%2C1%29)<br/>
&nbsp;&nbsp;&nbsp;- Beta(Retail Sales)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Beta(Retail Sales)](https://latex.codecogs.com/png.latex?b_R%20%5Csim%20Normal%280%2C1%29)<br/>
&nbsp;&nbsp;&nbsp;- Beta(Campaign)&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Beta(Campaign)](https://latex.codecogs.com/png.latex?b_C%20%5Csim%20Normal%280%2C1%29)<br/>
&nbsp;&nbsp;&nbsp;- Mean&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Beta(mu)](https://latex.codecogs.com/png.latex?mu%20%3Da&plus;b_R*R%28std%29&plus;b_C*C%28std%29)<br/>
&nbsp;&nbsp;&nbsp;- Profit&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![Profit](https://latex.codecogs.com/png.latex?profit%5Csim%20Normal%28mu%2Csigma%29)<br/>


