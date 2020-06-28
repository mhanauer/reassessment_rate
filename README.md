

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
InterviewType_07
1 = Baseline
3 = 6-month follow-up
```{r}
library(lubridate)
setwd("T:/CRI_Research/telehealth_evaluation/data_codebooks/Reassessment_data")
fhhc_reasses_data = read.csv("reasses_test_fhhc.csv", header = TRUE)
fhhc_reasses_dat = fhhc_reasses_data
#Only keep baseline and 6-month
fhhc_reasses_dat = subset(fhhc_reasses_dat, InterviewType_07 == 1 | InterviewType_07 ==3)
# Get data just for this quarter so get due date
fhhc_reasses_dat = fhhc_reasses_dat[c("ConsumerID", "InterviewType_07", "InterviewDate")]
fhhc_reasses_dat$InterviewDate = mdy(fhhc_reasses_dat$InterviewDate)
fhhc_reasses_dat
fhhc_reasses_dat$reasses_due = fhhc_reasses_dat$InterviewDate + months(6)
fhhc_reasses_dat$reasses_due
#Create an indicator for quarter one for baseline and only keep those people
fhhc_reasses_dat$q1_due = ifelse(fhhc_reasses_dat$reasses_due >= "2019-10-01" & fhhc_reasses_dat$reasses_due <= "2019-12-31" & fhhc_reasses_dat$InterviewType_07 ==1, 1, 0)
q1_due_dat = subset(fhhc_reasses_dat, q1_due == 1)
# Then grab everyone with a 6-month reassessment to see if we can match them
month6_q1 = subset(fhhc_reasses_dat, InterviewType_07 == 3)
## match those with baseline reassessment with those with three month reassesments 
q1_complete = merge(q1_due_dat, month6_q1, by = "ConsumerID")
q1_complete

results_q1 = data.frame(q1_due = dim(q1_due_dat)[1], q1_complete = dim(q1_complete)[1])
results_q1
```
CSAT
InterviewTypeCode 
1 = Intake
2 = 6 month follow up
3 = 12 month follow up 
4 = 3 month follow up
5 = Discharge

FFY Quarter consists of four quarters as follows:
. Quarter 1: October 1 - December 31
. Quarter 2: January 1 - March 31
. Quarter 3: April 1 - June 30
. Quarter 4: July 1 - September 30
```{r}
setwd("T:/CRI_Research/telehealth_evaluation/data_codebooks/Reassessment_data")
emerge_reasses_data = read.csv("CSAT_DataDownload_06_25_2020.csv", header = TRUE)
emerge_reasses_dat = emerge_reasses_data
#Only keep baseline and 6-month
emerge_reasses_dat
emerge_reasses_dat = subset(emerge_reasses_dat, InterviewType == 1 | InterviewType ==2)
# Get data just for this quarter so get due date
emerge_reasses_dat = emerge_reasses_dat[c("ClientID", "InterviewType", "InterviewDate")]
emerge_reasses_dat$InterviewDate = mdy(emerge_reasses_dat$InterviewDate)
emerge_reasses_dat
### Remove missing intake dates
emerge_reasses_dat = na.omit(emerge_reasses_dat)
emerge_reasses_dat$reasses_due = emerge_reasses_dat$InterviewDate + months(6)
emerge_reasses_dat$reasses_due
### Having trouble with 2018-05-31, because 2018-11-30 only have 30 days
emerge_reasses_dat[is.na(emerge_reasses_dat)] = ymd("2018-12-01")
#Create an indicator for quarter one for baseline and only keep those people
emerge_reasses_dat$q1_due = ifelse(emerge_reasses_dat$reasses_due >= "2019-10-01" & emerge_reasses_dat$reasses_due <= "2019-12-31" & emerge_reasses_dat$InterviewType ==1, 1, 0)
q1_due_dat = subset(emerge_reasses_dat, q1_due == 1)
# Then grab everyone with a 6-month reassessment to see if we can match them
month6_q1 = subset(emerge_reasses_dat, InterviewType == 2)
## match those with baseline reassessment with those with three month reassesments 
q1_complete = merge(q1_due_dat, month6_q1, by = "ClientID")
q1_complete

results_q1 = data.frame(q1_due = dim(q1_due_dat)[1], q1_complete = dim(q1_complete)[1])
results_q1
```
Q3 CSAT
```{r}
setwd("T:/CRI_Research/telehealth_evaluation/data_codebooks/Reassessment_data")
emerge_reasses_data = read.csv("CSAT_DataDownload_06_25_2020.csv", header = TRUE)
emerge_reasses_dat = emerge_reasses_data
#Only keep baseline and 6-month
emerge_reasses_dat
emerge_reasses_dat = subset(emerge_reasses_dat, InterviewType == 1 | InterviewType ==2)
# Get data just for this quarter so get due date
emerge_reasses_dat = emerge_reasses_dat[c("ClientID", "InterviewType", "InterviewDate")]
emerge_reasses_dat$InterviewDate = mdy(emerge_reasses_dat$InterviewDate)
emerge_reasses_dat
### Remove missing intake dates
emerge_reasses_dat = na.omit(emerge_reasses_dat)
emerge_reasses_dat$reasses_due = emerge_reasses_dat$InterviewDate + months(6)
emerge_reasses_dat$reasses_due
### Having trouble with 2018-05-31, because 2018-11-30 only have 30 days
emerge_reasses_dat[is.na(emerge_reasses_dat)] = ymd("2018-12-01")
sum(is.na(emerge_reasses_dat))
#Create an indicator for quarter one for baseline and only keep those people
emerge_reasses_dat$q3_due = ifelse(emerge_reasses_dat$reasses_due >= "2020-04-01" & emerge_reasses_dat$reasses_due <= "2020-06-30" & emerge_reasses_dat$InterviewType ==1, 1, 0)
subset(emerge_reasses_dat, q3_due == 1)
q3_due_dat = subset(emerge_reasses_dat, q3_due == 1)
# Then grab everyone with a 6-month reassessment to see if we can match them
month6_q3 = subset(emerge_reasses_dat, InterviewType == 2)
## match those with baseline reassessment with those with three month reassesments 
q3_complete = merge(q3_due_dat, month6_q3, by = "ClientID")
q3_complete

results_q3 = data.frame(q3_due = dim(q3_due_dat)[1], q3_complete = dim(q3_complete)[1])
results_q3

```



