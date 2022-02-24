---
title: MySQL 添加、更新和查询等常用SQL语句整理
date: 2022-02-13 11:53:33 +0800
categories: [Database, MySQL]
tags: [sql-guide]
---

### 根据另一张表更新当前表数据

```sql
UPDATE
  `audio_allot_statistics` AS a,
  `user` AS u,
  `user_week` AS w
SET
  a.p_icf = u.icf_no,
  a.h_id = u.h_id,
  a.p_join_time = u.create_time,
  a.p_end_time = w.end_time
WHERE
  a.p_id = u.id
  AND u.id = w.u_id
  AND u.id = 294232
  AND w.week_num = 12;
```

### 从一个表中查数据，插入另一个表

```sql
INSERT INTO 目标表 SELECT * FROM 来源表;
INSERT INTO 目标表 (字段1, 字段2, ...) SELECT 字段1, 字段2, ... FROM 来源表 ;
```

### 替换或添加

```sql

REPLACE INTO test_users_copy (user_id, user_name, user_status, id_card)
SELECT
  id,
  username,
  1,
  id_card,
FROM
  test_users
WHERE
  `username` = 'test';
```

###  聚合查询 “某字段” 内容相同的所有记录

```sql

SELECT
  sku_id,
  GROUP_CONCAT(
    spec_option_id
    ORDER BY
      spec_option_id ASC
  ) AS same_sku_ids
FROM
  sku_spec_option
WHERE
  spec_option_id IN (1, 4)
GROUP BY
  sku_id
HAVING
  same_sku_ids = "1,4";
```

> [**group_concat用法**](https://baijiahao.baidu.com/s?id=1595349117525189591&wfr=spider&for=pc)
