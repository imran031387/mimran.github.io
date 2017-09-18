---
layout: article
title: "Install and configure Apache HIVE with vanilla Hadoop"
date: 2017-09-18
modified:
categories: articles
excerpt:
tags: [Data Science,hadoop,HIVE,Big Data]
ads: true
image:
  feature: 2017-09-18/1.png
  teaser: 2017-09-18/0.png
--- 

The Apache Hive ™ data warehouse software facilitates reading, writing, and managing large datasets residing in distributed storage using SQL.
 Structure can be projected onto data already in storage. A command line tool and JDBC driver are provided to connect users to Hive.

#### Installation
**Pre-requisite**


- Apache Hadoop
- JDBC driver for MySQL (<a href="https://dev.mysql.com/downloads/connector/j/5.1.html">Download</a>)


#### Installing SQOOP

**STEP 01:** Download Apache HIVE

**STEP 02:** Extract the HIVE tar File.

```ruby
    tar -xzvf hive-2.3.0.tar.gz
```

**STEP 03:** Add HIVE paths in the bash_profile file (.bash_profile) as shown below.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-18/2.png"></a>
	<figcaption></figcaption>
</figure>
Then, save the bash file and close it.

NOTE:


- For applying all these changes to the current Terminal, execute the source command.
```ruby
    source ~/.bash_profile
```

**STEP 04:** Download JDBC driver for MySQL and place in $HIVE_HOME/lib folder

```ruby
    cp mysql-connector-java-5.1.18-bin.jar /Users/mimran/Data-Science/apache-hive-2.3.0-bin/lib/
```

**STEP 05:** Create hive-site.xml file under path "/Users/mimran/Data-Science/apache-hive-2.3.0-bin/conf" 

```ruby
    <configuration>
       <property>
          <name>javax.jdo.option.ConnectionURL</name>
          <value>jdbc:mysql://localhost:3306/hive?createDatabaseIfNotExist=true</value>
          <description>metadata is stored in a MySQL server</description>
       </property>
       <property>
          <name>javax.jdo.option.ConnectionDriverName</name>
          <value>com.mysql.jdbc.Driver</value>
          <description>MySQL JDBC driver class</description>
       </property>
       <property>
          <name>javax.jdo.option.ConnectionUserName</name>
          <value>root</value>
          <description>user name for connecting to mysql server</description>
       </property>
       <property>
          <name>javax.jdo.option.ConnectionPassword</name>
          <value>root</value>
          <description>password for connecting to mysql server</description>
       </property>
    </configuration>
```

**STEP 06:** To make sure that HIVE has been properly installed on your system and can be accessed through the Terminal, execute the following command.

```ruby
    hive
```

If you get similar results as follow then everything is fine with your HIVE installation

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-18/3.png"></a>
	<figcaption></figcaption>
</figure>

**NOTE 1:** In some cases, HIVE meta store won't created by the installation script if so we need to create the metastore manually as bellow for work HIVE properly  

```ruby
    mysql -u root -p
    Enter password:
    mysql> CREATE DATABASE metastore;
    mysql> USE metastore;
    mysql> SOURCE $HIVE_HOME/scripts/metastore/upgrade/mysql/hive-schema-2.3.0.mysql.sql;
```

**NOTE 2:** To Avoid "com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure"  Error we need to change the my.cnf file setting a follow

```ruby
    vi /etc/mysql/my.cnf
    #change bind-address = 127.0.0.1   to  bind-address = 0.0.0.0
```

**USEFUL commands**

```ruby
    #Enable DEBUG MODE on HIVE
    hive -hiveconf hive.root.logger=DEBUG,console
     
    #start metastore
    hive --service metastore
```