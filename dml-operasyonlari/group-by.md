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
```

#### 

#### CUBE - [https://www.oracletutorial.com/oracle-basics/oracle-cube/](https://www.oracletutorial.com/oracle-basics/oracle-cube/) 

#### ROLLUP - [https://www.oracletutorial.com/oracle-basics/oracle-rollup/](https://www.oracletutorial.com/oracle-basics/oracle-rollup/) 

```sql
--MS SQL ÖRNEK
select case when GROUPING_ID(Color)=0 
       then Color else 'Toplam' end,
       sum(ListPrice),GROUPING_ID(Color)
from Production.Product
group by Color
with rollup
```

#### PIVOT - [https://www.oracletutorial.com/oracle-basics/oracle-pivot/](https://www.oracletutorial.com/oracle-basics/oracle-pivot/) 

#### UNPIVOT - [https://www.oracletutorial.com/oracle-basics/oracle-unpivot/](https://www.oracletutorial.com/oracle-basics/oracle-unpivot/) 

#### CONNECT BY LEVEL



