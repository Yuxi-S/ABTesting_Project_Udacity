# Experiment Description

Currently, the Udacity course home pages offer two options: "start free trial" and "access course materials." By selecting "start free trial," users are prompted to enter credit card information and are then enrolled in a 14-day trial period, followed by automatic charges. Alternatively, choosing "access course materials" allows users to view the course content but excludes coaching support, verified certificates, and project feedback.

The rationale behind this change is to allocate study time for students, potentially enhancing the overall student experience and enabling coaches to better support students who are likely to complete the course. Also, this change aims to avoid a significant reduction in the number of students continuing beyond the free trial period.

# Experiment Design

The initial unit of diversion to the conrol and experiment groups is a unique cookie. However, once a student enrolls in the free trial, they are tracked by user-id. The same user-id can't enroll more than once. Users who don't enroll are not tracked by user-id. The uniqueness of a cookie is determined per day.

# Metric Choice
We need two groups of metrics. One group for measuring the effect of change that is imposed, and one group for sanity check, i.e. validation of the test.


## Invariant Metrics:
**(1). Number of cookies.**  The number of unique cookies to visit the course overview page. This is the unit of diversion and even distribution amongst the control and experiment groups is expected. It is therefore appropriate as an invariant metric.

**(2). Number of clicks.** The number of users (tracked as unique cookies at this stage) to click the free trial buttion. This is appropriate as an invariant metric but not an evaluation metrice. Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.

**(3). Click-through-probability.** Unique cookies to click the "start free trial" button per unique cookies to view the course overview page. Equal distribution amongst the experiment and control groups would be expected since at this point in the funnel the experience is the same for all users and therefore elements of the experiment would not be expected to impact clicking the "start free trial" button.


## Evaluation Metrics: 
 
Evaluation metrics were chosen since there is the possibility of different distributions between experiment and control groups as a function of the experiment. Each evaluation metric is associated with a minimum difference (dmin) that must be observed for consideration in the decision to launch the experiment. The ultimate goal is to minimize student frustration and satisfaction and to most effectively use limited coaching resources. Cancelling early may be one indication of frustration or low satisfaction and the more students enrolled in the course who do not make at least one payment, much less finish the course, the less coaching resources are being used effectively. With this in mind, in order to consider launching the experiment either of the following must be observed:


 
**(1). Gross conversion.** 

**(2). Retention.**

**(3). Net conversion.**

