library(sampling)
library(ggplot2)
library(GGally) # ggplot extension for correlation matrix
library(gpairs)

# filename for original dataset : insurance.csv
# filename for the sample of 1000 records : sample.csv

# load the original data 
setwd("") # path to dataset

original.data <- read.csv("insurance.csv", col.names = c("age", "gender", 
                            "bmi", "children","smoker", "region", "premium"))

# look for missing data
which(is.na(original.data))
#which(complete.cases(original.data)==FALSE)

# stratified sample based on the region - 250 samples each
strat <- strata(data=original.data, stratanames = "region", size = c(250,250,250,250),
                  method = "srswor")
sampledata <- getdata(original.data, strat)

View(sampledata)

# save the sample data
#write.csv(x=sampledata, "C:/ARCHANA/Boston University MS Applied Data Analytics/METCS555 - Data Analysis & Visualization/Final Project/sample.csv")

# read sample data of 1000 observations
data <- read.csv("sample.csv")

View(data)

# drop the columns related to prob, stratum, row-id & region
data <- subset(data, select = -c(X, ID_unit, Prob, Stratum, region))

# analyze the spread & distribution of data

# boxplot for premium-charged
ggplot(data=data, aes(x=premium))+
  geom_boxplot(outlier.size=1, fill="steelblue")+
  ggtitle("Boxplot of Premium Charged")+
  labs(x="Premium Amount in $")+
  theme_minimal()

summary(data$premium)

# IQR calculation
outlier_iqr <- function(x){
  q3 <- summary(x)[5]
  q1 <- summary(x)[2]
  iqr <- q3 - q1
  lowrange <- q1-(1.5*iqr)
  highrange <- q3+(1.5*iqr)
  outliers <- which ((x > highrange) | (x < lowrange))
  return(outliers)
}

outlier_iqr(data$premium)
# 96 outliers

# plot age vs premium
ggplot(data=data, aes(x=age, y=premium))+
  geom_point()+
  ggtitle("Policyholder's Age vs Premium Charged")+
  labs(x="Policyholder's Age", y="Premium Amount in $")+
  theme_minimal()

# plot bmi vs premium
ggplot(data=data, aes(x=bmi, y=premium))+
  geom_point()+
  ggtitle("Policyholder's BMI vs Premium Charged")+
  labs(x="Policyholder's BMI", y="Premium Amount in $")+
  theme_minimal()

# plot smoking behavior vs premium
ggplot(data=data, aes(x=smoker, y=premium))+
  geom_boxplot(outlier.size=1, fill="sienna1")+
  ggtitle("Boxplot of Premium Charged by smoking behavior")+
  labs(x="Smoker - Yes / No", y="Premium Amount in $")+
  theme_minimal()

# plot gender vs premium
ggplot(data=data, aes(x=gender, y=premium))+
  geom_boxplot(outlier.size=1, fill="hotpink1")+
  ggtitle("Boxplot of Premium Charged by smoking behavior")+
  labs(x="Policyholder's Gender", y="Premium Amount in $")+
  theme_minimal()

# distribution of age
ggplot(data=data, aes(x=age))+
  geom_histogram(color="black", binwidth = 3)+
  ggtitle("Frequency Distribution of Policyholder's Age")+
  labs(x="Policyholder's Age", y="Count")+
  theme_minimal()

# barplot for gender
ggplot(data=data, aes(x=factor(gender)))+
  geom_bar(stat="count", width=0.5, fill="firebrick3")+
  ggtitle("Frequency Distribution of Policyholder's Gender")+
  labs(x="Policyholder's Gender", y="Count")+
  theme_minimal()

# barplot for smoker/non-smoker
ggplot(data=data, aes(x=factor(smoker)))+
  geom_bar(stat="count", width=0.5, fill="steelblue")+
  ggtitle("Frequency Distribution of Policyholder's Smoking Behavior")+
  labs(x="Policyholder's Smoking Behavior", y="Count")+
  theme_minimal()

# correlation matrix to analyze collinearity
ggcorr(data, label = TRUE, label_round = 2)+
  ggtitle("Correlation Matrix - Continuous Attributes")

# pairwise comparison of all attributes
ggpairs(data)

# factor categorical variables - gender & smoker
data$newgender <- factor(data$gender, 
                            levels = c("male", "female"), 
                            labels = c("male", "female")) 
data$newsmoker <- factor(data$smoker, 
                            levels = c("yes", "no"), 
                            labels = c("yes", "no"))

# fit multiple linear regression model
mlr <- lm(data$premium ~data$age+data$newgender+data$bmi+data$children+data$newsmoker)

summary(mlr)

# residual plot - fitted vs residuals
ggplot(mlr, aes(x=fitted(mlr),y=resid(mlr)))+
  geom_point(size=1)+
  geom_hline(yintercept = 0, color="Red")+
  ggtitle("Residual Plot - Fitted vs Residuals - MLR")+
  labs(x="Fitted Values", y="Residuals")+
  theme_minimal()

# histogram of the residuals
ggplot(data=mlr, aes(x=resid(mlr)))+
  geom_histogram(color="black", bins=20)+
  ggtitle("Histogram of Residuals - MLR")+
  labs(x="Residuals", y="Count")+
  theme_minimal()

# two-sample means - smokers vs non-smokers

smk <- subset(data, data$smoker=="yes")
nonsmk <- subset(data, data$smoker=="no")

qt(p=0.975, df=230.38)

t.test(x=smk$premium, y=nonsmk$premium, alternative = "two.sided")

############################################################################
# code for analyzing which variables is causing the clusters 
# drop one of the variables already in the model

newdata <- data
newdata <- subset(newdata, select = -c(bmi))
newdata <- subset(newdata, select = -c(children))
newdata <- subset(newdata, select = -c(age))
newdata <- subset(newdata, select = -c(gender, newgender))
newdata <- subset(newdata, select = -c(smoker, newsmoker))
# mlr model
newmlr <- lm(newdata$premium ~newdata$age)
summary(newmlr)
# residual plot
ggplot(newmlr, aes(x=fitted(newmlr),y=resid(newmlr)))+
  geom_point(size=1)+
  geom_hline(yintercept = 0, color="Red")+
  ggtitle("Residual Plot - Fitted vs Residuals - MLR")+
  labs(x="Fitted Values", y="Residuals")+
  theme_minimal()

############################################################################

