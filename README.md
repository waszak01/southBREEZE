southBREEZE
===========

Projekt "Relacyjne bazy danych"

Autorzy: Sławomir Maderak (dokumentacja), Tomasz Waszak (kodowanie)


INSTALACJA
==========

Można również zainstalować poszczególne komponenty osobno, jednak wymaga to od użytkownika względnie szerokiej wiedzy i doświadczenia, co może znacznie wydłużyć czas uruchomienia, a czasem nawet wymagać pomocy innego bardziej doświadczonego użytkownika. Jeśli zaczynamy dopiero naszą przygodę z relacyjnymi bazami danych, najszybciej skorzystać z narzędzi w postaci pakietu XAMP (http://www.apachefriends.org/index.html) dostępnego dla współczesnych popularnych systemów operacyjnych. Pod wspomnianym wyżej adresem znajdziemy pakiety zawierające wiele różnych narzędzi, wśród których interesują nas przede wszystkim:

* serwer WWW - Apache (www.apache.org) wraz z modułem PHP (http://php.net/)
* system zarządzania relacyjną bazą danych - MySQL (www.mysql.com) albo jego społecznościowy zamiennik MariaDB (mariadb.org)
* graficzny interfejs obsługi bazy danych - phpMyAdmin (www.phpmyadmin.net)

Warto wspomnieć, że w ramach tego pakietu mamy także możliwość zainstalowania dodatkowych narzędzi takich jak:

* serwer plików FTP - FileZilla FTP Server (https://filezilla-project.org/)
* interpreter języka Perl wraz z modułami dla serwera www Apache (www.perl.org)
* serwer poczty elektronicznej - Mercury Mail Transport System (http://www.pmail.com/)
* bezpieczna komunikacja przez sieć - OpenSSL (www.openssl.org)
* kontener aplikacji webowych - Tomcat (http://tomcat.apache.org/)
 
Jednak w naszym projekcie nie będą one wykorzystywane.


URUCHOMIENIE
============

Bazę danych odpowiadającą za warstwę persystencji naszej aplikacji zainicjowaliśmy używając narzędzia phpMyAdmin (www.phpmyadmin.net). Narzędzie to umożliwia proste i szybkie wyklikanie nawet bardzo złożonych czynności administracyjnych, które w typowym scenariuszu należałoby wykonać z konsoli tekstowej używając języka SQL.


BUDOWA I DZIAŁANIE
==================

Funkcjonujący system dostępny jest pod adresem http://btw.com.pl/agh/ . Warstwa prezentacji oraz logika aplikacji została zakodowana w języku PHP ze wstawkami HTML.
Można z tego poziomu przetestować funkcjonalność serwisu (http://btw.com.pl/agh/edit_klient.php):

* przeglądanie rekordów przykładowej tabeli (klienci),
* dodawanie nowego rekordu zawierającego wprowadzone dane,
* usunięcie rekordu z tabeli,

Można także przetestować działanie relacji wiele-do-jednego wyświetlając liczbę zamówień danego klienta (http://btw.com.pl/agh/list.php). Można także wyświetlić wszystkie zamówienia danego klienta.
