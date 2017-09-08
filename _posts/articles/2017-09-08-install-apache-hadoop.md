---
layout: article
title: "Install Apache Hadoop: Setting up a Single Node Hadoop Cluster"
date: 2017-09-08
modified:
categories: articles
excerpt:
tags: [Data Science,hadoop,Big Data]
ads: true
image:
  feature: 2017-09-08/1.png
  teaser: 2017-09-08/0.png
--- 

The Apache Hadoop software library is a framework that allows for the distributed processing of large data sets across 
clusters of computers using simple programming models. It is designed to scale up from single servers to thousands of 
machines, each offering local computation and storage. Rather than rely on hardware to deliver high-availability, the 
library itself is designed to detect and handle failures at the application layer, so delivering a highly-available 
service on top of a cluster of computers, each of which may be prone to failures.

The project includes these modules:


- Hadoop Common: The common utilities that support the other Hadoop modules.
- Hadoop Distributed File System (HDFS™): A distributed file system that provides high-throughput access to application 
data.
- Hadoop YARN: A framework for job scheduling and cluster resource management.
- Hadoop MapReduce: A YARN-based system for parallel processing of large data sets.


#### Installation

Basically, There are two ways to install Apache Hadoop, as a Single node and Multi node.

##### Single Node

This method is widely used for studying or testing purposes also In here only one DataNode running and setting up all the 
NameNode, DataNode, ResourceManager and NodeManager on a single machine. single node cluster can be used to test 
sequential workflow(collecting, aggregating, storing and processing the data) in a smaller environment as compared 
to large environments which contain terabytes of data distributed across hundreds of machines.


##### Multi Node
While in a Multi node cluster, there are more than one DataNode running and each DataNode is running on different machines. 
The multi node cluster is practically used in organizations for analyzing Big Data.

So in this document, i'll explain how to install Apache Hadoop on a single node cluster before move on to the installation we need to make sure that we are good with the following


- JAVA: You need to install the Java 8 package on your system. (Download)
- HADOOP: You require Hadoop 2.7.4package. (Download)


#### Installing HADOOP

**STEP 01:** Extract the Hadoop tar File.

```ruby
    tar -xvf hadoop-2.7.3.tar.gz
```

**STEP 02:** Add the Hadoop and Java paths in the bash_profile file (.bash_profile) as shown below.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/2.png"></a>
	<figcaption></figcaption>
</figure>
Then, save the bash file and close it.

NOTE:


- For applying all these changes to the current Terminal, execute the source command.
```ruby
    source ~/.bash_profile
```
- To make sure that Java and Hadoop have been properly installed on your system and can be accessed 
through the Terminal, execute the java -version and Hadoop version commands.

```ruby
    java -version
    hadoop version
```
if you are getting the similar message as following for the above commands you are good to proceed with the further steps.
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/3.png"></a>
	<figcaption></figcaption>
</figure>

**STEP 03:** Edit the Hadoop Configuration files.

All the Hadoop configuration files are located in ```ruby /Users/mimran/Data-Science/hadoop-2.7.4/etc/hadoop```
directory as you can see in the snapshot below:
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/4.png"></a>
	<figcaption></figcaption>
</figure>

**STEP 04:** Open *core-site.xml* and add property tags as shown below.

*core-site.xml* informs Hadoop daemon where NameNode runs in the cluster. It contains configuration settings of Hadoop core such as I/O settings that are common to HDFS & MapReduce.

```ruby
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
    <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:8000</value>
    </property>
    </configuration>
```

**STEP 05:** Open *hdfs-site.xml* and add the property tags as mentioned below.

*hdfs-site.xml* contains configuration settings of HDFS daemons (i.e. NameNode, DataNode, Secondary NameNode). It also includes the replication factor and block size of HDFS.

```ruby
    <?xml version="1.0" encoding="UTF-8"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
    <property>
    <name>dfs.replication</name>
    <value>1</value>
    </property>
    <property>
    <name>dfs.permission</name>
    <value>false</value>
    </property>
    <property>
    <name>dfs.namenode.name.dir</name>
    <value>/Users/mimran/Data-Science/hadoop-2.7.4/hadoop_data/hdfs/namenode</value>
    </property>
    <property>
    <name>dfs.datanode.data.dir</name>
    <value>/Users/mimran/Data-Science/hadoop-2.7.4/hadoop_data/hdfs/datanode</value>
    </property>
    </configuration>
```

**STEP 06:** Open the *mapred-site.xml* file and add the property tags as shown below:

*mapred-site.xml* contains configuration settings of MapReduce application like number of JVM that can run in parallel, the size of the mapper and the reducer process,  CPU cores available for a process, etc.

In some cases, *mapred-site.xml* file is not available. So, we have to create the *mapred-site.xml* file using *mapred-site.xml.template* file.

```ruby
    <?xml version="1.0"?>
    <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
    <configuration>
    <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
    </property>
    </configuration>
```

**STEP 07:** Open *yarn-site.xml* and add the property tags as mentioned below.

*yarn-site.xml* contains configuration settings of ResourceManager and NodeManager like application memory management size, the operation needed on program & algorithm.

```ruby
    <?xml version="1.0"?>
    <configuration>
    <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
    </property>
    <property>
    <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>
    <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
    <property>
        <name>yarn.nodemanager.pmem-check-enabled</name>
        <value>false</value>
      </property>
      <property>
        <name>yarn.nodemanager.vmem-check-enabled</name>
        <value>false</value>
      </property>
    </configuration>
```

**STEP 08:**  Open *hadoop-env.sh* and add the JAVA PATH as mentioned below.
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/5.png"></a>
	<figcaption></figcaption>
</figure>

**STEP 09:** Go to Hadoop home directory and format the NameNode.
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/6.png"></a>
	<figcaption></figcaption>
</figure>

NOTE:
This formats the HDFS via NameNode. This command is only executed for the first time. Formatting the file system means initializing the directory specified by the dfs.name.dir variable.
Never format, up and running Hadoop filesystem. You will lose all your data stored in the HDFS.

**STEP 10:** Once the NameNode is formatted, go to ```ruby hadoop-2.7.4/sbin``` directory and start all the daemons by typing the 
following.

```ruby
cd /Users/mimran/Data-Science/hadoop-2.7.4/sbin
./start-dfs.sh
```
This will start all the deamons as follow or you can start each and every deamn seperately as well


- **NameNode**

  The NameNode is the centerpiece of an HDFS file system. It keeps the directory tree of all files stored in the HDFS and tracks all the file stored across the cluster and it can be started via the following command:
  
  ```ruby
  ./hadoop-daemon.sh start namenode
  ```

- **DataNode**

  On startup, a DataNode connects to the Namenode and it responds to the requests from the Namenode for different operations and it can be started via the following command:
  
  ```ruby
  ./hadoop-daemon.sh start datanode
  ```

- **ResourceManager**

  ResourceManager is the master that arbitrates all the available cluster resources and thus helps in managing the distributed applications running on the YARN system. Its work is to manage each NodeManagers and the each application’s ApplicationMaster and it can be started via the following command:
  
  ```ruby
  ./yarn-daemon.sh start resourcemanager
  ```
  
- **NodeManager**

  The NodeManager in each machine framework is the agent which is responsible for managing containers, monitoring their resource usage and reporting the same to the ResourceManager and it can be started via the following command:
  
  ```ruby
  ./yarn-daemon.sh start nodemanager
  ```
  
- **JobHistoryServer**

  JobHistoryServer is responsible for servicing all job history related requests from client and it can be started via the following command:
  
  ```ruby
  ./mr-jobhistory-daemon.sh start historyserver
  ```
After started the all the daemons we can verify all the daemons are up and running by using the following command.
  
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/7.png"></a>
	<figcaption></figcaption>
</figure>

if every thing went ok then you can see the Namenode interface on ```ruby localhost:50070/dfshealth.html``` as shown below

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-08/8.png"></a>
	<figcaption></figcaption>
</figure>