# Description:
A project using Hive for Intrusion Detection with a 2 feature decision tree algorithm based on network traffic features, utilizing the UNSW-NB15 dataset and a Hadoop docker container.

# Abstract:
In this project we are interested in using Hive for Intrusion Detection using a simple 2 feature decision tree algorithm based on network traffic features. The project will be based on a research project that was done by Nour Moustafa and Jill Slay. During this project, I will use the UNSW-NB15 dataset which is free to use for academic purposes, and this is the case with my project. Furthermore, my project will be based on two research papers that were published by the contributors of the main research project. As mentioned before, the goal behind this project is using Hive. So, the Hadoop environment will be a docker container that contains all the necessary tools.

# Description of the data set:
The UNSW-NB15 data set was created using an IXIA PerfectStorm tool in the Cyber Range Lab of the Australian Center for Cyber Security to generate a hybrid of the realistic moden normal activities and the synthetic contemporary attack behaviors from network traffic. A tcpdumb tool was used to capture 100 GB of raw network traffic. However, Tables 1, 2, 3, 4, and 5 provide a clear idea about the features of the data set and its description.

### Table 1. Flow features:
No. | Name | Description 
--- | --- | --- 
1 | Srcip | Source IP address
2 | Sport | Source port number
3 | Dstip | Destination IP address
4 | Dsport | Destination port number
5 | Proto | Protocol type (such as TC, UDP)

### Table 2. Basic features:
No. | Name | Description 
--- | --- | --- 
6 | state | Indicates to the state and its dependent protocol (such as ACC, CLO and CON)
7 | dur | Record total duration
7 | sbytes | Source to destination bytes
9 | dbytes | Destination to source bytes
10 | sttl | Source to destination time to live
11 | dttl | Destination to source time to live
12 | sloss | Source packets retransmitted or dropped
13 | dloss | Destination packets retransmitted or dropped
14 | service | Such as http, ftp, smtp, ssh, dns and ftp-data
15 | sload | Source bits per second
16 | dload | Destination bits per seconds
17 | spkts | Source to destination packet count
18 | dpkts | Destination to source packet count

### Table 3. Content features:
No. | Name | Description 
--- | --- | --- 
19 | swin | Source TCP window advertisement value
20 | dwin | Destination TCP window advertisement value
21 | stcpb | Source TCP base sequence number
22 | stcpb | Destination TCP base sequence number
23 | smeansz | Mean of the flow packet size transmitted by the src
24 | dmeansz | Mean of the flow packet size transmitted by the dst
25 | trans_depth | Represents the pipelined depth into the connection of http request/response transaction
26 | res_bdy_len | Actual uncombressed content size of the data transferred from the server's http service

### Table 4. Time features:
No. | Name | Description 
--- | --- | --- 
27 | sjit | Source jitter (mSec)
28 | djit | Destination jitter (mSec)
29 | stime | Record start time
30 | ltime | Record last time
31 | sintpkt | Source interpacket arrival time (mSec)
32 | dintpkt | Destination interpacket arrival time (mSec)
33 | tcprtt | TCP connection setup round-trip time, the sum of 'synack' and 'ackdat'
34 | synack | TCP connection setup time, the time between the SYN and the SYN_ACK packets
35 | ackdat | TCP connection setup time, the time between the SYN_ACK and the ACK packets

### Table 5. Additional generated features:

In total, there are 47 features with class label. The records were stored in CSV files.
No. | Name | Description 
--- | --- | --- 
36 | is_sm_ips_ports | If *srcip* (1) equals to *dstip* (3) and *sport* (2) equals to *dsport* (4), this variable assigns to 1 otherwise 0
37 | ct_state_ttl | No. for each *state* (6) according to specific range of values of *sttl* (10) and *dttl* (11)
38 | ct_flw_http_mthd | No. of flows that has methods such as Get and Post in http service
39 | is_ftp_cmd | If the ftp session is accessed by user and password then 1 else 0
40 | ct_ftp_cmd | No. of flows that has a command in ftp session
41 | ct_srv_src | No. of records that contain the same *service* (14) and *srcip* (1) in 100 records according to the *ltime* (26)
42 | ct_srv_dst | No. of records that contain the same *service* (14) and *dstip* (1) in 100 records according to the *ltime* (26)
43 | ct_dst_ltm | No. of records of the same *dstip* (3) in 100 records according to the *ltime* (26)
44 | ct_src_ltm | No. of records of the same *srcip* (1) in 100 records according to the *ltime* (26)
45 | ct_src_dport_ltm | No. of records of the same *srcip* (1) and the *dsport* (4) in 100 records according to the *ltime* (26)
46 | ct_dst_sport_ltm | No. of records of the same *dstip* (3) and the *sport* (2) in 100 records according to the *ltime* (26)
47 | ct_dst_src_ltm | No. of records of the same *srcip* (1) and the *dstip* (3) in 100 records according to the *ltime* (26)

# Overview of Hadoop architecture used:
### Docker Container:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Docker%20Container.png)

*Fig 1. Different Component of the Hadoop Architecture used in a Docker Container*

### NameNode Shell:

`hadoop fs -rm input train_set_schema.hql`

This Hadoop command is used to remove the **train_set_schema.hql** file from the **input** directory

`hadoop fs -ls input`

This Hadoop command is used to list the files and directories present in the **input** directory

`hadoop fs -put UNSW_NB15_training-set2.csv input/`

This Hadoop command is used to upload the **UNSW_NB15_training-set2.csv** file to the **input** directory

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Interaction%20with%20Namenode.png)

*Fig 2. Interaction with Namenode Using a Power Shell Window*

### Hive Shell:

`load data inpath 'input/UNSW_NB15_training-set2.csv' into table trainset;`

This HiveQL command is used to load the data from the **UNSW_NB15_training-set2.csv** file present in the **input** directory into the **trainset** table

`select count(*) from trainset;`

This HiveQL command is used to **count** the number of rows present in the **trainset** table

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Interaction%20with%20Hive.png)

*Fig 3. Interaction with Hive Using Power Shell Window*


## Load data into HDFS:

### Copy the files from local machine to namenode:

`ls`

This command is used to **list** the contents of the current directory in a Unix/Linux based system

`docker ps`

This command is used to **list** all the running **Docker containers** on the system.

`docker cp ./UNSW_NB15_training-set2.csv /2f27328ed1be:/`

This command is used to copy the **UNSW_NB15_training-set2.csv** file from the local system to the **root** directory of the Docker container with ID **2f27328ed1be**

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Copying%20the%20Files.png)

*Fig 4. Copying the Files to the Namenode Container*

### Load the files into HDFS:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Loading%20Files.png)

*Fig 5. Loading the Files into HDFS*


# Build the schema for ingesting, including the partitioning:

### The Schema:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Schema.png)

*Fig 6. The Schema of the Training Set*

### Create the schema in Hive:

`create external table trainset (id int, dur float, proto String, Service String, .... )`

This HiveQL command is used to create an external table named trainset with columns id (integer), dur (float), proto (string), Service (string), and other column names and data types specified in the command.

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Schema%20Hive.png)

*Fig 7. Building the Shema in Hive, Train Set example*

### Example of Partitioning by attack_cat:

`partition(attack_cat='Normal');`

This HiveQL command is used to add a **partition** named "**attack_cat**" to the "**trainset**" table, where the attack_cat value is set to "Normal"

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Partition.png)

*Fig 8: Example of Creating a Partition*


## HQL queries to confirm the various statistics of the dataset:

### Test Data Set:

`select count(attack_cat) as count from testset where attack_cat = 'Normal';`

This HiveQL command is used to **count** the number of **rows** in the testset table where the value of the attack_cat column is 'Normal' and display the count as 'count'

`select count(*) from testset;`

This HiveQL command is used to **count** the number of **rows** in the testset table

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blob/master/Assets/Test1.png)

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blob/master/Assets/Test2.png)

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blob/master/Assets/Test3.png)

### Train Data Set:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blob/master/Assets/Train1.png?raw=true)

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blob/master/Assets/Train2.png)

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blob/master/Assets/Train3.png)

The above queries can be summarized in the following table:

Category | Training Set | Testing Set
--- | --- | --- 
Normal | 56,000 | 37,000
Analysis | 2,000 | 677
Backdoor | 1,746 | 583
DoS | 12,264 | 4,089
Exploits | 33,393 | 11,132
Generic | 40,000 | 18,871
Reconnaissance | 10,491 | 3,496
Shellcode | 1,133 | 378
Worms | 130 | 378
Total Records | 175,341 | 378

## HQL queries for computing the Gini impurity for Root Node:
In this part, I will calculate the impurity for 5 features because they are the most likely to split our training set. However, the features are:

* is_sm_ips_sport
* is_ftp_login
* ct_ftp_cmd
* Service
* ct_state_ttl
* proto

### Computing the Gini impurity for is_sm_ips_ports:

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ls_sm_ips_port.png)

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ls_sm_ips_port1.png)

### Compute Gini impurity:
Left node = 1 – (0/(0+2762))^2 – (2762/(0+2762))^2 = 0

Right node = 1 - (119,341/(119,341+53,238))^2 – (53,238/(119,341+53,238))^2 = 0.43

Total impurity = (2762/(2762+172579))*0 + (172579/(2762+172579))*0.43 = 0.42

### Computing the Gini impurity for is_ftp_login:

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ls_ftp_login.png)

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ls_ftp_login1.png)

### Compute Gini impurity:
Left node = 1 – (1618/(1618+949))^2 – (949/(1618+949))^2 = 0.46

Right node = 1 - (111,723/(111,723+55,081))^2 - (55,081/(111,723+55,081))^2 = 0.44

Total impurity = (1618+949)/(1618+949+111723+55081)*0.46 + (111723+55081)/(1618+949+111723+55081)*0.44 = 0.435

### Computing the Gini impurity for ct_ftp_cmd:

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ct_ftp_cmd.png)

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ct_ftp_cmd1.png)

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ct_ftp_cmd2.png)

![alt text](https://raw.githubusercontent.com/AchrafAjrhourh/Hive-Detection-Intrusion/master/Assets/ct_ftp_cmd3.png)

Value of the class | Yes | No
---|---|---
ct_ftp_cmd = 0 | 117,723 | 55,051
ct_ftp_cmd = 1 | 1,598 | 947
ct_ftp_cmd = 2 | 4 | 2
ct_ftp_cmd = 4 | 16 | 0
ct_ftp_cmd = 0 or ct_ftp_cmd = 1 | 119,321 | 55,998
ct_ftp_cmd = 0 or ct_ftp_cmd = 2 | 117,727 | 55,053
ct_ftp_cmd = 0 or ct_ftp_cmd = 4 | 117,739 | 55,051
ct_ftp_cmd = 1 or ct_ftp_cmd = 2 | 1,602 | 949
ct_ftp_cmd = 1 or ct_ftp_cmd = 4 | 1,614 | 947
ct_ftp_cmd = 4 or ct_ftp_cmd = 2 | 20 | 2
ct_ftp_cmd = 0 or ct_ftp_cmd = 1 or ct_ftp_cmd = 2 | 119,325 | 56,000
ct_ftp_cmd = 0 or ct_ftp_cmd = 1 or ct_ftp_cmd = 2 | 119,337 | 55,998
ct_ftp_cmd = 1 or ct_ftp_cmd = 2 or ct_ftp_cmd = 4 | 1,618 | 949

The total Gini impurity for ct_ftp_cmd = 0.43

### Computing the Gini impurity for the rest columns:

To make the process much easier, I will be implementing a pyth
on workflow to compute the Gini impurity for each column. the implementation is based on couple of functions as shown below:

```
attribute_names = ['is_sm_ips_ports', 'service', 'ct_state_ttl', 'is_ftp_login', ct_ftp_cmd', proto']
#STEP 1: Calculate gini(D)
def gini_impurity(value_counts):
  n = value_counts.sum()
  p_sum = 0
  for key in value_count.keys():
      p_sum = p_sum + (value_counts[key] / n) * (value_counts[key] / n)
  gini = 1 - p_sum
  return gini
```
  
  
```
def gini_split_a(attribute_name):
    attribute_values = df[attribute_name].value_counts()
    gini_A = 0
    for key in attribute_values.keys():
        df_k = df['label'][df[attribute_name] == key].value_counts()
        n_k = attribute_values[key]
        n = df.shape[0]
        gini_A = gini_A + ((n_k / n)*gini_impurity(df_k))
    return gini_A
```
  
### The result of Gini impurity:


* Total Gini for is_sm_ips_ports is **0.42**
* Total Gini for service is **0.403**
* Total Gini for ct_state_ttl is **0.158**
* Total Gini for is_ftp_login is **0.435**
* Total Gini for ct_ftp_cmd is **0.435**
* Total Gini for proto is **0.352**

As we can see from the above result the **Root Node is ct_state_ttl** because it has the best Gini impurity.

## Computing Gini Impurity for the Next Best Feature to Choose:

I used the same process as I did in the previous question, I write HQL queries, and I automate them using python. However, this is the results after computing Gini Imuprity for each split of the Root Node:


### The result of Gini impurity for Root Node ct_state_ttl = 0:

* Total Gini for is_sm_ips_ports is **0.042**
* Total Gini for service is **0.041**
* Total Gini for is_ftp_login is **0.042**
* Total Gini for ct_ftp_cmd is **0.042**
* Total Gini for proto is **0.017**

As we can see from the above result the **First Node is proto** because it has the best Gini impurity.

### The result of Gini impurity for Root Node ct_state_ttl = 1:

* Total Gini for is_sm_ips_ports is **0.291**
* Total Gini for service is **0.270**
* Total Gini for is_ftp_login is **0.290**
* Total Gini for ct_ftp_cmd is **0.290**
* Total Gini for proto is **0.291**

As we can see from the above result the **Second Node is service** because it has the best Gini impurity.

### The result of Gini impurity for Root Node ct_state_ttl = 2:

* Total Gini for is_sm_ips_ports is **0.065**
* Total Gini for service is **0.117**
* Total Gini for is_ftp_login is **0.125**
* Total Gini for ct_ftp_cmd is **0.125**
* Total Gini for proto is **0.063**

As we can see from the above result the **Third Node is proto** because it has the best Gini impurity.

### The result of Gini impurity for Root Node ct_state_ttl = 3:

* Total Gini for is_sm_ips_ports is **0.267**
* Total Gini for service is **0.05**
* Total Gini for is_ftp_login is **0.257**
* Total Gini for ct_ftp_cmd is **0.257**
* Total Gini for proto is **0.137**

As we can see from the above result the **Fourth Node is service** because it has the best Gini impurity.

### The result of Gini impurity for Root Node ct_state_ttl = 6:

* Total Gini for is_sm_ips_ports is **0.494**
* Total Gini for service is **0.441**
* Total Gini for is_ftp_login is **0.495**
* Total Gini for ct_ftp_cmd is **0.495**
* Total Gini for proto is **0.009**

As we can see from the above result the **Fifth Node is proto** because it has the best Gini impurity.

### HQL Queries to Build the Leaf Nodes:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20to%20Build%20the%20Leaf%20Nodes.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20to%20Build%20the%20Leaf%20Nodes1.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20to%20Build%20the%20Leaf%20Nodes2.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20to%20Build%20the%20Leaf%20Nodes3.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20to%20Build%20the%20Leaf%20Nodes4.png)

### The Decision Tree Schema:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Decision%20Tree%20Schema.png)

### Example of How this Decision Tree works:

For a certain record in the dataset, we will see first the value of the ct_state_ttl because it is the Root Node. Let's say, for example, ct_state_ttl = 0, then the second split will be in the level of the proto column. If the proto has as a value ‘sctp’, the prediction of the label will be 1. So, this is a simple example of how that decision tree works.

## Predictions and Confusion Matrix:

### Predictions that are produced by the Decision Tree Model for Training:

### Positive Predictions for Training Set:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Positive%20Predictions%20for%20Training%20Set.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Positive%20Predictions%20for%20Training%20Set1.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Positive%20Predictions%20for%20Training%20Set2.png)

* Positive Predictions: 129,560

### Negative Predictions for Training Set:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Negative%20Predictions%20for%20Training%20Set.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Negative%20Predictions%20for%20Training%20Set1.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Negative%20Predictions%20for%20Training%20Set2.png)

* Negative Predictions: 45,796

### Confusion Matrix for Training Set:

### *HQL Queries to compute the True Positive and False Positive*:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20TP%20FP.png)
![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20TP%20FP1.png)

* True Positive: 118,076
* False Positive: 11,484

### *HQL Queries to compute the True Negative and False Negative*:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20TN%20FN.png)

* True Negative: 44,576
* False Negative: 1,220

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Overall%20Table.png)

* Training Accuracy: (TP + TN) / (TP+FN+TN+FP) = 0.92

### Predictions for Testing Using the Decision Tree Model:

### Positive Predictions for Testing Set:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Positive%20Predictions%20for%20Testing%20Set.png)

* Positive Predictions: 60,507

### Negative Predictions for Testing Set:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Negative%20Predictions%20for%20Testing%20Set.png)

* Negative Predictions: 21,780

### Confusion Matrix for Testing Set:

### *HQL Queries for True Positive and False Positive*:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20TF%20FP%20T.png)

* True Positive: 44,954
* False Positive: 15,533

### *HQL Queries for True Negative and False Negative*:

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/HQL%20Queries%20TN%20FN%20T.png)

* True Negative: 21,447
* False Negative: 333

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/raw/master/Assets/Overall%20Table%20T.png)

* Testing Accuracy: (TP+TN)/(TP+TN+FN+FP) = 0.80
