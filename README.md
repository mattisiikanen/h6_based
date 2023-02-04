# h6_based


## Lukuläksy
Lukuläksynä tuli lukea ja tiivistää 2 Apacheen liittyvää dokumenttia: Apache Software Foundation 2023: Getting Started (https://httpd.apache.org/docs/2.4/getting-started.html) ja Apache Software Foundation 2023: Name-based Virtual Host Support (https://httpd.apache.org/docs/current/vhosts/name-based.html)

Apache Software Foundation 2023: Getting Started
Dokumentin tarkoituksena on saattaa lukija alkuun Apachen HTTP palvelimen konfiguroinnissa.
- Clients, Servers and URLs
	- käydään läpi näiden toimintaa keskenään
	- Client pyytää
	- URL - toimii välikätenä
	- Server vastaa ja lähettää vastauksen
- Hostnames and DNS
	- DNS kääntää palvelimen nimen IP-osoitteeksi
	- Yksi tai useampi hostname voi kohdistua samaan IP-osoitteeseen
	- Useampia web-sivuja voidaan ajaa samalla koneella käyttämällä virtual hosteja
	- Omaa sivua testaillessa voi muokkaamalla hosts tiedostoa
- Configuration Files and Directives
	- Configuration tiedostoilla voidaan kontrolloida Apachen sivuja 
	- Tiedostot saa järjestää haluamansa mukaan ja onkin suositeltavaa tehdä niin, mikäli siinä on järkeä omasta mielestä
	- Directivet määrittelevät tiedon ja asetukset configuration tiedostoissa
- Web Site Content
	- Voi olla monen muotoista, mutta yleisimmillään voidaan jakaa staattiseen ja dynaamiseen sisältöön
	- Staattinen sisältö on valmiiksi luotua, kuten tekstiä tai kuvia
	- Dynaaminen sisältö generoituu vierailijan käyttäessä sivustoa, esim. jotain muuttuvia numeroita tms.
- Log Files and Troubleshooting
	- Toimii kehittäjän parhaimpana työkaluna vianselvityksessä
	- Oletus sijainti voidaan määrittää

Apache Software Foundation 2023: Name-based Virtual Host Support
Dokumentissa käsitellään missä ja miten käytetään name-based virtual hosteja.
- Name-based vs. IP-based Virtual Hosts
	- IP-based Virtual Host ohjaa liikenteen haluttuun virtuaalihostiin IP-osoitteen perusteella, jokaista hostia varten tarvitaan oma erillinen IP-osoite
	- Name-based Virtual Host ohjaa liikenteen haluttuun virtuaalihostiin käyttämällä hostnamea HTTP headereissa, tällä tavalla useammalla hostilla voi olla sama IP-osoite
	- IP-based on historian jäänne ja sitä käytetään vielä joissain erityistapauksissa
	- Name-based on yleisesti käytössä ja sitä suositaan
- How the server selects the proper name-based virtual host
	- Ensimmäinen askel on IP-based resoluutio
	- Tunnistus menee kaikkein sopivimman kandidaatin mukaan
	- Jos yhteensopivinta ei löydy, niin palvelin ohjaa liikenteen ensimmäiselle virtual hostille listalla
- Using Name-based Virtual Hosts
	- Tarkoituksena on "maskeerata" itse palvelin pois
	- Koostuu useammasta osiosta "VirtualHost", "ServerAlias", "ServerName" & "DocumentRoot"
	- Nimi periytyy oletus konfiguraatiosta, mikäli sitä ei määritellä erikseen VirtualHostissa
	- ServerAliaksella voidaan konfiguroida palvelimelle useampi nimi samanaikaisesti käyttöön
	

## Ympäristö

Hyper-V kotikoneella (Host):

- CPU: i5-9600K
- RAM: 16Gb
- HDD: 120Gb
- Win 11 Pro x64

Virtuaalikoneen speksit:

- 2 x CPU
- 4 Gb RAM
- 50 Gb HDD
- Generation 2 (Hyper-V pyytää määrittelemään)

## Aloitus 
Tehtävän tarkoituksena oli jatkaa tunnilla käytyyn Apacheen liittyviä tehtäviä.
Aloitin tehtävät 4.2.2023 klo 11:00.


## a) Vaihda Apachelle uusi etusivu
Kirjauduin ensimmäiseksi virtuaalikoneelleni ja avasin terminalin. Lähdin työstämään tehtävää käyttämällä komentoja, jotka olivat käytössä tunnilla.
Ensimmäiseksi kirjoitin komentokehotteeseen pätkän ```sudoedit /etc/apache2/sites-available/frontpage.conf```, komento avasi editorin, johon syötin kuvassa näkyvät määritteet:</br>
![Kuva 1](https://user-images.githubusercontent.com/122887740/216758755-d0717008-b563-4260-a13c-1cfdc2300415.png)</br>
Seuraavana oli luvassa uuden etusivun konfigurointi oletussivuksi. Operaatio aloitettiin poistamalla ensin oletussivu pois käytöstä komennolla ```sudo a2dissite 000-default.conf´´´ ja syöttämällä uuden sivun käyttämällä komentoa ```sudo a2ensite frontpage.conf```, sekä Apachen palvelun uudelleenkäynti, joka tulee tehdä aina muutoksien jälkeen. Komento: ```systemctl restart apache2```
![Kuva 2](https://user-images.githubusercontent.com/122887740/216759054-e1330500-ff80-4403-8813-6875a27cc35e.png)</br>
![Kuva 3](https://user-images.githubusercontent.com/122887740/216759057-3b32836f-f2fa-48ca-9995-8f8b2a1574d2.png)</br>
Roottina tehtyjen muutosten jälkeen oli tarkoitus siirtyä käyttäjän kansioon tekemään uudet sivut. Onnekseni olinkin jo valmiiksi oman käyttäjäni juurikansiossa, joten aloitin sivujen luonnin ensiksi luomalla uuden kansion nimeltä public_html komennolla ```mkdir public_html```, navigoin sinne komennolla ```cd public_sites``` ja viimeisenä aloitin uuden sivun luonnin komennolla ```micro index.html```. Tekstieditorissa syötin seuraavan tekstin: </br>
![Kuva 4](https://user-images.githubusercontent.com/122887740/216759196-d20c6c8d-6cdc-439d-b652-1fac20fa17ba.png)</br>
Nyt oli vuorossa testaus ja siirryinkin avaamaan Firefoxin ja navigoimaan osoitteeseen ```localhost```: </br>
![Kuva 5](https://user-images.githubusercontent.com/122887740/216760036-ca4afb12-c516-4af0-8a04-2928c186487b.png)</br>
Sain jostain syystä vanhan sivuston. Lähdin tutkimaan tilannetta ja ajoin komennon ```sudo apachectl -S```, tällä näin nykyiset asetukset: </br>
![Kuva 6](https://user-images.githubusercontent.com/122887740/216760135-c68145ff-2699-4a84-917f-7783c52dda49.png)</br>
Siellähän oli vanha asetus vielä päällä, joten päätin aloittaa oletussivun asetusprosessin alusta, varmistuakseni tilanteesta:</br>
![Kuva 7](https://user-images.githubusercontent.com/122887740/216760235-92459afe-8cff-474a-8832-a42d0492ae90.png)</br>
Jostain syystä olin unohtanut näköjään sitten aktivoida frontpage.conf asetuksen oletuksena. Oli uuden testin aika: </br>
![Kuva 8](https://user-images.githubusercontent.com/122887740/216760289-34917a7a-4db2-4bb4-a7c2-3434ed7cfbf6.png)</br>
Nyt se toimi oikein!


## b) Tee Apachen asetustiedostoon kirjoitusvirhe
Klo 11:40</br>
Navigoin osoitteeseen ```/etc/apache2/sites-available/```, jossa käsittelin frontpage.conf tiedostoa komennolla ```micro frontpage.conf``` ja lisäsin tahallisesti yhden ylimääräisen kirjaimen <Directory> direktiiviin: </br>
![Kuva 9](https://user-images.githubusercontent.com/122887740/216760477-cd7fe87e-7eaa-49b6-9a33-59a17ffc5037.png)</br>
Tallensin Sudona tiedoston ja käynnistin Apache palvelun uudelleen komennolla ```systemctl restart apache2```. Tämän jälkeen siirryin testaamaan toimintaa ja sain seuraavan tuloksen: </br>
![Kuva 10](https://user-images.githubusercontent.com/122887740/216760534-05a39c73-6a04-41ae-b7a2-c1c9b0e086b8.png) </br>
Ongelma on selvästi generoitu. Seuraavaksi oli vuorossa käydä tutkimassa ongelmaa lokeista sekä ```apache2ctl configtest``` komentoa käyttämällä. </br>
![Kuva 11](https://user-images.githubusercontent.com/122887740/216760620-1586ab69-5d4b-4408-a1f0-8fde59e1af6d.png) </br>
Kuvasta jo näkee selvästi tekemäni virheen. Komennon jälkeen siirryin tutkimaan lokeja menemällä lokikansioon komennolla ```cd /var/log/apache2/```, komento törmäsi oikeusongelmaan. Oli aika korottaa paukkuja komennolla ```sudo su```, korotuksen jälkeen kansioon pääsi heittämällä. </br>
![Kuva 12](https://user-images.githubusercontent.com/122887740/216760752-e131eb82-fab0-4f6d-a2a0-cfdb09b84497.png) </br>
Nyt oli tarkoitus tutkia error.log-tiedostoa ja sitä tutkin komennolla ```cat error.log```, tulokset olivat seuraavanlaiset: </br>
![Kuva 13](https://user-images.githubusercontent.com/122887740/216760846-1c621ed3-17ac-435e-b7dd-f111629c101e.png) </br>
Tässä huomataan, että virheen ilmaantuessa Apachen palvelu on mennyt sammuksiin. Error.log antaa palveluun liittyvää operatiivista tietoa, kun taas aiemmin käyttämä komento ottaa selvästi kantaa itse konfiguraatiotiedoston ongelmiin. Lopulta päätin vielä palauttaa tilanteen normaaliksi poistamalla ylimääräisen kirjaimen oletuskonfiguraatio tiedostosta.

## n) Vapaaehtoinen: Tee Apachelle kaksi nimipohjaista palvelua
Klo 12:00 </br>
Aloitin tehtävän etsimällä ohjeet Internetin syövereistä ja löysin Karim Buzdarin kirjoittaman artikkelin aiheesta: How to Edit the Hosts File on Debian. Ohjeita seuraamalla käynnistämällä root oikeudet komennolla ```sudo su``` ja navigoimalla osoitteseen: ```/etc/``` ja avaamalla hosts tiedoston komennolla: ```micro hosts```. Lisäsin localhostiin (127.0.0.1) sekä foo.example.com ja bar.example.com omille riveilleen: </br>
![Kuva 14](https://user-images.githubusercontent.com/122887740/216761241-60a840e6-ceb2-4195-b70e-0df20a396ab9.png)</br>
Tallensin tiedoston ja siirryin testaamaan sivuja: </br>
![Kuva 15](https://user-images.githubusercontent.com/122887740/216761249-42011b9b-f1a6-4e57-bb33-235f07d40171.png) </br>
![Kuva 16](https://user-images.githubusercontent.com/122887740/216761253-acc347d3-7a5c-4e27-b559-8ee3f5a7af42.png) </br>
Testaus oli onnistunut. Tätä tehtäessä tajusin, että hosts tiedostolla voidaan periaatteessa estää halutuille sivustoille pääsy ohjaamalla liikenne suoraan haluttuun osoitteeseen.


## Lopetus
Lopetin tehtävien teon klo 12:08. Kyseinen tehtävä avasi lisää Apachen web-palvelimen käyttäytymistä ja sen konfigurointia. Hommiin meni tällä erää n. 2h.

## Lähteet:
- Karvinen Tero: Linux Palvelimet 2023 alkukevät (https://terokarvinen.com/2023/linux-palvelimet-2023-alkukevat/)
- Apache Software Foundation 2023: Getting Started (https://httpd.apache.org/docs/2.4/getting-started.html)
- Apache Software Foundation 2023: Name-based Virtual Host Support (https://httpd.apache.org/docs/current/vhosts/name-based.html)
- Buzdar, Karim: How to Edit the Hosts File on Debian (https://vitux.com/hosts_file_debian/)
