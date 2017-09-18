---
layout: article
title: "Install and configure Apache Sqoop with vanilla Hadoop"
date: 2017-09-12
modified:
categories: articles
excerpt:
tags: [Data Science,hadoop,sqoop,Big Data]
ads: true
image:
  feature: 2017-09-12/1.png
  teaser: 2017-09-12/0.png
--- 

Apache Sqoop(TM) is a tool designed for efficiently transferring bulk data between Apache Hadoop and 
structured datastores such as relational databases.

#### Installation
**Pre-requisite**


- Apache Hadoop
- Apache Sqoop (compatible with Hadoop version)
- Apache Hive (optional)
- Apache HBase (optional)
- Apache HCatalog (optional)
- JDBC driver for MySQL (<a href="https://dev.mysql.com/downloads/connector/j/5.1.html">Download</a>)


#### Installing SQOOP

**STEP 01:** Download Apache Sqoop (This should be compatible with the version of Hadoop that you are installed)

**STEP 02:** Extract the Sqoop tar File.

```ruby
    tar -xvf sqoop-1.4.6.bin__hadoop-2.0.4-alpha.tar.gz
```

**STEP 03:** Add the Hadoop and Java paths in the bash_profile file (.bash_profile) as shown below.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-12/2.png"></a>
	<figcaption></figcaption>
</figure>
Then, save the bash file and close it.

NOTE:


- For applying all these changes to the current Terminal, execute the source command.
```ruby
    source ~/.bash_profile
```
- To make sure that sqoop has been properly installed on your system and can be accessed through the Terminal, 
execute the following command.

```ruby
    sqoop help
```
if you are getting the similar message as following for the above commands you are good to proceed with the further steps.
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-12/3.png"></a>
	<figcaption></figcaption>
</figure>

**STEP 04:** Download JDBC driver for MySQL and place in $SQOOP_HOME/lib folder

```ruby
    cp mysql-connector-java-5.1.18-bin.jar /Users/mimran/Data-Science/sqoop-1.4.6.bin__hadoop-2.0.4-alpha/lib/
```

**STEP 05:** Retrieving list of Databases available in MySQL from SQOOP to make sure everything working accordingly.

```ruby
    sqoop list-databases --connect jdbc:mysql://localhost:3306/  --username root -P
```

if you can see the databases from the MySQL server as follow that means your MySQL connector is working fine.
<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-09-12/4.png"></a>
	<figcaption></figcaption>
</figure>

