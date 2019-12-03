# Where

#### WHERE komutu veri çekilirken, verilen kriterlere göre verinin filtrelenerek istenilen kayıtlara ulaşılmasını sağlar. Genel kullanımı aşağıdaki gibidir.

```sql
SELECT * FROM EMPLOYEES E
WHERE E.FIRST_NAME = 'David'
```

#### UPPER,INITCAP, LOWER

```sql
SELECT E.*, 
       UPPER(E.FIRST_NAME)   F_UPER,    UPPER(E.LAST_NAME)   L_UPPER,
       INITCAP(E.FIRST_NAME) F_INITCAP, INITCAP(E.LAST_NAME) L_INITCAP,
       LOWER(E.FIRST_NAME)   F_LOWER,   LOWER(E.LAST_NAME)   L_LOWER
       FROM EMPLOYEES E
WHERE UPPER(E.FIRST_NAME) = 'DAVID';
```

#### 

#### LIKE

```sql
SELECT * FROM EMPLOYEES E
WHERE UPPER(E.LAST_NAME) LIKE '%OZ%'
```

#### 

#### AND

```sql
SELECT * FROM EMPLOYEES E
WHERE E.FIRST_NAME = 'David'
  AND E.LAST_NAME = 'Lee'
```

#### 

#### OR \(PARANTEZ TEHLIKESI\)

```sql
--Hatalı Kullanım; MAR içeren tüm kayıtları da listeye ekler.
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID = 80
  AND E.SALARY > 5000
  AND E.MANAGER_ID = 148
  AND UPPER(E.LAST_NAME) LIKE '%OZ%' 
   OR UPPER(E.LAST_NAME) LIKE '%MAR%'

--Doğru Kullanım   
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID = 80
  AND E.SALARY > 5000
  AND E.MANAGER_ID = 148
  AND (    UPPER(E.LAST_NAME) LIKE '%OZ%' 
        OR UPPER(E.LAST_NAME) LIKE '%MAR%' )
```

#### 

#### 

#### BETWEEN

```sql
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID BETWEEN 10 AND 50
```

#### 

#### NOT

```sql
SELECT * FROM EMPLOYEES E
WHERE UPPER(E.LAST_NAME) LIKE '%OZ%';

SELECT * FROM EMPLOYEES E
WHERE NOT (   UPPER(E.LAST_NAME) LIKE '%OZ%' 
           OR UPPER(E.LAST_NAME) LIKE '%MAR%' );

```

#### 

#### IS NULL: NULL içeren kayıtları çekmek için kullanılır.

```sql
SELECT * FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NULL;
```

#### 

#### IS NOT NULL: NULL içermeyen kayıtları çakmek için kullanılır.

```sql
SELECT * FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NOT NULL;
```



#### NULL Veri Sorgulama Problemleri

```sql
--Yanlış Kullanım, Kayıt Getirmez
SELECT * FROM EMPLOYEES E
WHERE E.MANAGER_ID = NULL

--Doğru Kullanım, bir kayıt getirir
SELECT * FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NULL

--Kayıt Sayısı 1 Döner Sorun Yok.
SELECT COUNT(*) FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NULL

--Kayıt Sayısı 1 Döner Sorun Yok.
SELECT COUNT(E.EMPLOYEE_ID) FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NULL

--Kayıt Sayısı 0 Döner Sorun Var.
SELECT COUNT(E.MANAGER_ID) FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NULL

--NVL Kullanımı ile yukarıdaki sorun düzeltildi.
SELECT COUNT( NVL(E.MANAGER_ID,0) ) FROM EMPLOYEES E
WHERE E.MANAGER_ID IS NULL

--Where kriterinde IS NULL yerine NVL kullanımı.
SELECT COUNT( NVL(E.MANAGER_ID,0) ) FROM EMPLOYEES E
WHERE NVL(E.MANAGER_ID, 0) =  0

--MANAGER_ID alanı NULL olan kayıt listelenmez. Problem Var.
SELECT * FROM EMPLOYEES E
WHERE E.MANAGER_ID <> 100   

--MANAGER_ID alanı NULL olan kayıt listelenir. Problem Düzeldi.
SELECT * FROM EMPLOYEES E
WHERE NVL(E.MANAGER_ID, -1) <> 100




```

#### NVL\(\)\*  Oracle PL/SQL dilinde NULL değer kontrolünde kullanılır.

#### _ISNULL\(\)+  MS-SQL TSQL dilindeki karşılığı_

#### Detaylı Bilgi için : [https://www.w3schools.com/sql/sql\_isnull.asp](https://www.w3schools.com/sql/sql_isnull.asp)

```sql
NVL(NUMBER, 0) --Number alanlar için NVL Kullanımı
NVL(STR, 'T')  --String alanlar için NVL Kullanımı
NVL(DATE, '01.01.3000') --Date alanlar için NVL Kullanımı
```

####   __

#### _MOD\(\) MATEMATIKSEL MODE ALMA FONKSIYONU_ 

```sql
--- Matematikdeki 324 MOD 5 e denk gelir. 
SELECT MOD(321, 5) FROM DUAL;
```

\_\_

#### _DECODE_ \* 

Oracle üzerinde CASE komutu yerine kullanılan DECODE komutu ile hızlı bir şekilde değer kontrolü yapılarak farklı ifadeler döndürülebilir. Hem SELECT hem de WHERE bloklarında kullanılabilir. NULLIF+ MS-SQL deki karşılığı bu olabilir.

```sql
SELECT DECODE(E.SALARY, 3000,1, 4000,2, 5000,3, 4) 
FROM EMPLOYEES E
WHERE E.SALARY IN (3000,4000,5000, 6000);
```



#### DECODE KULLANIMI PRATIK ÖRNEK 

Oracle SP PARAMETRELERİNİN BAZISI DOLU BAZISI NULL OLDUĞUNDA, sadece verilen kriterlere göre çalışması, verilmeyen kriterler için tüm kayıtları getirmesi için.

```sql
--DECODE KULLANIMI PRATIK YONTEM
--HIRE_DATE:'01.01.2005', SALARY:3000
SELECT * FROM EMPLOYEES E
WHERE E.HIRE_DATE >= &DATE_
  AND E.SALARY    >= &SALARY_

SELECT * FROM EMPLOYEES E
WHERE ( ( E.HIRE_DATE >= &DATE_   AND &DATE_   IS NOT NULL ) OR ( E.HIRE_DATE >= E.HIRE_DATE AND &DATE_   IS NULL ) )
  AND ( ( E.SALARY    >= &SALARY_ AND &SALARY_ IS NOT NULL ) OR ( E.SALARY    >= E.SALARY    AND &SALARY_ IS NULL ) );

SELECT * FROM EMPLOYEES E
WHERE E.HIRE_DATE >= DECODE( NVL(&DATE_  ,'01.01.1900'), '01.01.1900',E.HIRE_DATE, &DATE_  )
  AND E.SALARY    >= DECODE( NVL(&SALARY_, 0          ),           0 ,E.SALARY   , &SALARY_);    
```

#### 

####  IN

Filitrelenecek tablo kolonunun, istenen listenin içinde olması istendiğinde kullanılır.

```sql
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID IN (90, 60);

SELECT * FROM DEPARTMENTS D, LOCATIONS L
WHERE D.LOCATION_ID = L.LOCATION_ID
  AND L.STATE_PROVINCE = 'Washington'
  
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID IN (  SELECT D.DEPARTMENT_ID 
                            FROM DEPARTMENTS D, LOCATIONS L
                            WHERE D.LOCATION_ID = L.LOCATION_ID
                              AND L.STATE_PROVINCE = 'Washington'
                           ) --SUBQUERY bir kez çalıştırılır.
```

#### 

#### NOT IN

Filitrelenecek tablo kolonunun, istenen listenin içinde olmaması istendiğinde kullanılır.

```sql
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID NOT IN (90, 60)
```

#### 

#### ANY \(yada SOME\) 

Daha fazla bilgi için: [https://www.oracletutorial.com/oracle-basics/oracle-any/](https://www.oracletutorial.com/oracle-basics/oracle-any/) 

```sql
-- ANY = İLE KULLANILDIĞINDA IN KULLANIMINDAN FARKI YOK
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID = ANY (90, 60)
==
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID = 90 OR E.DEPARTMENT_ID = 60

-- ANY DE DİĞER OPERATÖRLER DE KULLANILABİLİYOR
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID < ANY (90, 60)
==
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID < 90 OR E.DEPARTMENT_ID < 60;
```

#### 

#### ANY Kullanımı için Görev : 

EMPLOYEES \(ÇALIŞAN\) TABLOSUNDA BULUNAN SALARY \(MAAŞ\) RAKAMLARI, AYNI İŞ İÇİN JOB TABLOSUNDA BULUNAN MIN MAX MAAŞ ARALIĞINDA MI?

```sql
SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.JOB_ID,
       ( SELECT J.MIN_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )MINSAL,  --CORRELATED SUBQUERY
       E.SALARY,
       ( SELECT J.MAX_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )MAXSAL   --CORRELATED SUBQUERY
FROM EMPLOYEES E
WHERE ( E.SALARY >= ANY ( SELECT J.MIN_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )
    AND E.SALARY --<
      = ANY ( SELECT J.MAX_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )
      )
```



#### ALL - [https://www.oracletutorial.com/oracle-basics/oracle-all/](https://www.oracletutorial.com/oracle-basics/oracle-all/) 

```sql
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID <= ALL (30, 20);
==
SELECT * FROM EMPLOYEES E
WHERE E.DEPARTMENT_ID <= 30 AND E.DEPARTMENT_ID <= 20;
```



#### EXISTS : ÇEKİLEN SORGUDAN KAYIT GELİP \(EXISTS\), GELMEDİĞİNE BAKAR.

```sql
SELECT * FROM EMPLOYEES E
WHERE EXISTS (SELECT 1 FROM DUAL)

SELECT * FROM EMPLOYEES E
WHERE EXISTS (SELECT 1 FROM DUAL WHERE 1=2)

SELECT * FROM EMPLOYEES E
WHERE EXISTS ( SELECT 1 FROM DUAL WHERE MOD(E.EMPLOYEE_ID,2)=1 ) --SUBQUERY

SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.JOB_ID,
       ( SELECT J.MIN_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )MINSAL,  --CORRELATED SUBQUERY
       E.SALARY,
       ( SELECT J.MAX_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )MAXSAL   --CORRELATED SUBQUERY
FROM EMPLOYEES E
WHERE EXISTS (SELECT 1 FROM JOBS J WHERE J.JOB_ID = E.JOB_ID AND E.SALARY BETWEEN J.MIN_SALARY AND J.MAX_SALARY )

SELECT E.EMPLOYEE_ID, E.FIRST_NAME, E.LAST_NAME, E.JOB_ID,
       ( SELECT J.MIN_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )MINSAL,
       E.SALARY,
       ( SELECT J.MAX_SALARY FROM JOBS J WHERE J.JOB_ID = E.JOB_ID )MAXSAL
FROM EMPLOYEES E
WHERE EXISTS (SELECT 1 FROM JOBS J WHERE J.JOB_ID = E.JOB_ID AND E.SALARY = J.MAX_SALARY )
```

#### SUBQUERY Kullanımı

```sql
SELECT * FROM EMPLOYEES E 
WHERE E.SALARY >= ( SELECT AVG(EE.SALARY) FROM EMPLOYEES EE ) 
                 -- SUBQUERY ORDER BY E.SALARY
```

