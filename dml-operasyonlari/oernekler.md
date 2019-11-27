# Connect By Level

```sql
--LEVEL ile 1000 KAYITLIK RANDOM VERİ ÜRETİLİP, 
--TABLOYA DOLDURULUYOR
DROP TABLE dimension_tab; 
CREATE TABLE dimension_tab ( fact_1_id NUMBER NOT NULL, 
                             fact_2_id NUMBER NOT NULL, 
                             fact_3_id NUMBER NOT NULL,
                             fact_4_id NUMBER NOT NULL, 
                             sales_value NUMBER(10,2) NOT NULL );
                             
INSERT INTO dimension_tab 
SELECT TRUNC(DBMS_RANDOM.value(low => 1, high => 3)) 
    AS fact_1_id, TRUNC(DBMS_RANDOM.value(low => 1, high => 6)) 
    AS fact_2_id, TRUNC(DBMS_RANDOM.value(low => 1, high => 11)) 
    AS fact_3_id, TRUNC(DBMS_RANDOM.value(low => 1, high => 11)) 
    AS fact_4_id, ROUND(DBMS_RANDOM.value(low => 1, high => 100), 2) 
    AS sales_value 
FROM DUAL
CONNECT BY level <= 1000;
COMMIT;

--------------------------------------------------------------------------------------------
--------------------------------------------------------------------------------------------
--LEVEL ile 2014-2015 senelerinin tüm günleri seçildikten sonra
--dış sorgu ile tekrar sorgulanarak sadece ay sonları filtreleniyor.
--Sorguda herhangi bir tablonun kullanılmadığına dikkat ediniz.
SELECT TO_DATE('01.01.2014','DD.MM.YYYY') + LEVEL AS TUM_GUNLER, 
       LEVEL, 
       TO_DATE('01.01.2016','DD.MM.YYYY') - TO_DATE('01.01.2014','DD.MM.YYYY') 
FROM DUAL 
CONNECT BY LEVEL <= TO_DATE('01.01.2016','DD.MM.YYYY') - TO_DATE('01.01.2014','DD.MM.YYYY')


SELECT TO_CHAR(TUM_GUNLER,'DD.MM.YYYY') AS AY_SONLARI 
FROM 
 ( SELECT TO_DATE('01.01.2014','DD.MM.YYYY') + LEVEL AS TUM_GUNLER, 
          LEVEL, 
          TO_DATE('01.01.2016','DD.MM.YYYY') - TO_DATE('01.01.2014','DD.MM.YYYY')
   FROM DUAL 
   CONNECT BY LEVEL <= TO_DATE('01.01.2016','DD.MM.YYYY') - TO_DATE('01.01.2014','DD.MM.YYYY') 
 ) 
WHERE TUM_GUNLER = LAST_DAY(TUM_GUNLER)
```



```sql
--LEVEL ile 1-1000 arası sayılar kümesi, kendi kendisi ile join kurularak
--HAVING ile sadece asal sayıların listelenmesi sağlanıyor.
--Sorguda herhangi bir tablonun kullanılmadığına dikkat ediniz.

WITH all_numbers 
  AS ( SELECT LEVEL AS nmbrs 
       FROM dual 
       CONNECT BY LEVEL <=1000 )
SELECT a.nmbrs,                       
       MIN(CASE WHEN ROUND(a.nmbrs/b.nmbrs)=(a.nmbrs/b.nmbrs)
                THEN 0 ELSE 1 END ) prime_flag 
FROM all_numbers a CROSS JOIN all_numbers b 
WHERE a.nmbrs<>b.nmbrs 
  AND a.nmbrs<>1 
  AND b.nmbrs<>1 
GROUP BY a.nmbrs 
HAVING MIN(CASE WHEN ROUND(a.nmbrs/b.nmbrs)=(a.nmbrs/b.nmbrs) 
                THEN 0 ELSE 1 END ) = 1 
ORDER BY 1;
```





