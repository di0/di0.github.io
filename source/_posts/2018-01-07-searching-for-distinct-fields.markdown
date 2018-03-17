---
layout: post
title: "MySQL - Searching for distinct fields"
date: 2018-01-07 21:42:23 -0200
comments: true
categories: [mysql]
---

When you haven't idea where one or more columns name are available, you can search them on all table of the database, using <!--more--> the following clause:


```sql
SELECT DISTINCT TABLE_NAME 
    FROM INFORMATION_SCHEMA.COLUMNS
    WHERE COLUMN_NAME IN ('COLUMN_NAME_DESIRED')
AND TABLE_SCHEMA='DATABASE_TARGET';
```

Where **COLUMN_NAME_DESIRED** is the name of the column to be searched and **DATABASE_TARGET** is the database where it must be searched.
