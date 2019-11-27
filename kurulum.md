# Oracle Platformu Kurulumu

Temel SQL eğitimi süresince kullanılacak, örnek sorguların büyük bir çoğunlu Oracle kurulumu ile gelen HR scheması üzerinde çalıştırılabilir. Aşağıdaki link üzerinden Oracle kurulum dosyalarını edinebiliriniz.

Oracle Database Express Edition \(XE\) Release 18.4.0.0.0 \(18c\) [https://www.oracle.com/database/technologies/xe-downloads.html](https://www.oracle.com/database/technologies/xe-downloads.html) 

Default olarak kilitli gelen HR \(Human Resource\) Schema sı için aşağıdaki kodları SYS userı ile çalıştırmanız gerekmektedir.

```sql
ALTER USER HR ACCOUNT UNLOCK; 
ALTER USER HR IDENTIFIED BY HR;
```

Sorguları çalıştırmak için PL/SQL Developer yada Oracle SQL Developer uygulamalarından birini kullanabilirsiz.

Temel SQL eğitimi notlarını hazırlarken, platformdan bağımsız olarak SQL öğretilmesi amaçlanmıştır. Sadece Oracle platformuna özel yapılardan, mümkün mertebe daha sonraki konularda bahsedilecektir. 

Temel SQL eğitimi içinde oluşabilecek farklı platform yapıları için aşağıdaki ayıraçları takip edebilirsiniz.

**\(\*\)** yıldız komutun sadece **ORACLE** üzerinde kullanıldığını belirtirken

**\(+\)** artı işareti **MS SQL** üzerinde kullanılan şeklini belirtir.

## 

