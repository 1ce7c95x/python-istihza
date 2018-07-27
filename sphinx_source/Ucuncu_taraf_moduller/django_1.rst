.. meta::
   :description: Bu b�l�mde django ile site yapmaya ba�layaca��z.
   :keywords: python, django, �eviri
   
.. highlight:: python3

*****************************************
�LK DJANGO PROJEN� YAZ, part 1
*****************************************

**Kaynak Kodu:** https://docs.djangoproject.com/en/2.0/intro/tutorial01/

Bir �rnekle ��renmeye ba�layal�m. Bu �rnekte basit bir anket uygulamas� olu�turaca��z.
Uygulama iki k�s�mdan olu�acak:

	#. Anketlerin oylanmas� i�in herkese a��k bir site 
	#. Anketleri d�zenlemek veya ekleyip silmek i�in bir admin paneli

Senin Djangoyu y�kledi�ini varsay�yoruz. Komut isteminde a�a��daki komutu �al��t�rarak Djangonun y�kl� olup olmad���na ve Django s�r�m�ne ula�abilirsin::

	python -m django --version

E�er Django y�kl�yse y�kl� olan versiyonu g�rmelisin. E�er de�ilse "No module named django" yaz�s� ile kar��la�mal�s�n.

Bir proje olu�tur
==================

Komut sat�r�nda cd komutuyla komutunuzu saklamak istedi�iniz dizine gidin ve a�a��daki kodu �al��t�r�n::

	django-admin startproject mysite

Bu kod bulundu�unuz dizinde mysite dizinini yaratacak.
�imdi `startproject` komutunun olu�turduklar�na bakal�m::

	mysite/
	    manage.py
	    mysite/
	        __init__.py
	        settings.py
	        urls.py
	        wsgi.py

En d��ardaki `mysite/` dizini, projeniz i�in sadece bir kapsay�c�d�r. Ad� Django i�in �nemli de�il. Be�endi�iniz herhangi bir �eye yeniden adland�rabilirsiniz.
`manage.py`: Komut sat�r�ndan django projesiyle etkile�ime ge�menizi sa�layan bir programd�r.
��erideki `mysite/` dizini , projeniz i�in ger�ek bir python paketidir.
`mysite/init.py`: Bo� bir python dosyas�d�r. mysite/ dizininin python paketi olmas�n� sa�lar.
`mysite/setting.py`: Django projesinin ayarlar� ile ilgili bir dosya.
`mysite/urls.py`: Projeniz i�in URL'leri bar�nd�ran dosya.
`mysite/wsgi.py`: WSGI uyumlu web sunucular� i�in projenize hizmet edecek bir giri� noktas�.

Geli�tirme sunucusu
====================

�imdi django projemizin �al���p �al��mad���n� kontrol edelim. Komut sat�r�nda d��ar�daki `mysite` dizinine gidin ve a�a��daki kodu �al��t�r�n::

	python manage.py runserver

��kt� olarak �unu g�rmelisin::

	Performing system checks...

	System check identified no issues (0 silenced).

	You have unapplied migrations; your app may not work properly until they are applied.
	Run 'python manage.py migrate' to apply them.

	April 29, 2018 - 15:50:53
	Django version 2.0, using settings 'mysite.settings'
	Starting development server at http://127.0.0.1:8000/
	Quit the server with CONTROL-C.

.. Note:: Veritaban�yla ilgili uyar�y� dikkate almay�n.

Django geli�tirme sunucusunu ba�latt�n�z.

Kullan�lan portu de�i�tirme
============================

`runserver` komutu geli�tirme sunucusu i�in standart olarak 8000 portunu kullan�r. 
E�er bu portu de�i�tirmek isterseniz bunu komuta arg�man olarak verin. Mesela a�a��daki komut 8080 portunda geli�tirme sunucusunu �al��t�r�yor::

	python manage.py  runserver 8080

E�er sunucunun IP adresini de�i�tirmek isterseniz port ile birlikte belirtin. �rnek olarak kullan�labilir t�m IP'leri dinlemek istiyorsan�z �u kodu �al��t�r�n::

	python manage.py runserver 0:8000

Yukar�da yazd���m�z kodda 0'�n anlam� 0.0.0.0 (Yani bir k�saltma).

Bir anket uygulamas� olu�tural�m
=================================

Art�k proje ortam�m�z kuruldu. �al��maya ba�layabiliriz.
Django'da yazd���m�z her uygulama bir python paketinden olu�ur ve Django'da uygulaman�n dizini otomatik olarak olu�turulur. Bu sayede dizin olu�turmakla u�ra�aca��m�z zamanda kod yazabiliriz.
Bir uygulama olu�turmak i�in komut sat�r�nda `manage.py` ile ayn� dizine gelin ve �u komutu yaz�n::

	python manage.py startapp polls

`polls` isimli bir dizin olu�turulacak. Bakakl�m i�inde neler var::
	
	polls/
	    __init__.py
	    admin.py
	    apps.py
	    migrations/
	        __init__.py
	    models.py
	    tests.py
	    views.py

Bu dizin anket uygulamam�z�n merkezi olacak.

�lk view'�m�z� yazal�m
=======================

Hadi yazmaya ba�layal�m. �imdi `polls/views.py` a��n ve �u kodlar� yaz�n::

	from django.http import HttpResponse
	def index(request):
	    return HttpResponse("Hello, world. You're at the polls index.")

Bu Django'da yaz�labilecek en basit view. Art�k bu view � �a��rabilmek i�in bir URL haritas�na ihtiyac�m�z var ve URL haritas� i�in de URL �emas�na.
polls dizininde `urls.py` isimli bir dosya olu�turarak uygulaman�n URL �emas�n� da olu�turmu� oluruz.(Dosya Gezgininden kendiniz urls.py isimli bir python mod�l� olu�turun.)  Uygulama dizini son olarak ��yle g�r�nmeli::

	polls/
	    __init__.py
	    admin.py
	    apps.py
	    migrations/
	        __init__.py
	    models.py
	    tests.py
	    views.py
	    urls.py

�imdi de yeni olu�turdu�umuz polls dizinindeki urls.py dosyas�nda �u kodlar yaz�l� olmal�::

	from django.urls import path
	from . import views

	urlpatterns = [
	    path('', views.index, name='index'),
	]

Burada olu�turdu�umuz URL �emas�n� ger�ek Url �emas�nda tan�tman�n vakti geldi. Bunun i�in mysite dizinindeki urls.py dosyas�nda include fonksiyonunu i�e aktar�p  url listesini aktarmada kullanaca��z. Sonu� olarak mysite dizinindeki urls.py dosyan�z �u hale gelmeli::

	from django.contrib import admin
	from django.urls import include, path
	
	urlpatterns = [
	    path('polls/', include('polls.urls')),
	    path('admin/', admin.site.urls),
	]

Art�k index view'�n� bir dizine ba�lad�n�z. Test etmenin vakti geldi. Komut sat�r�nda �u kodu �al��t�r�n::

	python manage.py runserver

`include()` fonksiyonu di�er URL �emalar�na ula�mam�za izin verir. Django include ile kar��la�t���nda e�le�en URL'yi kalan i�lemler i�in verilen URL �emas�na g�nderir.