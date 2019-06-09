---
layout: post
title: "Getting column name bound with Unique Constraint"
date: 2019-06-09 10:34:38 -0300
comments: true
categories: mysql
---

Here you can see, how we can get the column name bound with specific unique constraint name...<!--more-->For example:

{% highlight mysql %}
select k.COLUMN_NAME
	from information_schema.KEY_COLUMN_USAGE k
	join information_schema.TABLE_CONSTRAINTS tc on k.TABLE_SCHEMA = tc.TABLE_SCHEMA 
	and k.TABLE_SCHEMA = tc.TABLE_SCHEMA
	and k.CONSTRAINT_NAME = tc.CONSTRAINT_NAME
	where k.table_name = 'table_name'
	and constraint_type = 'UNIQUE';
{% endhighlight %}
