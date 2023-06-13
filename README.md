# 1. Experiment Description

Currently, the Udacity course home pages offer two options: "start free trial" and "access course materials." By selecting "start free trial," users are prompted to enter credit card information and are then enrolled in a 14-day trial period, followed by automatic charges. Alternatively, choosing "access course materials" allows users to view the course content but excludes coaching support, verified certificates, and project feedback.

The rationale behind this change is to allocate study time for students, potentially enhancing the overall student experience and enabling coaches to better support students who are likely to complete the course. The goal of the experiment is reducing the number of students who would quit the course during the starting 14days period, while leaving the number of studnets who pass this 14days period intact. I would like to call the first group of students, “frustrated”, and the second group, “resolute” students.


# 2. Experiment Design

The initial unit of diversion to the conrol and experiment groups is a unique cookie. However, once a student enrolls in the free trial, they are tracked by user-id. The same user-id can't enroll more than once. Users who don't enroll are not tracked by user-id. The uniqueness of a cookie is determined per day.

# 3. Metric Choice

We need two groups of metrics. One group for measuring the effect of change that is imposed, and one group for sanity check, i.e. validation of the test.


## 4. Invariant Metrics:
**(1). Number of cookies:**  The number of unique cookies to visit the course overview page. This is the unit of diversion and even distribution amongst the control and experiment groups is expected. It is therefore appropriate as an invariant metric.

**(2). Number of clicks:** The number of users (tracked as unique cookies at this stage) to click the free trial buttion. This is appropriate as an invariant metric but not an evaluation metrice. Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.

**(3). Click-through-probability:** Unique cookies to click the "start free trial" button per unique cookies to view the course overview page. Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.


## 5. Evaluation Metrics: 
 
Evaluation metrics were chosen since there is the possibility of different distributions between experiment and control groups as a function of the experiment. Each evaluation metric is associated with a minimum difference (dmin) that must be observed for consideration in the decision to launch the experiment. The ultimate goal is to minimize student frustration and satisfaction and to most effectively use limited coaching resources. Cancelling early may be one indication of frustration or low satisfaction and the more students enrolled in the course who do not make at least one payment, much less finish the course, the less coaching resources are being used effectively. With this in mind, in order to consider launching the experiment either of the following must be observed:


 
**(1). Gross conversion:**   That is, number of user-ids to complete checkout and enroll in the free trial divided by number of unique cookies to click the “Start free trial” button. (dmin= 0.01) - It can be an evaluation metric. The value should be reduced in the experiment group since the number of enrollments is expected to decrease.

**(2). Retention:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by number of user-ids to complete checkout. (dmin=0.01) - This can be an evaluation metric. Since we expect that the number of enrollment reduces, and the number of payments remains more or less the same, this metric is expected to be more in the experiment group comparing to the control group.

**(3). Net conversion:** That is, number of user-ids to remain enrolled past the 14-day boundary (and thus make at least one payment) divided by the number of unique cookies to click the “Start free trial” button. (dmin= 0.0075) - This can be an evaluation metric. The number of clicks is unaffected by the change, the number of payments is expected to remain more or less the same, so the change of this metric is expected to be insignificant.


# 6. Measure of Variability

It is asked in the template to calculate the “standard deviation of the sampling distribution” or standard error(SE). The standard error of the current metrics depends on the sample size, and can be computed analytically. 

Since the daily unique cookies of the overview page is 5000, we need to figure out sample size for each metric. The sample size is equal to the denominator quantity, assuming 5000 unique cookies for the overview page.

The formula which is used here for estimation of the standard error is based on CLT, and the fact that sampling distribution of the metrics has normal distribution.



```python
p_enroll_click <- 0.20625
n <- 400
SE_gross_conversion <- sqrt(p_enroll_click*(1-p_enroll_click)/n)
SE_gross_conversion
```

```
## [1] 0.0202306
```


```python
p_payment_enroll <- 0.53
n <- 82.5 
SE_retention <- sqrt(p_payment_enroll*(1-p_payment_enroll)/n)
SE_retention
```

```
## [1] 0.05494901
```

```python
p_payment_click <- 0.1093125
n <- 400
SE_net_conversion <- sqrt(p_payment_click*(1-p_payment_click)/n)
SE_net_conversion
```

```
## [1] 0.01560154
```


# 7. Sizing

  
| Unique cookies to view page per day:	              | 40000   |
|  :---                                              | ---     |
| Unique cookies to click "Start free trial" per day:| 3200    |
| Enrollments per day:	                              | 660     |
| Click-through-probability on "Start free trial": 	 | 0.08    |
| Probability of enrolling, given click:	            | 0.20625 |

# 8. Sanity Check

The first step in validation of the experiment is assessment of invariant metrics. There are some metrics that are expected to have more or less identical values in the both experiment and control groups.

From the metrics that we have, I have chosen the number of cookies, number of clicks, and click-through probability as invariant metrics. Now using  [this data](https://www.google.com), I check whether the values of these metrics are significantly different in the experiment and control groups.


```python
control <- read.csv("/Users/yuxishen/Downloads/Final\ Project\ Results\ -\ Control.csv")

experiment <- read.csv("/Users/yuxishen/Downloads/Final\ Project\ Results\ -\ Experiment.csv")

```

For the counts metrics, we assumed that 50% of the experiment traffic goes to the experiment group and 50% goes to the control group. If we call these two groups success and failure, then the model can be a Bernoulli distribution. So I would check whether the current counts of these two groups can come from a population with 0.5 change of success or failure. And I do it using bootstrapping to estimate and build confidence interval.

**Null Hypothesis:** Status quo. Any difference between the metric value of the two groups is due to chance. 
**Alternate Hypothesis:** The difference between the metric value of the two groups is meaningful, and significant. It cannot be due to random change.

It is said in the course that we should calculate the fraction of the control group on the total. It can be the difference of the numbers of the two groups, or relative size of each group to another one. There are different ways anyway.

**The calculation:**

```python
alpha <- 0.05

experiment_pageviews <- experiment$Pageviews
total_exp_pview <- sum(experiment_pageviews)
total_exp_pview
```
```
## [1] 344660
```

```python
control_pageviews <- control$Pageviews
total_cntl_pview <- sum(control_pageviews)
total_cntl_pview
```

```
## [1] 345543
```

```python
observed_ratio<- total_cntl_pview/
        (total_exp_pview+total_cntl_pview)
observed_ratio
```

```
## [1] 0.5006397
```



```python

pool <- c(rep(x= 1,total_cntl_pview),rep(0,total_exp_pview))
ratio_vector <- vector(length=10000)

#sum(pool)/length(pool)

# Calculation Using for-loop
#----------------------------
# for (i in 1:10000){
#         pool_resample <- sample(pool,
#                                 size = length(pool),
#                                 replace = TRUE)
#         
#         ratio_vector[i] <- sum(pool_resample)/length(pool)
# }
# 
# hist(ratio_vector)
# abline(v = observed_ratio)
# --------------------------
# Calculation Using lapply()
#----------------------------
# t<- lapply(1:10000 , function(i){
#         pool_resample <- sample(pool,
#                                 size = length(pool),
#                                 replace = TRUE)
#         
#         ratio_vector[i] <- sum(pool_resample)/length(pool)
#         #ratio_vector[i]
# })
# 
# t <- unlist(t)
# hist(t)
# -----------------------------
#Calculation using parallel computing 
#----------------------------

# Calculate the number of cores
no_cores <- detectCores() - 1

# Initiate cluster
cl <- makeCluster(no_cores)



t_parallel<- mclapply(X = 1:10000 , FUN = function(i){
        pool_resample <- sample(pool,
                                size = length(pool),
                                replace = TRUE)
        
        ratio_vector[i] <- sum(pool_resample)/length(pool)
        #ratio_vector[i]
} )

t_parallel <- unlist(t_parallel)
t_parallel <- sort(t_parallel,decreasing = FALSE)
lower_bound <- t_parallel[10000*alpha/2]
upper_bound <- t_parallel[10000 - (10000*alpha/2)]

hist(t_parallel, main = "Proportion of #cookies" )
abline(v =0.5 , col = "blue" )
abline(v = lower_bound, col = "red")
abline(v = upper_bound, col = "red")
```
![Alt text](/Users/syx/Desktop/pic_1.png "Optional title")




