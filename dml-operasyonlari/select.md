# Select

#### SELECT : Veritabanından kayıt okumayı sağlayan komuttur. \(\*\) yıldız ifadesi sorgulanan tablonun tüm kolonlarının gösterilmesini sağlar.

```sql
--107 TOPLAM ÇALIŞAN
SELECT * FROM EMPLOYEES
```

#### 

#### ALIAS TANIMLAMA : İsimlendirme. Kolonlar için alias tanımı \(As\) ve Tablolarda alias tanımlama.

```sql
-- KOLONLAR İSİMLENDİRİLİRKEN AS İFADESİ KULLANILMASA DA ÇALIŞIR
SELECT E.FIRST_NAME AS AD, 
       E.LAST_NAME     SOYAD 
FROM EMPLOYEES E; 
--TABLOLARIN KISA ISIMLENDIRILMASI KODALAMADA KOLAYLIK SAĞLAR
```

#### 

#### DISTINCT : Çekilen kayıtların tekrarlanmamasını sağlar.

```sql
--107 PERSONELE AIT MUDUR SAYISI 19 OLARAK GÖRÜNTÜLENİR
SELECT DISTINCT E.MANAGER_ID FROM EMPLOYEES E;
```

#### 

#### STRING Birleştirme Operatörü \( \|\| \)\*

MS-SQL üzerinde string birleştirmek için \( + \) operatörü kullanabilirsiniz.

```sql
SELECT E.FIRST_NAME || ' ' || E.LAST_NAME ADSOYAD 
FROM EMPLOYEES E
```

####  

#### SUBSTR : String alan üzerinden kırpma, kesme

```sql
SELECT SUBSTR(E.FIRST_NAME,1,1)F, SUBSTR(E.FIRST_NAME,-1,1)FS, E.FIRST_NAME,
       SUBSTR(E.LAST_NAME,1,1)L,  SUBSTR(E.LAST_NAME,-1,1)LS,  E.LAST_NAME
FROM EMPLOYEES E


```

#### 

#### DUAL\* : Tablosuz Select çekmeyi sağlar. Fonksiyonların test edilmesinde kullanılabilir.

```sql
SELECT 1 AS DUMMY FROM DUAL;
SELECT SYSDATE AS SIMDI FROM DUAL;
```

#### 

#### SYSDATE : Sistemden canlı zaman bilgisinin alındığı fonksiyon.

```sql
SELECT SYSDATE, 
       SYSDATE-1, 
       SYSDATE - (1/24), 
       SYSDATE - (1/1440) 
FROM DUAL;
```

####  TRUNC\(\) : Ondalık kesme fonksiyonu.       ROUND\(\) : Ondalık yuvarlama fonksiyonu.

```sql
--Number Alanlar Üzerinde Kullanım
SELECT E.SALARY, 
       E.SALARY/3, 
       TRUNC(E.SALARY/3), 
       ROUND(E.SALARY/3), 
       ROUND(E.SALARY/3,2)  
FROM EMPLOYEES E;

--Date Alanlar Üzerinde Kullanım
--Tarih formatlarında, saat bilgisi ondalık olarak saklanır 
SELECT SYSDATE, 
       SYSDATE-1, 
       TRUNC(SYSDATE), 
       TRUNC(SYSDATE-1) 
FROM DUAL;
```



#### Hesaplanmış bir kolon kullanımı.

```sql
SELECT E.FIRST_NAME AD, 
       E.LAST_NAME SOYAD, 
       E.SALARY MAAS, 
       E.SALARY * 0.15 KDV 
FROM EMPLOYEES E 
```



#### SELECT CASE, 

```sql
SELECT E.FIRST_NAME, E.LAST_NAME, E.SALARY,
       CASE WHEN E.SALARY < 2000 THEN 'FAKİR'
            WHEN E.SALARY BETWEEN 2000 AND 8000
                 THEN 'ORTA DİREK'
        ELSE 'ZENGİN' END DURUM
FROM EMPLOYEES E
```



#### MOD\(\) fonksiyonu. Matematikteki mod alma işlevini yerine getirir.

```sql
SELECT MOD(358, 2), MOD(357, 2) FROM DUAL;
```



#### DECODE\* : Oracle üzerinde kısa case kullanımını sağlan bir fonksiyondur.

```sql
SELECT E.EMPLOYEE_ID, 
       MOD(E.EMPLOYEE_ID, 2) TEKCIFT, 
       E.FIRST_NAME, E.LAST_NAME, 
       CASE WHEN MOD(E.EMPLOYEE_ID, 2)=0 THEN 'ÇİFT'
       ELSE 'TEK' END TEKCIFTSTR
FROM EMPLOYEES E;

--Her iki sorgu da aynı sonucu döner

SELECT E.EMPLOYEE_ID, 
       MOD(E.EMPLOYEE_ID, 2) TEKCIFT, 
       E.FIRST_NAME, E.LAST_NAME, 
       DECODE( MOD(E.EMPLOYEE_ID, 2), 0,'ÇİFT', 1,'TEK', 'BİLEMEM') TEKCIFTSTR
FROM EMPLOYEES E;
```



