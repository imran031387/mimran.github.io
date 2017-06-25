---
layout: article
title: "Data Migration From XML File to Postgres Database Using Talend"
date: 2017-06-27
modified:
categories: articles
excerpt:
tags: [xml,postgres,migration,talend]
ads: true
image:
  feature: 
  teaser: 2017-06-27/0.gif
--- 

In this blog post, I’m going to discuss how to migrate data which reside within an XML file to Postgres 
database using TALEND before that I'll give a brief introduction about what is TALEND.

#### What is TALEND?

Talend is a next-generation leader in cloud and big data integration software that helps companies become 
data driven by making data more accessible, improving its quality and quickly moving it where it’s needed 
for real-time decision making. Talend’s open-source, native, and unified integration platform, Data Fabric, 
enables customers to embrace new innovations and scale to meet the evolving data demands of the business.

Ok let's move on to the tutorial I assume that you have pre-installed the TALEND OPEN STUDIO community edition
or you can find installation guide from the following link

```ruby
   http://www.talendforge.org/wiki/doku.php?id=doc:installation_guide
```

Once you setup the environment first thing we need to to is to create a project as below

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/1.png"></a>
	<figcaption></figcaption>
</figure>

By clicking the `Create a new project` button you can create a project with the name you 
desired for the project

Next, we need to create a "JOB" for that you need to follow the following steps.

1. `Click`, `JOB DESIGN` from the left panel
2. Then, right click on the `standard` link and create a "JOB" with a specific name and the purpose.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/2.png"></a>
	<figcaption></figcaption>
</figure>

Now we have all set up to implement our migration, next we need to import XML file to extract the data, 
for this we need to follow following steps

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/3.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/4.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/5.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/6.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/7.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/8.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/9.png"></a>
	<figcaption></figcaption>
</figure>

Once you created the XML input it'll appear as following and you can drag and drop 
that to the `JOB WORKPLACE` and create tFileInputXML component as below.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/10.png"></a>
	<figcaption></figcaption>
</figure>

Next, we need to map our extracted data into the Postgres schema for that we can use tXMLMap 
component by typing the name on the "JOB WORKPLACE" as follow.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/11.png"></a>
	<figcaption></figcaption>
</figure>

Then right click the Payroll_XML component and select `main` from the `row` and 
connect to `tXMLMap_1` then you will see as follow.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/12.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/13.png"></a>
	<figcaption></figcaption>
</figure>

Next, You need to map extracted fields from the XML to Database Schema by following below steps
First, `Double` click `tXMLMap_1` and then you will see the following screen

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/14.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/15.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/16.png"></a>
	<figcaption></figcaption>
</figure>

Then, You need to click the "Auto Map" button as per shown in Figure 14. Then you will see a mapped 
connection between XML fields and the database fields as follow

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/17.png"></a>
	<figcaption></figcaption>
</figure>

As a Final Step next we need setup the Postgres connection, For that first, you need to create a 
component `tPostgresqlOutput` by typing the name on the `JOB WORKPLACE` as below.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/18.png"></a>
	<figcaption></figcaption>
</figure>

and we need to setup the Postgres connection details as following by double clicking the `tPostgresqlOutput_1` 
component and then we need to connect the `tXMLMap_1` and the `tPostgresqlOutput_1` as follow

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/19.png"></a>
	<figcaption></figcaption>
</figure>

and our final output of the "JOB" file will be like this 

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/20.png"></a>
	<figcaption></figcaption>
</figure>

Then we can run the job file and check whether our migration is working or not if every thing went well
we can see the follow output.

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/21.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/22.png"></a>
	<figcaption></figcaption>
</figure>

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-27/23.png"></a>
	<figcaption></figcaption>
</figure>

and that's it like this you can migrate data from xml to postgres following video is showing a demo
for migrating a complex xml which are having nested data to postgres database.


<iframe width="560" height="315" src="//www.youtube.com/embed/uEcRBc_245c" frameborder="0"> </iframe>





