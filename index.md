---
title       : Basic Satistical Model Comparison demo
subtitle    : Project Application for Coursera Developing Data Products
author      : Abel Pardo-Lopez
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Introduction

The objective of this application is to show the difficulties of finding a proper statistical model.  
In this application we have a quadratic function with statistical error, and 3 statistical models (linear, quadratic and cubic) than try to capture the "original" function. The source of the application is in [Github](https://github.com/abelpardolopez/ShinyApp]).  

The application displays a quadratic function wich points have been obtained with a independent variable $X$ from an uniform distribution between 0 and 2, and a dependent varible $Y$ through following formula:  $y= x^{2}+K*x+B+\epsilon$  
There are 3 slider to show different behavior of the statistical models, when original function changes:  
-- **Standard deviation slider** increase or reduce the standard deviation of the error($\epsilon$ in previous formula).  
-- **K Slope slider** changes the linear constant ($K$ in the previous formula), from  -2 to 2.  
-- **Independent term slider** changes the constant fo the previous formula ($B$), from  -1 to 1.  

Also there are 3 checkboxes to show or not the linear, quadratic and cubic models on the plot.  In following slides are shown application results for the defaults values. The function for default values is: $y = x^{2}+\epsilon$

---  
## Server Code  
In following lines are shown server code,  to generate plot and statistical models.  


```r
x<-runif(300,min=0,max=2)       # Independent variables
eps <- rnorm(300,mean=0,sd=0.5) # Standadard Error
# Function
y <- x*x+0*x+0+eps # The default values for slope  and for independent term is 0
Data<-data.frame(y, x,x^2,x^3) # Data frame for ggplot and stats model
# Plot Code
p <-ggplot(Data,aes(x=x,y=y))+geom_point(size=3)
p<-p+geom_smooth(method= "lm",color="red")
p<-p+geom_smooth(method= "lm",formula = y ~ poly(x, 2),color="blue")
p<-p+geom_smooth(method= "lm",formula = y ~ poly(x, 3),color="green")
# Satistical models
modLin <- lm(y ~ x, Data) # Linear Model
modQuad <- lm(y ~ x+x.2, Data) # Quadartic Model
modCubic <- lm(y ~ x+x.2+x.3, Data) # Cubic Model
resume <- data.frame(rbind(glance(modLin),glance(modQuad),glance(modCubic)))%>%select(r.squared,AIC,BIC);rownames(resume)=c("Linear","Quadratic","Cubic ")
resumeLin<-tidy(modLin);resumeQuad<-tidy(modQuad);resumeCubic<-tidy(modCubic)
```

---  
## Plot
Nex plot shows and example of the application plot using default values:  
<img src="assets/fig/unnamed-chunk-1-1.png" title="plot of chunk unnamed-chunk-1" alt="plot of chunk unnamed-chunk-1" style="display: block; margin: auto;" />

Next slide shows an example of table results of the application usign slider default values. It is shown the model stats comparison ($R^{2}$, AIC, BIC). Following tables shows the statistical results of the estimated constants from the models.

---  
##  Statistical Models Results
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Fri Dec 09 08:45:15 2016 -->
<table border=1>
<tr> <th>  </th> <th> r.squared </th> <th> AIC </th> <th> BIC </th>  </tr>
  <tr> <td align="right"> Linear </td> <td align="right"> 0.84 </td> <td align="right"> 483.93 </td> <td align="right"> 495.04 </td> </tr>
  <tr> <td align="right"> Quadratic </td> <td align="right"> 0.88 </td> <td align="right"> 404.17 </td> <td align="right"> 418.99 </td> </tr>
  <tr> <td align="right"> Cubic  </td> <td align="right"> 0.88 </td> <td align="right"> 405.08 </td> <td align="right"> 423.60 </td> </tr>
   </table>
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Fri Dec 09 08:45:15 2016 -->
<table border=1>
<tr> <th>  </th> <th> term </th> <th> estimate </th> <th> std.error </th> <th> statistic </th> <th> p.value </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> (Intercept) </td> <td align="right"> -0.66 </td> <td align="right"> 0.06 </td> <td align="right"> -10.94 </td> <td align="right"> 0.00 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> x </td> <td align="right"> 2.05 </td> <td align="right"> 0.05 </td> <td align="right"> 39.08 </td> <td align="right"> 0.00 </td> </tr>
   </table>
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Fri Dec 09 08:45:15 2016 -->
<table border=1>
<tr> <th>  </th> <th> term </th> <th> estimate </th> <th> std.error </th> <th> statistic </th> <th> p.value </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> (Intercept) </td> <td align="right"> -0.10 </td> <td align="right"> 0.08 </td> <td align="right"> -1.31 </td> <td align="right"> 0.19 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> x </td> <td align="right"> 0.36 </td> <td align="right"> 0.18 </td> <td align="right"> 1.98 </td> <td align="right"> 0.05 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> x.2 </td> <td align="right"> 0.84 </td> <td align="right"> 0.09 </td> <td align="right"> 9.65 </td> <td align="right"> 0.00 </td> </tr>
   </table>
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Fri Dec 09 08:45:15 2016 -->
<table border=1>
<tr> <th>  </th> <th> term </th> <th> estimate </th> <th> std.error </th> <th> statistic </th> <th> p.value </th>  </tr>
  <tr> <td align="right"> 1 </td> <td> (Intercept) </td> <td align="right"> -0.17 </td> <td align="right"> 0.10 </td> <td align="right"> -1.67 </td> <td align="right"> 0.10 </td> </tr>
  <tr> <td align="right"> 2 </td> <td> x </td> <td align="right"> 0.80 </td> <td align="right"> 0.46 </td> <td align="right"> 1.73 </td> <td align="right"> 0.08 </td> </tr>
  <tr> <td align="right"> 3 </td> <td> x.2 </td> <td align="right"> 0.28 </td> <td align="right"> 0.55 </td> <td align="right"> 0.51 </td> <td align="right"> 0.61 </td> </tr>
  <tr> <td align="right"> 4 </td> <td> x.3 </td> <td align="right"> 0.19 </td> <td align="right"> 0.18 </td> <td align="right"> 1.04 </td> <td align="right"> 0.30 </td> </tr>
   </table>
