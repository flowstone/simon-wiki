---
title: Oracle常用命令
slug: 
---

## 常用脚本
### 1、批量插入数据
``` sql
INSERT ALL
<foreach collection="groupDepts" item="item" index="index">
    INTO ADM_GROUP_DEPT
    <trim prefix="(" suffix=")">
        GROUP_ID, DEPT_ID,DEPT_NAME
    </trim>
    <trim prefix="VALUES (" suffix=")">
        #{item.groupId},
        #{item.deptId},
        #{item.deptName}
    </trim>
</foreach>
SELECT 1 FROM DUAL
```

### 2、分组拼接成虚拟表
``` sql
SELECT GROUP_ID, (LISTAGG(USERNAME,',') within group ( order by GROUP_ID)) AS username
      FROM ADMINDA1.ADM_GROUP_USER
      GROUP BY GROUP_ID
```

### 3、判断日期是否在范围内
``` sql
SELECT ID, TITLE, TYPE, TIME_TYPE, SEND_DATE,
       END_DATE, RECEIVE_PERSON
FROM POR_ANNOUNCEMENT
WHERE (SEND_DATE IS NULL OR SYSDATE > SEND_DATE)
AND (END_DATE IS NULL OR END_DATE > SYSDATE)
ORDER BY CREATE_TM DESC
```

### 4、第一个查询无结果采用第二个查询结果
``` sql
SELECT ID,USER_NAME,
NVL((SELECT NAME FROM ORG_EMPLOYEE WHERE ID = CREATED_BY), 
(SELECT USER_NAME FROM ORG_CUSTOMER_ACCOUNT WHERE ID = CREATED_BY)) AS name 
FROM ORG_CUSTOMER_ACCOUNT WHERE ID = '1369819577617219584';
```

### 5、递归查询
``` sql
-- 客户账号A创建客户账号B，客户账号B的创建人就客户A的ID，
-- 客户账号B创建客户账号C, 客户账号C的创建人就是客户B的ID
-- 向下递归
SELECT ID, USER_NAME
        FROM ORG_CUSTOMER_ACCOUNT
        START WITH ID='1374231638187180032'
        CONNECT BY PRIOR  ID =  CREATED_BY 
-- 向上递归
UNION SELECT ID,USER_NAME
        FROM ORG_CUSTOMER_ACCOUNT
        START WITH ID='1374231638187180032'
        CONNECT BY PRIOR  CREATED_BY =  ID
```