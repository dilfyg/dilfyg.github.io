---
title: "R COOKBOOK"
date: 2019-03-29
tags: [R]
#header:
#  image: ""
excerpt: "R Studio, Enzyme Kinetics Graphs"
---

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
