---
layout: article
title: "Comparison between Query builders(EntityFieldQuery, db_select, db_query) in Drupal 7"
date: 2017-06-23
modified:
categories: articles
excerpt:
tags: [query,drupal7]
ads: true
image:
  feature: 
  teaser: 2017-06-23/0.png
---  

One of major functionalities of Drupal 7 is  database abstraction layer this letting us to create complex 
static and dynamic queries easy and  more readable manner by using query builders but unfortunately there 
are some significant performance differences among the query builders.

#### Performance differences at a glance

* For simple queries, db_query() is `22% faster` than db_select()
* For simple queries, db_query() is `124% faster` than EFQ
* For queries with two joins, db_query() is `29% faster` than db_select()


### db_query.

we can use db_query on following cases

* When we have a simple or static query
* When query will be run multiple times per page
* When will never need to switch databases or support more than one database engine
* When we don't need any of the extra functionality provided by query builders such as table column sorting or a pager

```ruby
$sql = "SELECT name, quantity FROM goods WHERE vid = :vid";
$result = db_query($sql, array(':vid' => $vid));
if ($result) {
    while ($row = $result->fetchAssoc()) {
        //Do anything with the results. 
 }
}
```

### db_select.

We can use db_select on following cases

* When creating a dynamic query and want to avoid lots of messy string concatenation
* When want to make use of query extenders such as table column sorting or a pager
* When want to allow other modules to alter your query, such as the node access system
* When need to support multiple databases with the same code base
* When readability is more important than performance of the query
* The query is long-running or not used often so a small amount of overhead is negligible

```ruby
$query = db_select('node', 'n');
$query->join('users', 'u', 'n.uid = u.uid'); //JOIN node with users
$query->groupBy('u.uid');//GROUP BY user ID
$query->fields('n',array('title','created'))//SELECT the fields from node
->fields('u',array('name'))//SELECT the fields from user
->orderBy('created', 'DESC')//ORDER BY created
->range(0,2);//LIMIT to 2 records
$result = $query->execute();
while($record = $result->fetchAssoc()) {
    //Do anything with the results.
}
```

### EntityFieldQuery.

EntityFieldQuery is far more limited than db_select only benefit we gain by using this is flexibilty further EFQ using 
inner joints in case if we need other types of joints we cant do that by using EFQ.

```ruby
$query=new EntityFieldQuery();
$query->entityCondition('entity_type', 'node')
      ->entityCondition('bundle', 'article')
      ->propertyCondition('status', 1)
      ->fieldCondition('field_news_types', 'value', 'spotlight', '=')
      ->fieldCondition('field_photo', 'fid', 'NULL', '!=')
      ->fieldCondition('field_faculty_tag', 'tid', $value)
      ->fieldCondition('field_news_publishdate', 'value', $year. '%', 'like')
      ->range(0, 10)
      ->addMetaData('account', user_load(1)); // Run the query as user 1.

$result = $query->execute();
if (isset($result['node'])) {
    $nids = array_keys($result['node']);
 $nodes = entity_load('node', $nids);
}
```

## Conclusions

<figure>
	<a href="#"><img src="{{ site.url }}/images/2017-06-23/1.png"></a>
	<figcaption></figcaption>
</figure>
