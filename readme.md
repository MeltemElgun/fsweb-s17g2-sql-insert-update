# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.

    cevap---insert into yazar(yazarad,yazarsoyad) values('KEMAL' , 'UYUMAZ')

    2) Biyografi türünü tür tablosuna ekleyiniz.

     cevap---insert into tur(turadi) values("Biyografi")


    3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.

     cevap---insert into ogrenci(sinif,ograd,ogrsoyad,cinsiyet)
    values("10A","ÇAĞLAR", ÜZÜMCÜ","E"),("9B","LEYLA", "ALAGÖZ","K"),("11C","Ayşe","Bektaş","K"),


    4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz. rand()

    select * from yazar y
    inner join kitap k on k.yazarno=y.yazarno
    inner join islem i on i.kitapno=k.kitapno
    inner join ogrenci o on o.ogrno=i.ogrno


    cevap--- insert into yazar(yazarad,yazarsoyad) select ograd,ogrsoyad from ogrenci where rand() limit 1;
    select @@IDENTITY //hangi ıd değerini girdiğimizi gösterecek


    5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.

     cevap---insert into yazar (yazarad,yazarsoyad) select ograd,ogrsoyad from ogrenci where ogrno>10 and ogrno<30;
    select @@IDENTITY


    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)

     cevap--- Insert into yazar (yazarad,yazarsoyad) values ("Nurettin","Bellek")
     Select * yazar where
    yazarno=@@IDENTITY


    7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.

     cevap--- Select * from ogrenci
               Where sinif="10B" and ogrno=3

               Update ogrenci set sinif="10C" where sinif="10B" and ogrno=3

    8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın

     cevap--- select * from ogrenci
    update ogrenci set sinif='10A' where sinif='9A'

     derssorucevap--- SELECT * from ogrenci WHERE sinif='10A'
    update ogrenci set ogrsoyad= CONCAT(TRIM(ogrsoyad),'(',cinsiyet ,')' ) WHERE sinif='10A'order by dtarih DESC LIMIT 5

    9) Tüm öğrencilerin puanını 5 puan arttırın.

     cevap--- update ogrenci set puan=puan+5

    10) 25 numaralı yazarı silin.

      cevap--- delete yazar where yazarno=25

    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)

      cevap--- Select * from ogrenci
               Where dtarih is Null

    12) Doğum tarihi null olan öğrencileri silin.

      cevap---  Delete from ogrenci Where dtarih is Null

    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.

      cevap--- Update kitap set puan=puan+2
               Where kitapadi Like "a%"

    14) Kişisel Gelişim isimli bir tür oluşturun.

      cevap--- Insert into tur (turadi) values ("Gelişim")

    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.

     cevap--- Update kitap set turno= (Insert into tur (turadi) values ("Gelişim") Where turno=@@IDENTITY)
     Where kitapadi="Başarı Rehberi"

    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
     cevap--- Create Procedure ogrencilistesi
     as
     Select * from ogrenci
     Go;
     Exec ogrencilistesi

    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
     cevap---
     Create Procedure ekle @ograd nvarchar(30),@ogrsoyad nvarchar(30),@cinsiyet nvarchar(30),@dtarih nvarchar(30),@sinif nvarchar(30),@puan nvarchar(30)
     as
      insert into ogrenci(ograd,ogrsoyad,cinsiyet,dtarih,sinif,puan) values(@ograd,@ogrsoyad,@cinsiyet,@dtarih,@sinif,@puan)
     Go;
     Exec ekle;

     CEVAP2:
      Create Procedure ekle (
        IN name Varchar(50),
        IN surname VARCHAR(50)
        )
      BEGIN
        insert into ogrenci(ograd,ogrsoyad) values(name,surname);
     END;
     CALL ekle('meltem','elgun') // mysql'de kullanımı
      select @@IDENTITY  //hangi primary key eklediğini görüyoruz

     select * FROM  ogrenci

    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.

       cevap--- Create procedure sil @id numeric(10) as delete from ogrenci
               Where ogrno=@id;
               Exec sil 20

    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
        cevap--- Create Procedure changeclass @ogrno numeric(10),@sinif nvarchar(30) as update ogrenci set sinif=@sinif where ogrno=@ogrno Go;
               Exec changeclass 10,"10B"

    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.

       cevap---  Create Procedure search as Select concat(ograd,ogrsoyad) as AdSoyad from ogrenci;

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.

     cevap---  Drop Procedure [ogrencilistesi,ekle,sil,changeclass,search];

    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.


    23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.


    24) Kitap okumayan öğrencileri listeleyiniz.


    25) Okunmayan kitapları listeleyiniz


    26) Mayıs ayında okunmayan kitapları listeleyiniz.

derssorucevap--- update ogrenci set puan=puan+15 ,ograd=CONCAT(ograd,'(**\***)')
where ogrno IN (Select o.ogrno from ogrenci as o
Inner Join islem as i On i.ogrno=o.ogrno
Inner Join kitap as k On k.kitapno = i.kitapno
group by o.ogrno
order by Sum(k.sayfasayisi) DESC
Limit 5)
