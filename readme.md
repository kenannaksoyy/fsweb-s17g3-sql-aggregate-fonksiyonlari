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
	
	select ograd, ogrsoyad, atarih from ogrenci as o, islem as i 
	where o.ogrno=i.ogrno order by o.ograd;
	
	2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
	
	select kitapadi, turadi from kitap as k, tur as t 
	where k.turno=t.turno and 
	t.turadi in ("Fıkra", "Hikaye");
	//sqllite studioda denedim sonuç gelmedi ergineerde çalıştı
	select kitapadi, turadi from kitap as k, tur as t 
	where k.turno=t.turno and 
	t.turadi="Fıkra" and t.turadi="Hikaye";//aynı olmadı

	3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
	
	select o.ogrno,ograd, ogrsoyad, kitapadi from ogrenci as o, islem as i, kitap as k 
	where (sinif in ("10B", "10C")) and 
	o.ogrno=i.ogrno and 
	k.kitapno=i.kitapno;

	#join ile yazın
	4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
	
	select ograd, ogrsoyad, atarih from ogrenci as o left join islem as i on o.ogrno=i.ogrno;

	5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

	select kitapadi, turadi from kitap as k 
	left join tur as t on k.turno=t.turno 
	where t.turadi in ("Hikaye", "Fıkra");

	6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

	select o.ogrno, ograd, ogrsoyad, sinif, kitapadi from ogrenci as o 
	left join islem as i on o.ogrno=i.ogrno 
	left join kitap as k on i.kitapno=k.kitapno 
	where sinif in ("10B", "10C") order by ograd;

	7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.
	
	select ograd, ogrsoyad, atarih from ogrenci as o 
	left join islem as i on o.ogrno=i.ogrno;
	//inner joinle eşlemeyenler görünmez
	
	8) Kitap almayan öğrencileri listeleyin.

	select ograd, ogrsoyad, atarih from ogrenci as o 
	left join islem as i on o.ogrno=i.ogrno 
	where atarih is null;
	
	9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
	
	select k.kitapno, kitapadi, count(islemno) as "Alış Count" from kitap k 
	inner join islem as i on k.kitapno=i.kitapno 
	group by k.kitapadi order by "Alış Count"; //left join ile 1o. soru cevabı
	
	10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

	//9un left joinli hali

	11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.
	
	select o.ogrno, ograd, ogrsoyad, kitapadi from ogrenci as o
	inner join islem as i on o.ogrno=i.ogrno
	inner join kitap as k on i.kitapno=k.kitapno
	order by ograd;
	
	12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

	select o.ogrno, ograd, ogrsoyad, kitapadi, yazarad, yazarsoyad, t.turadi, i.atarih from ogrenci as o
	left join islem as i on o.ogrno = i.ogrno
	left join kitap as k on i.kitapno = k.kitapno
	left join yazar as y on k.yazarno = y.yazarno
	left join tur as t on k.turno= t.turno
	order by o.ogrno;//her türlü alan almayan

	select o.ogrno, ograd, ogrsoyad, kitapadi, yazarad, yazarsoyad, t.turadi, i.atarih from ogrenci as o
	inner join islem as i on o.ogrno = i.ogrno
	left join kitap as k on i.kitapno = k.kitapno
	left join yazar as y on k.yazarno = y.yazarno
	left join tur as t on k.turno= t.turno
	order by o.ogrno;//sadece kitap alanlar
	
	
	13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

	select ograd, ogrsoyad, count(islemno) "okuduğu kitap sayısı" from ogrenci as o 
	inner join islem as i on o.ogrno=i.ogrno 
	where sinif in("10A","10B") 
	group by o.ogrno 
	order by ograd;

	select ograd, ogrsoyad, count(islemno) "okuduğu kitap sayısı" from ogrenci as o 
	left join islem as i on o.ogrno=i.ogrno 
	where sinif in("10A","10B") 
	group by o.ogrno 
	order by ograd;
	
	
	14) Tüm kitapların ortalama sayfa sayısını bulunuz.
	#AVG

	select avg(sayfasayisi) as "ortalama sayfa sayısı" from kitap; //236.24444444444
	
	15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

	select kitapadi from kitap 
	where sayfasayisi>(select avg(sayfasayisi) from kitap); //22 kitap
	
	16) Öğrenci tablosundaki öğrenci sayısını gösterin
	
	select count(ogrno) as "öğrenci sayısı" from ogrenci;//103
	
	17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.
	
	select count(ogrno) as "toplam sayi" from ogrenci;

	18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.
	
	select count(distinct ograd) as "farklı isimde öğrenci" from ogrenci;//42
	
	19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	select max(sayfasayisi) from kitap;//393
	
	20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

	select kitapadi, sayfasayisi from kitap where sayfasayisi=(select max(sayfasayisi) from kitap);//Danıışmanlık incileri
	
	21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.
	
	select min(sayfasayisi) from kitap;//89
	
	22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

	select kitapadi, sayfasayisi from kitap where sayfasayisi=(select min(sayfasayisi) from kitap);//bir gemide 89
	
	23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.
	
	select max(sayfasayisi) from kitap as k 
	inner join tur as t on k.turno=t.turno 
	where t.turadi="Dram"; //391
	
	24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

	select sum(sayfasayisi) as "Öğrenci Toplam Sayfa Sayısı", o.ogrno from ogrenci as o
	left join islem as i on o.ogrno=i.ogrno
	left join kitap as k on i.kitapno=k.kitapno
	where o.ogrno=15; //1695
	
	
	25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

	select ograd, count(ograd) as adet from ogrenci group by ograd order by adet; //ali 1 ahmet 1

	26) Her sınıftaki öğrenci sayısını bulunuz.
	
	select sinif, count(*) as sayi from ogrenci group by sinif order by sayi desc;
	
	27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

	select sinif, cinsiyet, count(cinsiyet) as sayi from ogrenci group by sinif,cinsiyet order by sinif;
	
	28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

	select ograd, ogrsoyad, sum(sayfasayisi) as "okuduğu toplam sayfa sayısı" from ogrenci as o
	inner join islem as i on o.ogrno=i.ogrno
	left join kitap as k on i.kitapno=k.kitapno
	group by o.ogrno
	order by "okuduğu toplam sayfa sayısı" desc;
	
	29) Her öğrencinin okuduğu kitap sayısını getiriniz.

	select ograd, ogrsoyad, count(kitapadi) as "okuduğu kitap sayısı" from ogrenci as o
	inner join islem as i on o.ogrno=i.ogrno
	left join kitap as k on i.kitapno=k.kitapno
	group by o.ogrno
	order by ograd;