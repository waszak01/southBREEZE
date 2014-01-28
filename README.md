southBREEZE
===========

Projekt "Relacyjne bazy danych"

Autorzy: Sławomir Maderak (dokumentacja), Tomasz Waszak (kodowanie)


INSTALACJA
==========

Można zainstalować poszczególne komponenty osobno, jednak wymaga to od użytkownika względnie szerokiej wiedzy i doświadczenia, co może znacznie wydłużyć czas uruchomienia, a czasem nawet wymagać pomocy innego bardziej doświadczonego użytkownika. Jeśli zaczynamy dopiero naszą przygodę z relacyjnymi bazami danych, najszybciej skorzystać z narzędzi w postaci pakietu XAMP (http://www.apachefriends.org/index.html) dostępnego dla współczesnych popularnych systemów operacyjnych. Pod wspomnianym wyżej adresem znajdziemy pakiety zawierające wiele różnych narzędzi, wśród których interesują nas przede wszystkim:

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

Bazę danych odpowiadającą za warstwę persystencji naszej aplikacji zainicjowaliśmy używając narzędzia phpMyAdmin (www.phpmyadmin.net). Narzędzie to umożliwia proste i szybkie wyklikanie nawet bardzo złożonych czynności administracyjnych na bazie danych, które w typowym scenariuszu należałoby wykonać z konsoli tekstowej używając poleceń języka SQL.


BUDOWA I DZIAŁANIE
==================

Funkcjonujący system dostępny jest pod adresem http://btw.com.pl/agh/ . Warstwa prezentacji oraz logika aplikacji została zakodowana w języku PHP ze wstawkami HTML.
Można z tego poziomu przetestować funkcjonalność serwisu (http://btw.com.pl/agh/edit_klient.php):

* przeglądanie rekordów przykładowej tabeli (klienci),
* dodawanie nowego rekordu zawierającego wprowadzone dane,
* usunięcie rekordu z tabeli,

Można także przetestować działanie relacji wiele-do-jednego wyświetlając liczbę zamówień danego klienta (http://btw.com.pl/agh/list.php). Można także wyświetlić wszystkie zamówienia danego klienta.


KOD ŹRÓDŁOWY
=========================

Podłączenie do bazy danych realizowane jest poleceniem:

    $link = mysql_connect($_SESSION['sqlhost'], $_SESSION['sqllogin'], $_SESSION['sqlpassword'])
                or die("Nie można nawiązać połączenia z bazą");
                mysql_select_db($_SESSION['sqldatabase'])
                or die("Wystąpił błąd podczas wybierania bazy danych");
                
Odpowiednie parametry są pobrane wcześniej do zmiennych sesyjnych. Następnie po ustawieniu mniej istotnych zmiennych dotyczących prezentacji danych..

    $count=8;
    $offset=0;
    if(isset($_GET['count']))$count = $_GET['count'];
    if(isset($_GET['offset']))$offset = $count*$_GET['offset'];

pobieramy liczbę rekordów:

    $sql = 'Select count(*) from Customers';
    $result = mysql_query($sql,$link);
    $r = mysql_fetch_array($result);

pobieramy paczkę danych:

    $sql = 'SELECT * FROM Customers ORDER BY CompanyName ASC Limit '.$count.' offset '.$offset.';';
    $result = mysql_query($sql,$link);

którą następnie prezentujemy w tabeli:


    echo '<table><tr>
    <th>ID</th>
    <th>Nazwa</th><th>Kontakt</th>
    <th>Tytuł</th>
    <th>Adres</th>
    <th>Miasto</th>
    <th>Kod</th>
    <th>Kraj</th>
    <th>Telefon</th>
    <th>Fax</th>
    </tr>';


W pętli wyświetlamy wszystkie rekordy:


    while(($row=mysql_fetch_array($result))!=NULL){
    echo   '<tr><td>'      .$row['CustomerID'].
                '</td><td>'     .$row['CompanyName'].
                '</td><td>'     .$row['ContactName'].
                '</td><td>'     .$row['ContactTitle'].
                '</td><td>'     .$row['Address'].
                '</td><td>'     .$row['City'].
                '</td><td>'     .$row['PostalCode'].
                '</td><td>'     .$row['Country'].
                '</td><td>'     .$row['Phone'].
                '</td><td>'     .$row['Fax'].
                '</td></tr>';
    }
    echo '</table>';
 
 
Aby dodać nowy rekord, tworzymy najpierw formularz, do którego operator albo użytkownik systemu będzie wpisywał dane dodawanego klienta.

    echo "<form action='edit_klient.php' method=post>";
        echo "<br> Dodaj nowego klienta<br>";
                echo "<table><tr><td>ID :</td><td><input type=text name=id></td></tr>";
        echo "<tr><td>Nazwa :</td><td><input type=text name=nazwa>";
        echo "<tr><td>Kontakt :</td><td><input type=text name=kontakt>";
        echo "<tr><td>Tytuł :</td><td><input type=text name=tytul></td></tr>";
        echo "<tr><td>Adres :</td><td><input type=text name=adres></td></tr>";
                echo "<tr><td>Miasto :</td><td><input type=text name=miasto></td></tr>";
                echo "<tr><td>Kod :</td><td><input type=text name=kod></td></tr>";
                echo "<tr><td>Kraj :</td><td><input type=text name=kraj></td></tr>";
                echo "<tr><td>Telefon :</td><td><input type=text name=telefon></td></tr>";
                echo "<tr><td>Fax :</td><td><input type=text name=fax></td></tr>";

        echo "<input type=hidden value='1' name=send>";
        echo "<input type=submit value='Dodaj klienta'>";
        echo "</table></form>";
        
        
Następnie sprawdzamy dane i dodajemy nowy rekord do bazy:

    if($_POST["send"]==1)
        {
        if(!empty($_POST["id"]) && !empty($_POST["nazwa"]) )
                {
                $link_delikwenta="SELECT * FROM Customers WHERE CustomerID='".$_POST['id']."'";
                $zapytanie = mysql_query($link_delikwenta);
                $istnieje= mysql_num_rows($zapytanie);

                if ($istnieje > 0)
                        {
                        echoAlert("W bazie danych istnieje już klient o takim ID.");
                        }
                else
                        {
            $kwer="insert into Customers values(
                        '".htmlspecialchars($_POST["id"])."',
                        '".htmlspecialchars($_POST["nazwa"])."',
                        '".htmlspecialchars($_POST["kontakt"])."',
                        '".htmlspecialchars($_POST["tytul"])."',
                        '".htmlspecialchars($_POST["adres"])."',
                        '".htmlspecialchars($_POST["miasto"])."',
                        NULL,
                        '".htmlspecialchars($_POST["kod"])."',
                        '".htmlspecialchars($_POST["kraj"])."',
                        '".htmlspecialchars($_POST["telefon"])."',
                        '".htmlspecialchars($_POST["fax"])."'
                        )";
            mysql_query($kwer);
            echoAlert('Dodano klienta.');
                        }
                }
                else echoAlert("Musisz podać przynajmniej ID i nazwę.");
        }
        

Usunięcie rekordu realizuje kod:

    $del_qry="DELETE FROM Customers WHERE CustomerId='" . $_GET['id']."'";
    mysql_query($del_qry);
    echo "Usunąłem klienta o ID ".$_GET['id'].".<br><br>";
    
