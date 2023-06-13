# Experiment Description

Currently, the Udacity course home pages offer two options: "start free trial" and "access course materials." By selecting "start free trial," users are prompted to enter credit card information and are then enrolled in a 14-day trial period, followed by automatic charges. Alternatively, choosing "access course materials" allows users to view the course content but excludes coaching support, verified certificates, and project feedback.

The rationale behind this change is to allocate study time for students, potentially enhancing the overall student experience and enabling coaches to better support students who are likely to complete the course. Also, this change aims to avoid a significant reduction in the number of students continuing beyond the free trial period.

# Experiment Design

The initial unit of diversion to the conrol and experiment groups is a unique cookie. However, once a student enrolls in the free trial, they are tracked by user-id. The same user-id can't enroll more than once. Users who don't enroll are not tracked by user-id. The uniqueness of a cookie is determined per day.

# Metric Choice

## Invariant Metrics: number of cookies, number of clicks, click-through-probability

## Evaluation Metrics: gross conversion, retention, net conversion

