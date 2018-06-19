#tatest

tatest is called ta-test, a modified two-sample or two-condition t-test where "a" is alpha that is significance level. ta-test has much lower type I error rate than t-test in biological experiments with small samples but it has the same statistical power with t-test. ta-test can be used to test for single null hypothesis with one tail ("less", "greater") or two tails("two.sided") but also can be used to perform multiple hypotheses. 

## Usage

`ta.test(X, nci=NULL, na, nb, alpha=0.05, paired=FALSE, eqv=TRUE,LOG=c("NULL","LOG2","LOG","LOG10"),
 alternative = c("two.sided", "less", "greater"), distr="norm")`

  where 
   **X**: a numeric vector data or a numeric matrix (two columns) dataset with two conditions or groups A and B having sample size na and nb, respectively. X is also a datasheet of multiple variables (for example, genes ) with two groups A and B and information (character) columns(see an example Ecadherin)

    **nci**: a numeric int value, column numbers for strings or information of data. If X is a numeric vector data or a numeric matrix (two columns) dataset, then nci=NULL, otherwise, nci is required to be non-zeror integer, meaning that at least one column is reserved as information of data

     **alpha**: a numeric constant, statistical cutoff for significance level. If dataset X is multiple variables (rows) (variable number is over 1000) and nci>0, then alpha is taken default values of 0.05 and 0.01.

      **paired**: a logical indicating whether you want a paired t-test.

       *eqv*: a logical variable indicating whether to treat the two variances as being equal. If TRUE then the pooled variance is used to estimate the variance otherwise the Welch (or Satterthwaite) approximation to the degrees of freedom is used.

        **LOG**: logarithm. If data are not taken logarithm convertion, then LOG=NULL , if data are converted into logarithm with base 10, 2 or natural logarithm, then correspondingly, LOG="LOG10", "LOG2" or "LOG".

        **alternative**: a character string specifying the alternative hypothesis, must be one of "two.sided" (default), "greater" or "less". You can specify just the initial letter.

        **distr**:data distribution for estimating omega. Default is normal distribution.  In current version, we just consider three types of data distributions: "negative distribution"(or "NB"), "norm"(or "normal"), and "unif"(or "uniform").  User can choose one of them according to user's data distribution.

## Details:

The formula interface is only applicable for the 2-sample tests. 

alternative = "greater" is the alternative that x has a larger mean than y.

If paired is TRUE then both x and y must be specified and they must be the same length. Missing values are silently removed (in pairs if paired is TRUE). 

If var.equal is TRUE then the pooled estimate of the variance is used. By default, if var.equal is FALSE, then the variance is estimated separately for both groups and the Welch modification to the degrees of freedom is used.

If the input data are effectively constant (compared to the larger of the two means) an error is generated.
When pooled variance is larger, ttest is equvalent to t.test; When pooled variance is small, t-tavalue of ttest is smaller than than that of t.test; When the pooled variance is zeror, t-value of t.test is not defined but tvalue of ttest is still defined.  

Omega is dependent on alpha value.

ta.test can be used to test for differentially expressed genes in microarray data, proteomic data and RPPA data but cannot be used to count data of RNA-seq reads. 

## output value
If X is single-hypothesis test data, then  a list similar to t.test containing the following components:

*statistic*: the value of the t-statistic.

*parameter*: the degrees of freedom for the t-statistic.

*p.value*: the p-alue for the test.

*conf.int*: a confidence interval for the mean appropriate to the specified alternative hypothesis.

*estimate*: the estimated mean or difference in means depending on whether it was a one-sample test or a two-sample test.

*null.value*: the specified hypothesized value of the mean or mean difference. 

*alternative*: a character string describing the alternative hypothesis.

*method*: a character string indicating what type of t-test was performed.

*data.name*: a character string giving the name(s) of the data.

*omega*:a threshold for rho in tatest. 

If X is microarray, RPPA data, then a list is datasheet including t01,pv01, t05 and p05 columns

## examples
`X<-c(112,122,108,127)`

`Y<-c(302, 314,322,328)`

`dat<-cbind(X,Y)`

`ta.test(X=dat,nci=NULL, na=4,nb=4, eqv=TRUE, alpha=0.05, LOG="NULL", alternative = "two.sided")`

   `Two Sample t-test`

  `data:  XA and XB`

   `t = -94.353, df = 6, p-value = 9.55e-11`

   `alternative hypothesis: true difference in means is not equal to 0.95`

   `95 percent confidence interval:`

     `-96.79949 -91.90566`

     `sample estimates:`

    `mean of x mean of y` 

    `117.25    316.50` 

    `load(Ecadherin.rda)`

    `head(Ecadherin)`

   `res<-ta.test(X=Ecadherin,nci=1, na=3,nb=3, eqv=TRUE, LOG="LOG2", alternative = "two.sided")`

## Note
[1] if X contains negative values in two conditions, them ta.test is almost equivalent to t.test
[2] if X is taken logarithm using base 10, 2 or natural logarithm, then user must set LOG="LOG10", "LOG2" or "LOG", otherwise, the result is incorrect. 



