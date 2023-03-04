# Description:
A project using Hive for Intrusion Detection with a 2 feature decision tree algorithm based on network traffic features, utilizing the UNSW-NB15 dataset and a Hadoop docker container

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

![alt text](https://github.com/AchrafAjrhourh/Hive-Detection-Intrusion/blobmaster/Assets/Train1.png)

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


