# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu
Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar 
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]


##### Görevler
Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın. 


MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
	#ilk 3 soruyu join kullanmadan yazın.
	
	1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
	=> select o.ograd, o.ogrsoyad, i.atarih from islem i, ogrenci o

	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	=> SELECT k.kitapadi, t.turadi from kitap k, tur t 
		where t.turadi = 'Fıkra' or t.turadi = 'Hikaye'
	
	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	
	=> SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi from ogrenci o, kitap k
		where o.sinif = '10b' or o.sinif = '10c'
	
	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
	=> SELECT o.ograd, o.ogrsoyad, i.atarih from islem i
		JOIN ogrenci o on o.ogrno = i.ogrno
	
	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	=> SELECT k.kitapadi, t.turadi from kitap k
		JOIN tur t on t.turno = k.turno
	
	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.
	
	=> SELECT o.ogrno, o.ograd, o.ogrsoyad, k.kitapadi from islem i
		JOIN ogrenci o on o.ogrno = i.ogrno
		JOIN kitap k on k.kitapno = i.kitapno
		WHERE o.sinif = '10b' or o.sinif = '10c'
		order by o.ograd
	
	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	=> SELECT o.ograd, o.ogrsoyad, k.kitapadi from islem i
		LEFT JOIN ogrenci o on o.ogrno = i.ogrno
		LEFT JOIN kitap k on k.kitapno = i.kitapno
	
	8) Kitap almayan öğrencileri listeleyin.
	
	=> SELECT o.ogrno, o.ograd, o.ogrsoyad from islem i
		right JOIN ogrenci o on o.ogrno = i.ogrno
		WHERE i.ogrno is null
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
	=> SELECT i.kitapno, k.kitapadi, count(i.kitapno) AS 'sayac'  from islem i
		JOIN kitap k on k.kitapno = i.kitapno
		group by i.kitapno
		order by i.kitapno asc
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

	=> SELECT i.kitapno, k.kitapadi, 
		case 
		when count(i.kitapno) > 0 then count(i.kitapno)
		when count(i.kitapno) = 0 then 0 
		END AS 'sayac'  from islem i
		RIGHT JOIN kitap k on k.kitapno = i.kitapno
		group by i.kitapno
		order by i.kitapno asc;

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
	=> SELECT o.ograd, o.ogrsoyad, k.kitapadi from islem i
		JOIN kitap k on k.kitapno = i.kitapno
		JOIN ogrenci o on o.ogrno = i.ogrno;
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.
	
	=> SELECT o.ograd, o.ogrsoyad, k.kitapadi, y.yazarad, y.yazarsoyad,t.turadi, i.atarih from islem i
		LEFT JOIN kitap k on k.kitapno = i.kitapno
		LEFT JOIN ogrenci o on o.ogrno = i.ogrno
		LEFT JOIN yazar y on y.yazarno= k.yazarno
		LEFT JOIN tur t on t.turno = k.turno;
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.
	
	=> SELECT o.ograd, o.ogrsoyad, count(i.kitapno) as 'sayi' from islem i
		right JOIN ogrenci o on o.ogrno = i.ogrno
		WHERE o.sinif = '10a' or o.sinif = '10b'
        group by i.kitapno
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG
	
	=> SELECT AVG(sayfasayisi) as ortalama from kitap;
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.
	
	=> SELECT * from kitap HAVING sayfasayisi > (select AVG(sayfasayisi) from kitap)
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
	=> SELECT count(*) FROM ogrenci
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
	=> SELECT count(*) as 'Toplam Sayı' FROM ogrenci
	
	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
	=> SELECT count(ograd) FROM ogrenci
		group by ograd
		HAVING count(ograd) > 1;
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	=> SELECT sayfasayisi from kitap
		order by sayfasayisi desc limit 1
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	=> SELECT kitapadi, sayfasayisi from kitap
		order by sayfasayisi desc limit 1;
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	=> SELECT sayfasayisi from kitap
		order by sayfasayisi asc limit 1
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.
	
	=> SELECT kitapadi, sayfasayisi from kitap
		order by sayfasayisi asc limit 1;
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
	=> SELECT k.sayfasayisi from kitap k
		JOIN tur t on t.turno = k.turno
		where t.turadi = 'dram'
		order by k.sayfasayisi desc limit 1;
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.
	
	=> select sum(k.sayfasayisi) from islem i 
		JOIN kitap k on k.kitapno = i.kitapno
		where i.ogrno = 15;
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	=>	SELECT ograd, count(ograd) as 'Adet' FROM ogrenci
		group by ograd
	
	26) Her sınıftaki öğrenci sayısını bulunuz.
	
	=> SELECT sinif, count(sinif) as 'Adet' FROM ogrenci
		group by sinif
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.
	
	=> SELECT sinif, count(cinsiyet) as 'Adet' FROM ogrenci group by sinif, cinsiyet;
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.
	
	=> SELECT o.ograd, o.ogrsoyad, sum(k.sayfasayisi) 'sayfa' FROM islem i
		JOIN ogrenci o on o.ogrno = i.ogrno
		JOIN kitap k on k.kitapno = i.kitapno
		group by o.ograd, o.ogrsoyad
		order by sayfa desc
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

	=> SELECT o.ograd, o.ogrsoyad, sum(k.sayfasayisi) 'sayfa' FROM islem i
		JOIN ogrenci o on o.ogrno = i.ogrno
		JOIN kitap k on k.kitapno = i.kitapno
		group by o.ograd, o.ogrsoyad;