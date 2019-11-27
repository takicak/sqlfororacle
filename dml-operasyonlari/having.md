# Having

HAVING komutu, bir GROUP BY komutundan sonra kullanılır ve verinin gruplandıktan sonra tekrar filtrelenmesini amaçlar. Bu açıdan bakıldığında WHERE komutunun gruplanmadan sonra çalışan hali diyebiliriz.

```sql
SELECT D.DEPARTMENT_NAME, COUNT(*)
FROM EMPLOYEES E, DEPARTMENTS D
WHERE E.DEPARTMENT_ID = D.DEPARTMENT_ID
GROUP BY D.DEPARTMENT_NAME
HAVING COUNT(*) > 10
```



