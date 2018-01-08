---
layout: post
title: "Searching for table name"
date: 2013-05-13 21:30:04 -0200
comments: true
categories: [mysql]
---

No MySQL, tais informações metadatas, estão armazenadas na tabela **database_name**, portanto,
um simples **SHOW TABLES** nessa tabela, filtrando pelo nome da tabela desejada. Segue:

```sql
SHOW TABLES FROM database_name LIKE "%table_name%";
```
