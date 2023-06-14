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

yazar tablosu: yazarno, yazarad, yazarsoyad
tur tablosu: turno, turadi
kitap tablosu: kitapno, isbnno, kitapadi, yazarno, turno,sayfasayisi, puan
ogrenci tablosu: ogrno, ograd, ogrsoyad, cinsiyet, dtarih, sinif, puan
islem tablosu: islemno, ogrno, kitapno, atarih, vtarih

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
    INSERT INTO yazar (yazarad, yazarsoyad) VALUES ('KEMAL', 'UYUMAZ');


    2) Biyografi türünü tür tablosuna ekleyiniz.
    INSERT INTO tur (turadi) VALUES ('Biyografi');


    3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.
    INSERT INTO ogrenci (ograd, ogrsoyad, cinsiyet, sinif) VALUES ('ÇAĞLAR', 'ÜZÜMCÜ', 'E', '10A'), ('LEYLA', 'ALAGÖZ', 'K', '9B'), ('Ayşe', 'Bektaş', 'K', '11C');


    4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
    INSERT INTO yazar (yazarad, yazarsoyad) SELECT ograd, ogrsoyad FROM ogrenci ORDER BY RAND() LIMIT 1;


    5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
    INSERT INTO yazar (yazarad, yazarsoyad) SELECT ograd, ogrsoyad FROM ogrenci WHERE ogrno BETWEEN 10 AND 30;

    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
    INSERT INTO yazar (yazarad, yazarsoyad) VALUES ('Nurettin', 'Belek');
    SELECT @@IDENTITY;


    7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
    UPDATE ogrenci SET sinif = '10C' WHERE ogrno = 3;

    8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
    UPDATE ogrenci SET sinif = '10A' WHERE sinif = '9A';

    9) Tüm öğrencilerin puanını 5 puan arttırın.
    UPDATE ogrenci SET puan = puan + 5;

    10) 25 numaralı yazarı silin.
    DELETE FROM yazar WHERE yazarno = 25;

    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
    SELECT * FROM ogrenci WHERE dtarih IS NULL;

    12) Doğum tarihi null olan öğrencileri silin.
    DELETE FROM ogrenci WHERE dtarih IS NULL;

    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
    UPDATE kitap SET puan = puan + 2 WHERE kitapadi LIKE 'a%';

    14) Kişisel Gelişim isimli bir tür oluşturun.
    INSERT INTO tur (turadi) VALUES ('Kişisel Gelişim');

    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
    UPDATE kitap SET turno = (SELECT turno from tur where turadi = "Kişisel Gelişim") WHERE kitapadi ="Başarı Rehberi";


    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
    CREATE PROCEDURE `ogrencilistesi`()
    LANG SQL
    AS
    %BODY%
    	SELECT * FROM ogrenci;
    %BODY%


    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
    CREATE PROCEDURE `ekle`(IN ad VARCHAR(50), IN soyad VARCHAR(50), IN cinsiyet CHAR(1), IN sinif VARCHAR(10))
    LANG SQL
    AS
    %BODY%
    	INSERT INTO ogrenci (ograd, ogrsoyad, cinsiyet, sinif) VALUES (ad, soyad, cinsiyet, sinif);
    %BODY%


    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
    CREATE PROCEDURE `sil`(IN ogrno INT)
    LANG SQL
    AS
    %BODY%
    	DELETE FROM ogrenci WHERE ogrno = ogrno;
    %BODY%


    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
    CREATE PROCEDURE `sinifdegistir`(IN ogrno INT, IN sinif VARCHAR(10))
    LANG SQL
    AS
    %BODY%
    	UPDATE ogrenci SET sinif = sinif WHERE ogrno = ogrno;
    %BODY%


    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
    CREATE PROCEDURE `adsoyad`(IN ad VARCHAR(50), IN soyad VARCHAR(50))
    LANG SQL
    AS
    %BODY%
    	SELECT * FROM ogrenci WHERE CONCAT(ograd, ' ', ogrsoyad) LIKE CONCAT('%', ad, ' ', soyad, '%');

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.
    DROP PROCEDURE IF EXISTS `adsoyad`;
    DROP PROCEDURE IF EXISTS `sinifdegistir`;
    DROP PROCEDURE IF EXISTS `sil`;
    DROP PROCEDURE IF EXISTS `ekle`;
    DROP PROCEDURE IF EXISTS `ogrencilistesi`;


    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.

    SELECT * FROM kitap WHERE turno IN (SELECT turno FROM tur WHERE turadi = 'Dram');

    23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
    SELECT * FROM kitap WHERE yazarno IN (SELECT yazarno FROM yazar WHERE yazarad LIKE 'e%');

    24) Kitap okumayan öğrencileri listeleyiniz.
    SELECT * FROM ogrenci WHERE ogrno NOT IN (SELECT ogrno FROM islem);

    25) Okunmayan kitapları listeleyiniz
    SELECT * FROM kitap WHERE kitapno NOT IN (SELECT kitapno FROM islem);

    26) Mayıs ayında okunmayan kitapları listeleyiniz.
    SELECT * FROM kitap WHERE kitapno NOT IN (SELECT kitapno FROM islem WHERE MONTH(atarih) = 5);
