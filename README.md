

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
You have to copy and paste the baseline dates from csv into R and back into csv

InterviewType_07
1 = Baseline
3 = 6-month follow-up
ConductedInterview 
1 = Yes
InterviewDate
```{r}
library(lubridate)
setwd("T:/CRI_Research/telehealth_evaluation/data_codebooks/Reassessment_data")
fhhc_reasses_data = read.csv("reasses_test_fhhc.csv", header = TRUE)
fhhc_reasses_dat = fhhc_reasses_data
fhhc_reasses_dat = subset(fhhc_reasses_dat, InterviewType_07 == 1 | InterviewType_07 ==3)
fhhc_reasses_dat = subset(fhhc_reasses_dat, ConductedInterview == 1)
# Get data just for this quarter so get due date
fhhc_reasses_dat = fhhc_reasses_dat[c("ConsumerID", "InterviewType_07", "InterviewDate")]
fhhc_reasses_dat$InterviewDate = mdy(fhhc_reasses_dat$InterviewDate)
fhhc_reasses_dat
fhhc_reasses_dat$reasses_due = fhhc_reasses_dat$InterviewDate + months(6)
fhhc_reasses_dat$reasses_due

fhhc_reasses_dat$q1_due = ifelse(fhhc_reasses_dat$reasses_due >= "2019-10-01" & fhhc_reasses_dat$reasses_due <= "2019-12-31" & fhhc_reasses_dat$InterviewType_07 ==1, 1, 0)
q1_due_dat = subset(fhhc_reasses_dat, q1_due == 1)
month6_q1 = subset(fhhc_reasses_dat, InterviewType_07 == 3)

q1_complete = merge(q1_due_dat, month6_q1, by = "ConsumerID")
q1_complete

results_q1 = data.frame(q1_due = dim(q1_due_dat)[1], q1_complete = dim(q1_complete)[1])
results_q1
```

