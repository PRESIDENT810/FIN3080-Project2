---
title: "FIN3080 Project 2 Report"
author: YIN Jie\and DENG Ziying\and ZHONG Kaining
date: November 10, 2019
geometry: "left=2.57cm,right=2.57cm,top=2.54cm,bottom=2.54cm"
output: pdf_document
---

Content
---
\Large
* Markowitz Portfolio Optimization
	* Q(1a). Mean and Covariance Matrix
	* Q(1b): Efficient Frontier
	* Q(1c): Portfolio with Minimum Standard Deviation
	* Q(1d): Efficient Frontier with Risk-free Asset
	* Q(1e): Sharpe Ratio for the Tengency Portfolio
	* Q(2a): First Pass Regression
	* Q(2b): Second Pass Regression
* Momentum Strategy
	* Q(a): Mean and Variance of Annualized Returns
	* Q(b): Three-factor Model
	* Q(c): Pick the Best Momentum Strategy
	* Q(d): Momentum Strategy Failures
* Reference
* Appendix
	* Code for Markowitz Portfolio
		* Markowitz Portfolio.ipynb (Python Code)
		* Merge.ipynb (Python Code)
		* Regression.ipynb (Python Code)
	* Code for Momentum Strategy
		* Plot.ipynb (Python Code)
 		* ProblemSet2.do (Stata Code)


\normalsize

\pagebreak
# Problem set 1: Markowitz Portfolio Optimization

## Q(1a). Mean and Covariance Matrix


In question 1.1, we downloaded the monthly returns from Jan. 2013 to Jun. 2019 from CSMAR for securities 600519 (Gui Zhou Mao Tai), 002036 (Lian Chuang Dian Zi) and 002507 (Fu Ling Zhai Cai). The data is presented in the following form:

| Trading Date | Security Code | Monthly Close Price | Monthly return |
| :----------: | :-----------: | :-----------------: | :------------: |
|   2013-01    |    2036.0     |        8.96         |   -0.002227    |
|    ......    |    ......     |       ......        |     ......     |

Then, based on the downloaded historical data, we calculated the mean and variance-covariance matrix for these three stocks.

$$ mean : \mu=E[return_i]=\frac{1}{T}\sum_{t=1}^{T}return_{i,t}$$

$$ variance-covariance\ matrix: \Sigma=\left[
	\begin{matrix}{}
    \sigma_1^2 & Cov_{1,2} & Cov_{1,3}\\
    Cov_{2,1} & \sigma_2^2 & Cov_{2,3}\\
    Cov_{3,1} & Cov_{3,2} & \sigma_3^2\\
	\end{matrix}
	\right]$$

where

$$\sigma_i^2 = E[(return_i-E[return_i])^2]$$

$$ Cov_{i,j} = E[(return_i-E[return_i])\cdot (return_j-E[return_j])] $$

The mean of three securities' monthly return is:

| Security Code | Mean Return |
|:-------------:|:-----------:|
|    002507     |  0.033364   |
|    002036     |  0.020808   |
|    600519     |  0.028398   |

The variance-covariance matrix of three securities' monthly return is:

| Security Code |   002036   |  600519   |   002507   |
|:-------------:|:----------:|:---------:|:----------:|
|    002036     | 0.03751847 |           |            |
|    600519     | 0.00182164 | 0.0077167 |            |
|    002507     | 0.00754047 | 0.0023015 | 0.01302531 |



## Q(1b): Efficient Frontier

In this question, we are required to build the efficient frontier with the three given securities, with the y-axis begin the expected return and the x-axis being the standard deviation.

Each dot in the plot represents a portfolio build with these three securities:

$$ mean:\ \mu_p=(w_1 w_2 w_3)^T\cdot(\mu_1 \mu_2 \mu_3 ) $$


$$ standard\ deviation:\ \sigma_p=\sqrt{(w_1 w_2 w_3)^T\cdot\Sigma\cdot(w_1 w_2 w_3})$$

$$ s.t.\ ||(w_1 w_2 w_3)||_1=1 $$

And the efficient frontier is a quadratic function of mean and standard deviation: $$ \mu_p = \frac{a\sigma_p^2-2b\sigma_p+c}{\Delta}$$

where

$$ a=1\cdot \Sigma^{-1}\cdot 1 $$
$$ b=1\cdot\Sigma^{-1}\cdot \mu$$
$$ c=\mu\cdot\Sigma^{-1}\cdot \mu$$
$$ \Delta=ac-bb$$

The result is plotted as below, where the blue dots are all possible portfolios, and the red line is the efficient frontier:

![Q1.1(b). Efficient Frontier without Risk-free asset](./Q1.1/Q1.1(b).png)



## Q(1c): Portfolio with Minimum Standard Deviation

The portfolio with minimum standard deviation is represented by $\pi_g$, where

$$ \pi_g=\frac{\Sigma^{-1}\cdot 1}{a}$$

The weight for the portfolio with minimum standard deviation is:
$$ \pi_g = (0.06367325, 0.64471892, 0.29160782)$$

The corresponding expected return and standard deviation is: $ \mu_p=0.0274;\ \sigma_p=0.0760$

| Security Code |   002036   |   600519   |   002507   |
| :-----------: | :--------: | :--------: | :--------: |
|    Weight     | 0.06367325 | 0.64471892 | 0.29160782 |



## Q(1d): Efficient Frontier with Risk-free Asset

If we introduce a risk-free asset, then the effective frontier is supposed to be:
$$ \mu_p = r_f+\sigma_p \sqrt{c-2r_fb+r_f^2a}$$

The result is plotted as below, where the blue dots are all possible portfolios, the red line is the efficient frontier without risk-free asset, and the green line is the efficient frontier with risk-free asset:

![Q1.1(d). Efficient Frontier with Risk-free Asset](./Q1.1/Q1.1(d).png)

\pagebreak


## Q(1e): Sharpe Ratio for the Tengency Portfolio

The Sharpe Ratio is calculated by $$ Sharpe\ Ratio = \frac{\mu_m-rf}{\sigma_m}$$
Out tangency portfolio's Sharpe Ratio is 0.34073

The market portfolio's weights are:

| Security Code |   002036   |   600519   |   002507   |
| :-----------: | :--------: | :--------: | :--------: |
|    Weight     | 0.02793284 | 0.55968293 | 0.41238423 |


## Q(2a): First Pass Regression

In question 2.2, we downloaded monthly returns for all the stocks in Shanghai Stock Exchange A shares from Jan. 2013 to Dec. 2018. Then, we use first-pass regression and second-pass regression to test whether CAPM theory holds for securities in Chinese stock market. The market return is approximated by the monthly return of CSI300 Index, and the risk-free rate is already provided.

The data is presented in the following form:

Shanghai Stock Exchange A Share data:

| Stkcd  |   Trdmnt   |  Mretwd  |
| :----: | :--------: | :------: |
| 600000 | 2013-01-01 | 0.157258 |
| ...... |   ......   |  ......  |

where "Stkcd" is the code for securities, "Trdmnt" is the trading date, and "Mretwd" is the corredponding monthly return of the security



Risk-free rate data (**with $r_f$ divided by 100**):

|   Clsdt    | Nrrmtdt  |
| :--------: | :-----:  |
| 2013-01-01 | 0.002466 |
|   ......   | ......   |

where "Clsdt" is the date when the data is collected, and "Nrrmtdt" is the corresponding risk-free rate



Market return data:

| Indexcd |   Month    |  Idxrtn  |
| :-----: | :--------: | :------: |
|   300   | 2013-01-01 | 0.064975 |
| ......  |   ......   |  ......  |

where "Indexcd" is the index code for CSI300, "Month" is the month when this data is collected, and "Idxrtn" is the corresponding market return



We first merged three files into a single csv file, and **dropped stocks with listing date after 2013.01**. The data is shown below:

| Stkcd  |   Trdmnt   |  Mretwd  | $r_f$    |  $r_m$   |
| :----: | :--------: | :------: | :----:   | :------: |
| 600000 | 2013-01-01 | 0.157258 | 0.002466 | 0.064975 |
| ...... |   ......   |  ......  | ......   |  ......  |

Then we start to run the first regression.

In the first-pass regression, we run periodic cross-sectional regressions to regress each stock across T periods:

$$ r_{it}-r_{ft} = \alpha_i+\beta_i(r_{mt}-r_{ft})+\epsilon_{it}\ \forall t\in T$$

to calculate each stock's beta.

A sample regression plot for security 601333 is shown as below:

![Q1.2(a). First-Pass Regression Sample Result](./Q1.2/Q1.2(a).png)

and the first-pass regression results are:

Stock	Alpha	Beta	ri-rf

|     | Security Code | $\alpha$ | $\beta$  | $\overline {r_i-r_f}$ |
| ---:|:-------------:|:--------:|:--------:|:---------------------:|
|   0 |   600000.0    | 0.006729 | 0.691666 |       0.008905        |
|   1 |   600004.0    | 0.011361 | 0.870127 |       0.014099        |
|   2 |   600005.0    | 0.004312 | 1.048718 |       0.011767        |
|   3 |   600006.0    | 0.009654 | 0.656346 |       0.010664        |
|   4 |   600007.0    | 0.002727 | 0.711445 |       0.004965        |
|     |    ......     |  ......  |  ......  |        ......         |
The rest of the first-pass regression is stored in file "RegressionResult.csv"

The stock with the smallest beta:

|      Stock Code      |  600087   |
|:--------------------:|:---------:|
|       $\alpha$       | -0.047590 |
|       $\beta$        | -1.767840 |
| $\overline{r_i-r_f}$ | -0.038901 |

The stock with the largest beta:

|      Stock Code      |  600090  |
|:--------------------:|:--------:|
|       $\alpha$       | 0.022410 |
|       $\beta$        | 2.402965 |
| $\overline{r_i-r_f}$ | 0.011230 |


## Q(2b): Second Pass Regression

In second pass regression, we run average cross-sectional regressions for each security:

$$ \overline{r_i-r_f} = \gamma_0+\gamma_1 b_i$$

If CAPM holds, $\gamma_0=0;\ \gamma1=\overline{r_m-r_f}$

The result of the second-pass regression is shown as below:

![Q1.2(b). Second-Pass Regression Result](./Q1.2/Q1.2(b).png)



\pagebreak
The summary table of OLS regression:

|    Dep. Variable: |                y | R-squared:          | 0.017    |
| ----------------: | ---------------: | ------------------- | -------- |
|            Model: |              OLS | Adj. R-squared:     | 0.015    |
|           Method: |    Least Squares | F-statistic:        | 15.57    |
|             Date: | Sat, 09 Nov 2019 | Prob (F-statistic): | 8.55e-05 |
|             Time: |         16:39:12 | Log-Likelihood:     | 2931.5   |
| No. Observations: |             930 | AIC:                | -5859.   |
|     Df Residuals: |             928 | BIC:                | -5849.   |
|         Df Model: |                1 |                     |          |
|  Covariance Type: |        nonrobust |                     |          |

|       |    coef | std err |        t | P>\|t\| | [0.025 | 0.975] |
| ----: | ------: | ------: | -------: | ------: | -----: | ------ |
| const |  0.0045 |   0.001 |   3.773  |   0.000 |  0.002 | 0.007 |
|    x1 |  0.0047 |   0.001 |   3.946  |   0.000 |  0.008 | 0.007  |

|       Omnibus: | 427.329 | Durbin-Watson:    | 1.926     |
| -------------: | ------: | ----------------- | --------- |
| Prob(Omnibus): |   0.000 | Jarque-Bera (JB): | 6352.656  |
|          Skew: |   1.701 | Prob(JB):         | 0.00      |
|      Kurtosis: |  15.344 | Cond. No.         | 6.89      |

Then we need to test whether CAPM holds based on our regression result.

Test $\gamma_0 = 0$:

$H_0:\ \gamma_0=0$; $H_1:\ \gamma_0\ne0$

|     | coef   | std err | t     | P>\|t\| | [0.025 | 0.975] |
| --- | ------ | ------- | ----- | ------- | ------ | ------ |
| c0  | 0.0045 | 0.001   | 3.773 | 0.000   | 0.002  | 0.007  |

$P=0.000;\ t=3.373$; reject $H_0$ under $\alpha=0.05$ significance level


Test $\gamma_1=\overline{r_M-r_f}$:

$H_0:\ \gamma_1=\overline{r_M-r_f}$; $H_1:\ \gamma_1\ne\overline{r_M-r_f}$

|     | coef   | std err | t     | P>\|t\| | [0.025 | 0.975] |
| --- | ------ | ------- | ----- | ------- | ------ | ------ |
| c0  | 0.0047 | 0.001   | 1.304 | 0.193   | 0.002  | 0.007  |

$P=0.193;\ t=1.304$; reject $H_0$ under $\alpha=0.05$ significance level

Therefore, our conclusion is that CAPM does not hold.

\pagebreak

# Problem set 2: Momentum Strategy

## Q(a): Mean and Variance of Annualized Returns

In this question, we downloaded the return of all A shares from CSMAR with sample period Jan 2010 to June 2019, and replicated a monentum strategy as Titman and Jegadees did in their 2011 paper (Titman&Jegadees, 2011).

We first determined the winner and loser portfolio at the beginning of each month using the past six-month cumulative return, which is calculated by $$1+r_c=(1+r_1)(1+r_2)(1+r_3)...(1+r_6)$$
Then we hold the portfolio for one month where each stock in our portfolio has the same weight, and calculate each portfolio's one-month return. At the beginning of each month, we redetermine the winner and loser portfolio and hold for this month according to performance of the past six month. Repeat the process for each month and yield a time series of each portfolio's return.

We annualized the monthly return $r_a=12r_m$, where $r_a$ is the annual return, $r_m$ is the monthly return. Then we calculated the mean and variance. The result is listed as below.

| Portfolio Group | Monthly Return | Annual Return | Mean     | Variance |
| --------------- | -------------- | ------------- | -------- | -------- |
| 1 (Loser)       | .2053001       | 2.463602      | .2460941 | 1.478454 |
| 2               | .1857444       | 2.228932      | .184566  | 1.254344 |
| 3               | .1809964       | 2.171957      | .171586  | 1.157416 |
| 4               | .1616456       | 1.939748      | .1658929 | 1.141497 |
| 5               | .1549084       | 1.858901      | .1796667 | 1.11615  |
| 6               | .1461451       | 1.753742      | .1515947 | 1.155988 |
| 7               | .1353385       | 1.624063      | .1410179 | 1.132809 |
| 8               | .1387516       | 1.665019      | .1224207 | 1.152431 |
| 9               | .1177433       | 1.412919      | .1001712 | 1.193148 |
| 10 (Winner)     | .1279469       | 1.535362      | .0188642 | 1.356603 |

We notice that the loser portfolio (g=1) has the highest return and highest return, which indicates that **the reverse effect may be more significant than momentum in Chinese market**.

## Q(b): Three-factor Model

We use the three-factor series provided by our TA and USTF to calculate alphas for winner and loser portfolios we constructed in question (a). where the returns of our momentum strategy is defined as the difference in returns between the winner and loser portfolios. Then, we regressed the returns of the momentum strategy using the three factors:

$$ R_{it} = E(R_{it})+\beta_{i,RM}R_M+\beta_{i,SMB}SMB+\beta_{i,HML}HML+\epsilon_{it}$$
where
$R_M$ is the original market excess return in CAPM;
$SMB$ means Small Minus Big, i.e., the return of a portfolio of small stocks; and
$HML$ means High Minus Low, i.e., the return of a portfolio of stocks with a high book-to-market ratio in excess of the return on a portfolio of stocks with a low book-to-market ratio
in excess of thereturn on a portfolio of large stocks

The regression result is displayed below, where the return is the computed as annual return:

\pagebreak
Winner Portfolio:

![Q1.2(b). Winner Portfolio Regression Result](./Q2/Q2(b)winner.png)
Loser Portfolio:

![Q1.2(b). Loser Portfolio Regression Result](./Q2/Q2(b)loser.png)


\pagebreak
Strategy:

![Q1.2(b). Momentum Strategy Regression Result](./Q2/Q2(b)strategy.png)

After regressing the momentum strategy return (winner portfolio return-loser portfolio return), we found that the alpha has a negative value of -0.0147204. This indicates that **the Momentum strategy does not work in Chinese A share market** (cannot yield an abnormal **positive** excess return) when using 6 month cumulative return to sort the portfolios.

## Q(c): Pick the Best Momentum Strategy
In this question, we repeat the momentum strategy is (a) but with different choosing criteria. This time we pick stocks to build portfolios according to the past 3 to 12 month's cumulative return, and do regressions to determine their mean, variance and alpha, where the return is the computed as annual return.

| Criteria | Winner Mean | Winner Variance | Winner Alpha |
| -------- | ----------- | --------------- | ------------ |
| 3 month  | -.0624631   | 1.313303        | -.0142746    |
| 4 month  | -.0203283   | 1.400575        | -.0115399    |
| 5 month  | -.0118709   | 1.393320        | -.0105901    |
| 6 month  | .0188642    | 1.356603        | -.0079649    |
| 7 month  | .0099274    | 1.367587        | -.0074170    |
| 8 month  | -.0091916   | 1.308192        | -.0077624    |
| 9 month  | -.0107012   | 1.325665        | -.0076206    |
| 10 month | -.0366751   | 1.289689        | -.0090532    |
| 11 month | -.0227835   | 1.312117        | -.0079058    |
| 12 month | -.0023513   | 1.330433        | -.0060980    |


| Criteria | Loser Mean | Loser Variance | Loser Alpha |
| -------- | ---------- | -------------- | ----------- |
| 3 month  | .2684462   | 1.648614       | .0132296    |
| 4 month  | .2499141   | 1.523448       | .0113941    |
| 5 month  | .2571096   | 1.544343       | .0105484    |
| 6 month  | .2460941   | 1.478454       | .0097541    |
| 7 month  | .1775504   | 1.397584       | .0058569    |
| 8 month  | .1784306   | 1.424568       | .0066509    |
| 9 month  | .1867051   | 1.423378       | .0078991    |
| 10 month | .1769478   | 1.448498       | .0072352    |
| 11 month | .1941684   | 1.465229       | .0086521    |
| 12 month | .1940301   | 1.442279       | .0088904    |


| Criteria | Strategy Mean | Strategy Variance | Strategy Alpha |
| -------- | ------------- | ----------------- | -------------- |
| 3 month  | -.3309093     | .4923229          | -.0294206      |
| 4 month  | -.2702423     | .5291213          | -.0248503      |
| 5 month  | -.2689805     | .5179156          | -.0230549      |
| 6 month  | -.2272299     | .5015393          | -.0196354      |
| 7 month  | -.1676230     | .4932442          | -.0151908      |
| 8 month  | -.1876222     | .4908018          | -.0163313      |
| 9 month  | -.1974063     | .4968291          | -.0174378      |
| 10 month | -.2136230     | .5430513          | -.0182073      |
| 11 month | -.2169520     | .5147300          | -.0184761      |
| 12 month | -.1963813     | .5284036          | -.0169057      |


The alphas of all momentum strategy are negative, and none of these strategy generates an abnormal **positive** excess return. Therefore, momentum strategy fails.

Closer examination at the performance of the winner and loser portfolio reveals that the loser portofolio has a larger mean as well as a positive alpha, while the winner portfolio has a lower mean and negative alpha. So the momentum crash in Chinese Market may be a result of strong reverse effect.

The strategy's performance with 7, 8, 12 is relativaly better, and using 7 month cumulative return to build portfolio yields the largest mean & alpha, which means the loss under this kind of strategy is minimized.

## Q(d): Momentum Strategy Failures
10 month with the worst performance:

| Month   | Strategy Return | Market Risk Premiem |
| ------- | --------------- | ------------------- |
| 2015-06 | -26.16%         | -11.13%             |
| 2015-08 | -19.10%         | -17.21%             |
| 2015-09 | -15.25%         | -0.41%              |
| 2015-05 | -15.12%         | 14.75%              |
| 2012-01 | -14.37%         | 3.94%               |
| 2015-01 | -12.99%         | 0.10%               |
| 2015-10 | -12.72$         | 13.71%              |
| 2019-02 | -12.57%         | 15.71%              |
| 2018-11 | -11.40%         | 0.21%               |
| 2014-03 | -10.05%         | -2.32%              |

Summary:
According to Kent Daniel and Tobias J. Moskowitz in Momentum Crashes, they identified the momentum will experience negative return “in panic states, following market declines and when market volatility is high.” (Kent & Tobias, 2019)
As the following figure shows, this phenomenon of Momentum Crash has similar pattern in the Chinese A share market:

![Q2(d). Rist Premium with Worst Performance Month](./Q2/Q2(d).png)


- Finding1: **The extreme loss occurred when market declines**.
The extreme loss of momentum strategy, which happened in 2015/06, 2015/08, 2015/09, 2015/05 is clustered, corresponding to a market decline in 2015.
- Finding2: **Momentum crashes when the market is volatile**.
Around six of the ten worst months of momentum occurred at the peak of some change in market risk premium, that is 2014-02, 2015-01, 2015-08, 2015-10, 2018-11 and 2019-02 respectively. This indicates that Momentum fails during some periods that the return from stock market is volatile.

![Q2(d). Winner & Loser Portfolio Return with Worst Performance Month](./Q2/Q2(d)worst.png)

Finding3: **The crash perfomance is mianly attribute to the reversal perfomance of the loser(the short side)**. For example, in June, 2015 and August, where the momentum performs worst, the loser portfolio yeild a very high return and achieve its peak, exhibited in the loser portfolio return in Figure8.

\pagebreak
# Reference
Bali, Turan G., Robert F. Engle, and Scott Murray. Empirical asset pricing: the cross
section of stock returns. John Wiley & Sons, 2016.

Kent, D. & Tobias, J.M. (2015, December 17). Momentum crashes. Journal of Financial Economics. Retrieved from https://www.sciencedirect.com/science/article/pii/S0304405X16301490

Jegadeesh, N. & Titman, S. (2011, August 29). Momentum. Available at SSRN: https://ssrn.com/abstract=1919226 or http://dx.doi.org/10.2139/ssrn.1919226
