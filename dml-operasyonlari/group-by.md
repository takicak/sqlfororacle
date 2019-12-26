# Group By

#### GROUP BY FONKSIYONLARI 

#### \( COUNT\(\), SUM\(\), AVG\(\), MIN\(\), MAX\(\), STATS\_MODE\(\), LIST\_AGG\(\)  \) 

#### GROUP BY Fonksiyonlarının tamamı HAVING yada SELECT ifadelerinde kullanılabilir. 

```sql
--COUNT Genel Kullanım
SELECT E.DEPARTMENT_ID, COUNT(*) 
FROM EMPLOYEES E
GROUP BY E.DEPARTMENT_ID 
--------------------------------------------------------------
--------------------------------------------------------------
-- COUNT(), SUM(), AVG(), MIN(), MAX() Kullanımı
SELECT E.DEPARTMENT_ID, 
       D.DEPARTMENT_NAME, 
       COUNT(*), 
       SUM(E.SALARY), 
       AVG(E.SALARY), 
       MIN(E.SALARY), 
       MAX(E.SALARY)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY E.DEPARTMENT_ID, D.DEPARTMENT_NAME
--------------------------------------------------------------
--------------------------------------------------------------
--Bu kayıtlar incelendikten sonra
SELECT D.*, E.*
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID  --INNER JOIN
  AND E.DEPARTMENT_ID = 90

--Yukarıdaki Kayıtlar üzerinde 
--STATS_MODE fonksiyonu NUMBER alan için kullanılıyor
SELECT E.DEPARTMENT_ID, D.DEPARTMENT_NAME, STATS_MODE(SALARY)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
  AND E.DEPARTMENT_ID = 90
GROUP BY E.DEPARTMENT_ID, D.DEPARTMENT_NAME
--------------------------------------------------------------
--------------------------------------------------------------
--Bu kayıtlar incelendikten sonra
SELECT D.DEPARTMENT_NAME, COUNT(*)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY D.DEPARTMENT_NAME

--Yukarıdaki Kayıtlar üzerinde 
--STATS_MODE fonksiyonu STRING alan için kullanılıyor
SELECT STATS_MODE(D.DEPARTMENT_NAME)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
```

```sql
SELECT department_id, MAX(salary)
  FROM employees
  GROUP BY department_id;

--ENTERESAN BİR KULLANIM  
SELECT AVG(MAX(salary))
  FROM employees
  GROUP BY department_id;
```

#### 

#### GROUPING SETS

```sql
SELECT D.DEPARTMENT_NAME, SUM(E.SALARY)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY D.DEPARTMENT_NAME
UNION ALL
SELECT NULL, SUM(E.SALARY) FROM EMPLOYEES E;
==
SELECT D.DEPARTMENT_NAME, SUM(E.SALARY)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
GROUPING SETS ( (D.DEPARTMENT_NAME),
                (NULL)
               ) --BU PARANTEZ İÇİNDE DAHA FAZLA GRUP OLABİLİR.

--ÖRNEĞİN;
SELECT D.DEPARTMENT_NAME, E.JOB_ID, SUM(E.SALARY)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
GROUPING SETS ( (D.DEPARTMENT_NAME, E.JOB_ID),
                (D.DEPARTMENT_NAME),
                (E.JOB_ID),
                (NULL)
               )
--ORDER BY GROUPING_ID(NULL)

SELECT GROUPING(DEPARTMENT_NAME), GROUPING(JOB_ID), GROUPING(NULL), D.DEPARTMENT_NAME, E.JOB_ID, SUM(E.SALARY)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY
GROUPING SETS ( (D.DEPARTMENT_NAME, E.JOB_ID),
                (D.DEPARTMENT_NAME),
                (E.JOB_ID),
                (NULL)
               )
ORDER BY DEPARTMENT_NAME NULLS LAST, JOB_ID NULLS LAST;
ORDER BY GROUPING(NULL);
```

#### 

#### CUBE - [https://www.oracletutorial.com/oracle-basics/oracle-cube/](https://www.oracletutorial.com/oracle-basics/oracle-cube/) 

CUBE fonksiyonuna verilen 2 kolon için 2^2 adet GROUPING SETS oluşturur. Örneğin; 3 kolon için 8 GROUPING SETS oluşturur.

```sql
GROUPING SETS ( (D.DEPARTMENT_NAME, E.JOB_ID),
                (D.DEPARTMENT_NAME),
                (E.JOB_ID),
                (NULL)
               )
-- YERİNE
CUBE(D.DEPARTMENT_NAME, E.JOB_ID)
-- KULLANILABILIR
```

```sql
SELECT DEPARTMENT_ID, COUNT(SALARY), SUM(SALARY)
FROM EMPLOYEES E
GROUP BY DEPARTMENT_ID;

SELECT DEPARTMENT_ID, COUNT(SALARY), SUM(SALARY)
FROM EMPLOYEES E
GROUP BY CUBE(DEPARTMENT_ID);

SELECT DEPARTMENT_ID, E.JOB_ID, COUNT(SALARY), SUM(SALARY)
FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID IN (30,100)
GROUP BY DEPARTMENT_ID, E.JOB_ID;

SELECT DEPARTMENT_ID, E.JOB_ID, COUNT(SALARY), SUM(SALARY)
FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID IN (30,100)
GROUP BY CUBE(DEPARTMENT_ID, E.JOB_ID);
--ORDER BY DEPARTMENT_ID NULLS LAST, JOB_ID NULLS LAST; -- ***

-- *** EN PRATİK KULLANIMI ***
SELECT DEPARTMENT_ID, E.JOB_ID, COUNT(SALARY), SUM(SALARY)
FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID IN (30,100)
GROUP BY DEPARTMENT_ID, CUBE(E.JOB_ID)
ORDER BY DEPARTMENT_ID NULLS LAST, JOB_ID NULLS LAST;
-- *** ÖDEV TOPLAM SATIRINI İNSAN OKUSUN ***


```



#### ROLLUP - [https://www.oracletutorial.com/oracle-basics/oracle-rollup/](https://www.oracletutorial.com/oracle-basics/oracle-rollup/) 

```sql
--MS SQL ÖRNEK
select case when GROUPING_ID(Color)=0 
       then Color else 'Toplam' end,
       sum(ListPrice),GROUPING_ID(Color)
from Production.Product
group by Color
with rollup
```#### ROLLUP - [https://www.oracletutorial.com/oracle-basics/oracle-rollup/](https://www.oracletutorial.com/oracle-basics/oracle-rollup/) 

#### PIVOT - [https://www.oracletutorial.com/oracle-basics/oracle-pivot/](https://www.oracletutorial.com/oracle-basics/oracle-pivot/) 

#### UNPIVOT - [https://www.oracletutorial.com/oracle-basics/oracle-unpivot/](https://www.oracletutorial.com/oracle-basics/oracle-unpivot/) 

#### CONNECT BY LEVEL



<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY5MTM3NzYwNl19
-->