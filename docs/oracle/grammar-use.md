---
title: Oracle语法使用
slug: 
---

## 常见误区
### 1、推荐指定数据表的库名，如下
``` sql
SELECT * FROM ADMINDA1.ADM_USER;
```

### 2、指定数据库的别名，不能使用AS，如下
``` sql
SELECT * FROM ADMINDA1.ADM_USER USER;
```