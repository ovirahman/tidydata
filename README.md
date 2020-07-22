
**reading Variable names from "features.txt" file**

```
features<- read.table("./features.txt")

feature_names <- features[[2]]
```

**reading train and test data with variable names for each column**

```
train_data<- read.table("./train/X_train.txt", col.names = feature_names)
test_data<- read.table("./test/X_test.txt", col.names = feature_names)
```

**merging the data sets**

```
merged_data <- merge(train_data, test_data,all = T)
```

**extracting mean and standard deviation for each measurement**

```
meanAndSD<- data.frame(mean = colMeans(merged_data), sd = apply(merged_data, 2, sd))
```


**getting the label ids**

```
train_id <- read.table("./train/y_train.txt", col.names = "id")

test_id <- read.table("./test/y_test.txt", col.names = "id")
```

**combining label ids in the same order as merged_data**

```
label_ids <- rbind(train_id, test_id)
```

**getting activity labes from activity_labels.txt file**

```
label_names <- read.table("./activity_labels.txt", col.names = c("id", "activity"))
```

**creating a data frame of labels with label id and names, in same order as  merged_data for all observations**

```
labels<- merge(label_ids, label_names)
```

**getting the subjects for all observation**

```
subject_test<- read.table("./test/subject_test.txt", col.names = "subject")

subject_train <- read.table("./train/subject_train.txt", col.names = "subject")

subject <- rbind(subject_train, subject_test)
```

**adding activity labels and  subject on the merged data**

```
data <- data.frame(activity = labels$activity,subject = subject, merged_data)
```


**creating the tidy dataset with the average of each variable for each activity and each subject**

```
tidy<-aggregate(data[,3:563], list(activity = data$activity, subject = data$subject), mean)
```

**exporting the tidy dataset as tab separated values**

```
write.table(tidy, file = "tidydata.txt", sep = "\t", row.names = F)
```


