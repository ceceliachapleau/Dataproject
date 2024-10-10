# Dataproject
The purpose of this project is to show the impact of gross domestic product per capita, out-of-pocket health expenditures per capita, private health expenditures per capita, and government health expenditures per capita on life expectancy.  

getwd() 
setwd("") 
mydata = read.csv("")  
summary(mydata)

#define variables
y<-mydata$LifeExp
x1<-mydata$GDPPC
x2<-mydata$OutofpocketHEpc
x3<-mydata$privateHEpc
x4<-mydata$governmentHEpc

#draw distogram of y
hist(y)
#get the variance of the variables.
v=var(y)
v
v1=var(x1)
v1
v2=var(x2)
v2
v3=var(x3)
v3
v4=var(x4)
v4

modelur<-lm(y~x1+x2+x3+x4)
summary(modelur)

#for F-test
modelr<-lm(y~x2+x3)
summary(modelr)
anova(modelr, modelur)

#get the predicted y and compare to smooth density of y
pred<-fitted(modelur)
d1<-density(y)
plot(d1)
d2<-density(pred)
plot(d2)

#heteroscedasticity test
install.packages("lmtest")
library(lmtest)
bptest(modelur)

#White heteroscedasticity test command
install.packages("whitestrap")
library(whitestrap)

white_test(modelur)

#multicollinearity test
install.packages("mctest")
library(mctest)
imcdiag(modelur, method = "VIF")

#outlier test
install.packages("car")
library(car)

outliers <- outlierTest(modelur)
print(outliers)

#Ramsey test for nonlinearity
yhat<-pred
uhat<-y-pred
yhat2<-yhat*yhat
yhat3<-yhat*yhat*yhat
yhat4<-yhat2*yhat2
modelaux<-lm(uhat~yhat2+yhat3+yhat4)
summary(modelaux)

#F test after dropping covariates not significant at 5%
modelr<-lm(y~x2+x4)
summary(modelr)
anova(modelr, modelur)
