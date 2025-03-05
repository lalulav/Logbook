# Penetration testing report

## 1. Johdanto:

### Reportin tarkoitus ja laajuus.

Penetration Testing Phase 1 testaus, jonka tarkoituksena on tehdä penetraatiotestausta yrityksen kirjautumissivuihin. Tämä ensimmäinen vaihe kattaa kirjautumissivujen testauksen Dockerin ja Zaproxyn avulla.

### Testauksen aukataulu ja testausympäristö.

Testaus tapahtui Virtuaalikone ympäristössä Oracle VirtualBox ohjelmalla, johon asensin Kali Linux käyttöjärjestelmän. Ajankohta testaukseen tapahtui 17-19.2.2025. 

**Päivitetty 5.3.2025 Phase 1 Ver.2**

## 2. Yhteenveto:

-Kolme suurinta kohtaa, joihin pitää heti kiinnittää huomio, jotka ilmeentyivät penetraatiotestauksessa on kriittiset haavoituvuudet joita löytyi kaksi kappaletta Path Traversal, SQL Injection ja Format String Error.

-Path traversaliin voi käyttää sallittujen syötteiden luetteloa, joka on tiukasti vaatimusten mukainen. Hylkää kaikki syöttötiedot, jotka eivät täytä vaatimuksia, tai muunna ne sellaiseksi, joka vastaa niitä. Älä luota pelkästään haitallisten tai väärin muotoiltujen syötteiden etsimiseen (eli älä luota estolistaan). Kieltoluettelot voivat kuitenkin olla hyödyllisiä mahdollisten hyökkäysten havaitsemisessa tai sen määrittämisessä, mitkä syötteet ovat niin vääriä, että ne pitäisi hylätä suoraan.

-SQL Injektioiden estämiseen voidaan olla luottamatta asiakas syötteeseen, vaikka asiakaspuolen validointi olisi käytössä. Kaikki kirjoitus tulisi tarkastaa palvelinpuolella. 

-Format String Errorin saa korjattua kirjoittamalla taustaohjelman uudelleen poistamalla huonot merkkijonot oikein. Tämä vaatii taustaohjelman uudelleen kääntämisen.

Ensimmäisillä testauksilla kirjautumissivuilta löytyi heti jo useita uhkia. Muun muassa uhkia oli sivujen erittäin heikkojen salasanojen ja käyttäjätunnuksien hyväksyminen. 

## 3. Löydökset ja löydöksien kategoriointi:

Yksityiskohtaiset kuvaukset ja kategoriat poikkeamista ja haavoittuvuuksista ovat tässä.

### Punainen (Kriittinen): Poikkeamia ja haavoittuvuuksia, joita punaiselta alueelta löysin, oli kaksi kappaletta. Nämä kaksi olivat Path Traversal ja SQL Injection.

Path Traversal on hyökkäystekniikka, joka mahdollistaa hyökkääjän pääsyn tiedostoihin, hakemistoihin ja komentoihin, jotka mahdollisesti sijaitsevat verkkoasiakirjan juurihakemis-ton ulkopuolella. Hyökkääjä voi manipuloida URL-osoitetta siten, että verkkosivusto suorittaa tai paljastaa mielivaltaisten tiedostojen sisällön missä tahansa verkkopalvelimessa. 

SQL-injektio on koodin lisäystekniikka, jota käytetään hyökkäämään tietopohjaisiin sovel-luksiin, jossa haitallisia SQL-lauseita lisätään syöttökenttään suorittamista varten (esim. tie-tokannan sisällön tyhjentämiseksi hyökkääjälle). SQL-injektion on hyödynnettävä sovelluk-sen ohjelmiston tietoturvaheikkoutta, esimerkiksi silloin, kun käyttäjän syöte on joko suoda-tettu väärin SQL-käskyihin upotetuille merkkijonokirjaimille tai käyttäjän syötettä ei ole kirjoi-tettu voimakkaasti ja se suoritetaan odottamattomasti.

### Keltainen (Keskitaso): Haavoittuvuuksia, joita löysin keltaiselta alueelta, oli kolme kappalet-ta, olivat Content Security Policy (CSP) Header Not Set, Format String Error, Missing Anti-clickjacking Header. Näistä kaksi ensimmäistä oli kuitenkin vain kirjautumissivuun liittyviä ja viimeinen liittyi selaimeen itseen. 

Content Security Policy (CSP) on suojaustaso, joka auttaa havaitsemaan ja lieventämään tietynlaisia hyökkäyksiä, mukaan lukien Cross Site Scripting (XSS) ja tietojen lisäyshyök-käykset. Näitä hyökkäyksiä käytetään kaikkeen tietovarkauksista sivuston turmelemiseen tai haittaohjelmien levittämiseen.

Format String - virhe ilmentyy, kun sovellus arvioi syötemerkkijonon lähetetyt tiedot komen-tona.

**Updated version2 5.3.2025**
Absence of Anti-CSRF Tokens. Eli HTML-lähetyslomakkeessa ei löytynyt Anti-CSRF-tunnuksia. Sivustoiden välinen pyyntöväärennös on hyökkäys, jossa uhri pakotetaan lähettämään HTTP-pyyntö kohdekohteeseen hänen tietämättään tai aikomuksensa suorittaakseen toiminnon uhrina. 

### Vihreä (Matala): Haavoittavuuksia, joita vihreältä alueelta löysin, oli kaksi kappaletta, jotka olivat Application Error Disclosure ja X-Content-Type-Options Header Missing. Vain Error Disclosure viittaa sivustoon itsessään, toisin kuin X-Content-Type-Options Header Missing, joka viittaa selaimeen itsessään.

Error Disclosure viittaa sivun sisältämään virheilmoitukseen, joka voi paljastaa arkaluontoi-sia tietoja, kuten käsittelemättömän poikkeuksen aiheuttaneen tiedoston sijainnin. Näitä tie-toja voidaan käyttää uusien hyökkäysten käynnistämiseen verkkosovellusta vastaan. Varoi-tus voi olla virheellinen, jos virheilmoitus löytyy dokumentaatiosivulta.

**Updated**
### Informational(Medium):
Tarkista, onko vasteissa eroja fuzzed User Agentin perusteella (esim. mobiilisivustot, pääsy hakukoneen indeksointirobotina). Vertaa vastauksen tilakoodia ja vastauksen rungon hash-koodia alkuperäiseen vastaukseen.

## 4. Appendices:
- 2025-02-19-BookingSystemPhase1-Report1.md
- 2025-03-05-ZAP-Report-Ver2.md
