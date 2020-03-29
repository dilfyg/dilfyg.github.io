---
title: "R COOKBOOK"
date: 2019-03-29
tags: [R]
#header:
#  image: ""
excerpt: "R Studio, Enzyme Kinetics Graphs"
---
Showcasing some automation I did whilst at University studying Biochemistry.

## Introduction
Whilst at University I found that I was constantly having to remake and manipulate code in order to plot different graphs depending on the data that I had. This was time consuming and I wanted a way around this.

So I made a number of R scripts that I could run on a given dataset that I obtained in the lab, these would calculate the values I needed and plot a graph.

## R Code
Michaelis Menten plot code:
```r
### draw a Michaelis Menten plot  and determine Vmax and Km ###

# read in the data using the read.csv function (or xlsx etc.)
read.csv(filelocation)

# assign data to s and v from the data in csv.
# v needs to be velocity and s the substrate concentration
s <- c()
v <- c()

# plot curve

plot(s,v)

# Fitting process with estimates of Vmax and Km

mmModel <- nls(v~Vm*s/(Km+s), start=list(Vm=set this to an ~ Vm, Km=Set this to ~ Km))

#First, create a range over which to evaluate the function
# then evaluate the function over that range
# and finally add the points to the plot with the points() function.


s <- seq(min(s), max(s), length=100)
v <- predict(mmModel, list(s=s))
points(s, v, type='l', col='red')

# view the output with summary().
# The fitted values of Vmax and Km can be extracted with the coef() function.

summary(mmModel)
coef(mmModel) # the estimated values of Km and Vmax will be printed
```

Lineweaver Burk plot code:
```r
# draw a lineweaver burk plot 1/v vs 1/[S] and determine Vmax and Km

# import dataset
read.csv(location of file)

# input dataset (assign to s and v)
s <- c()
v <- c()

# LB model Km = gradient/Intercept, Vm = 1/intercept
y.LB <- 1/v
x.LB <- 1/s
Q1.LB <- lm(y.LB ~ x.LB)
summary(Q1.LB)

# Plot the data
plot(x.LB,y.LB,xlab = "1 / [s]", ylab = "1 / v", main = "Lineweaver-Burk plot")
abline(Q1.LB)

### Lineweaver-Burk Km = gradient/Intercept, Vmax = 1/Intercept
Q2.LB <- lm(y.LB ~ x.LB)
summary(Q2.LB)
Int.LB <- summary(Q1.LB)$coefficients[1, 1]
Grad.LB <- summary(Q1.LB)$coefficients[2, 1]
Int.tv.LB <- summary(Q1.LB)$coefficients[1,3]
Grad.tv.LB <- summary(Q1.LB)$coefficients[2,3]

# calculate Km from the LB plot
Km.LB <- Grad.LB / Int.LB

# calculate the Std. Error for Km
Km.LB.SE <- Km.LB * sqrt((1/Grad.tv.LB)^2 + (1/Int.tv.LB)^2)

# output the values for Km and its Std. Error from the LB plot
Km.LB
Km.LB.SE

#calculate Vmax from the LB plot
Vmax.LB <- 1/Int.LB
```

Eadie-Hofstee plot code:
```r
# import dataset
read.csv(location of file)

# input dataset
s <- c()
v <- c()

# drawing a hanes woolf
y.EH <- v
x.EH <- v/s

Q1.EH <- lm(y.EH ~ x.EH)
summary(Q1.EH)

# Plot the data
plot(x.EH,y.EH,xlab = "1/s", ylab = "1/v", main = "Eadie-Hofstee Plot")
abline(Q1.EH)

### Hanes Woolf Plot Km = -Int.EH, Vmax = 1/Grad.EH
Q2.EH <- lm(y.EH ~ x.EH)
summary(Q2.EH)
Int.EH <- summary(Q2.EH)$coefficients[1, 1]
Grad.EH <- summary(Q2.EH)$coefficients[2, 1]
Int.tv.EH <- summary(Q2.EH)$coefficients[1,3]
Grad.tv.EH <- summary(Q2.EH)$coefficients[2,3]

#calculate Vmax from the EH plot
Vmax.EH <- Int.tv.EH
Vmax.EH

# calculate Km from the HW plot
Km.EH <- -(Grad.EH)

# calculate the Std. Error for Km
Km.EH.SE <- Km.EH * sqrt((1/Grad.tv.EH)^2 + (1/Int.tv.EH)^2)

# output the values for Km and its Std. Error from the LB plot
Km.EH
Km.EH.SE
```
