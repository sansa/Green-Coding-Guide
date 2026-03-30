---
title: "Ohjelmiston kestävyyskädenjälki"
nav_order: 9
permalink: /handprint/
---

# 9. Ohjelmiston kestävyyskädenjälki

Vihreä koodaus, kuten tässä oppaassa kuvataan, keskittyy ensisijaisesti ohjelmiston suorittamisen suorien energiakustannusten vähentämiseen. Ohjelmistolla on kuitenkin myös välillinen ympäristöulottuvuus: järjestelmät, joita se mahdollistaa, käyttäytymiset, joita se välittää, ja fyysiset prosessit, jotka se korvaa tai parantaa, voivat tuottaa myönteisiä ympäristövaikutuksia, jotka ylittävät huomattavasti ohjelmiston oman energiakustannuksen. Tähän myönteiseen ympäristöpanokseen viitataan **ohjelmiston kestävyyskädenjälkenä**.

## 9.1 Jalanjäljestä kädenjälkeen

*Kädenjäljen* käsite on peräisin kestävyystutkimuksesta vastakohdaksi tutummalle *jalanjäljelle*. Siinä missä jalanjälki mittaa toiminnan ympäristörasitusta — kulutettuja resursseja ja syntyviä päästöjä — kädenjälki mittaa sen luomaa ympäristöhyötyä: jalanjälkeä, joka vältetään, vähennetään tai palautetaan toiminnan seurauksena (Biggs et al., 2020).

Ohjelmistoon sovellettuna jako hahmottuu selkeästi kahdeksi erilliseksi kysymykseksi:

- **Jalanjälkikysymys**: Kuinka paljon energiaa tämä ohjelmisto kuluttaa toimiessaan?
- **Kädenjälkikysymys**: Minkä ympäristöhaitan tämä ohjelmisto estää tai vähentää toimiessaan tarkoitetulla tavalla?

Nämä ovat toisistaan riippumattomia. Ohjelmistolla, jolla on suuri jalanjälki, voi olla suuri myönteinen kädenjälki — esimerkiksi laskennallisesti intensiivinen reittioptimaatiojärjestelmä, joka säästää miljoonia litroja polttoainetta vuosittain. Ohjelmistolla, jolla on pieni jalanjälki, voi olla merkityksetön kädenjälki — esimerkiksi hyvin optimoitu mutta toiminnallisesti triviaali sovellus. Ja ohjelmistolla voi joissain tapauksissa olla negatiivinen kädenjälki, kun se mahdollistaa toimintoja tai kulutuskuvioita, jotka lisäävät kokonaisympäristöhaittaa.

Vihreän koodauksen käytäntö käsittelee jalanjälkiulottuvuutta. Kädenjälkiulottuvuuden ymmärtäminen vaatii erilaisen ja laajemman kysymyksen esittämistä: *mitä tämä ohjelmisto mahdollistaa, ja mikä on sen ympäristöseuraus?*

## 9.2 Miten ohjelmisto luo myönteistä ympäristövaikutusta?

Ohjelmisto luo ympäristöhyötyä useiden eri mekanismien kautta. Seuraavat kategoriat havainnollistavat vaikutusten kirjoa fyysisten prosessien korvaamisesta fyysisten järjestelmien optimointiin.

**Dematerialisaatio**
Ohjelmisto voi korvata fyysisiä tavaroita ja palveluita, joiden tuottaminen ja toimittaminen vaatii energiaa, materiaaleja ja logistiikkaa. Digitaaliset asiakirjat korvaavat tulostetut. Videoneuvottelut korvaavat lennot. Suoratoistomedia korvaa fyysiset levyt ja niiden tuottamiseen ja jakeluun tarvittavat toimitusketjut. Dematerialisaatiosta saatava ympäristöhyöty on todellinen, mutta ei automaattinen: se riippuu siitä, otetaanko digitaalinen vaihtoehto käyttöön fyysisen sijaan eikä sen lisäksi (Hilty and Aebischer, 2015).

**Etätyön ja hajautetun yhteistyön mahdollistaminen**
Etätyötä tukevat ohjelmistoalustat vähentävät päivittäisen pendelöinnin ja työmatkustamisen tarvetta, jotka molemmat ovat merkittäviä liikenteen päästöjen lähteitä. Ympäristöhyöty riippuu siitä, tapahtuuko matkakorvaus todellisuudessa, ja sitä tasapainottaa jonkin verran lisääntynyt asumisenergiankäyttö ja yhteistyöohjelmiston oma energiakustannus. Nettovaikutus on yleensä myönteinen, kun pitkät työmatkasukkuloinnit tai lennot korvataan (Hook et al., 2020).

**Fyysisten järjestelmien optimointi**
Jotkut suurimmista kädenjälkimahdollisuuksista syntyvät ohjelmistosta, joka tekee fyysisestä infrastruktuurista tehokkaamman. Esimerkkejä ovat:
- *Älykkäät rakennusten hallintajärjestelmät*, jotka säätävät dynaamisesti lämmitystä, jäähdytystä ja valaistusta käytön ja sään perusteella, vähentäen rakennusten energiankulutusta, joka kattaa huomattavan osan maailman energiankäytöstä.
- *Älykkäät liikennejärjestelmät*, jotka optimoivat liikennevirtoja, vähentävät tyhjäkäyntiä ja parantavat joukkoliikenteen aikataulutusta, alentaen polttoaineen kulutusta ajoneuvokannassa.
- *Täsmämaatalousplatformit*, jotka käyttävät anturidataa ja mallinnusta levittämään vettä, lannoitteita ja torjunta-aineita vain sinne ja silloin kuin niitä tarvitaan, vähentäen resurssien kulutusta ja siihen liittyviä päästöjä.
- *Sähköverkon hallinta- ja kysyntäjoustoohjelmistot*, jotka integroivat uusiutuvia energianlähteitä, siirtävät joustavaa kysyntää uusiutuvan energian tuotannon huippuaikoihin ja vähentävät tuuli- ja aurinkovoiman leikkausta.

**Tieteellisen ja teknisen edistyksen mahdollistaminen**
Ohjelmistotyökalut, jotka nopeuttavat puhtaan energian, materiaalitieteen, ilmastomallinuksen ja vähähiilisen insinöörityön tutkimusta, voivat tuottaa kädenjälkivaikutuksia, jotka kertautuvat ajan myötä. Simulaatiotyökalun ympäristöarvoa, joka alentaa tehokkaamman akkukemian kehittämiskustannuksia, ei voida tavoittaa ohjelmiston omassa energiajalanjäljessä; se ilmenee sen mahdollistaman tutkimuksen jatkovaikutusten kautta.

## 9.3 Kädenjälkiväitteiden arviointi

Kädenjäljen käsite on arvokas mutta helposti väärinkäytetty. Koska kädenjälkivaikutukset ovat välillisiä ja sisältävät kontrafaktuaaleja (mitä *olisi tapahtunut* ilman ohjelmistoa), ne ovat luonteeltaan vaikeampia mitata ja todentaa kuin jalanjälkivaikutukset. Useat periaatteet ohjaavat tiukkaa kädenjälkiarviointia.

**Additionaalisuus**
Aito kädenjälki edellyttää, että ympäristöhyöty ei olisi toteutunut ilman ohjelmistoa. Jos reittioptimaatiojärjestelmä otetaan käyttöön korvaamaan järjestelmää, joka jo suoritti riittävää optimaatiota, uuden järjestelmän marginaalihyöty on relevantti kädenjälki, ei minkä tahansa reittioptimaation kokonaisvältettyjä päästöjä. Additionaalisuus on usein liioiteltu kestävyysraportoinnissa.

**Rebound-vaikutukset**
Ohjelmiston mahdollistamat tehokkuusparannukset voivat käynnistää lisääntymistä tehokkuutta tekevässä toiminnassa, osittain tai kokonaan kumoamalla ympäristöhyödyn. Tehokkaampi navigointi vähentää polttoaineen kulutusta matkaa kohden, mutta halvempi, nopeampi tai kätevämpien liikennemahdollisuuksien seurauksena matkojen kokonaismäärä voi kasvaa. Tehokkaampi datanpakkaus vähentää energiakustannusta gigatavua kohden, mutta alhaisempi kustannus gigatavua kohden lisää kokonaisdatan kulutusta. Nämä *rebound-vaikutukset* on otettava huomioon arvioitaessa nettokädenjälkeä.

**Kontrafaktuaalinen tarkkuus**
Kädenjälkiväite edellyttää sen määrittämistä, mikä vaihtoehtoinen skenaario on. "Ohjelmisto X vähentää hiilipäästöjä" ei ole täydellinen väite; se on verrattava määriteltyyn vertailutasoon. Onko vaihtoehto olla käyttämättä lainkaan ohjelmistoa? Vanhempi versio ohjelmistosta? Manuaalinen prosessi? Kilpailijan tuote? Kädenjäljen suuruus riippuu voimakkaasti siitä, mikä kontrafaktuaali valitaan, ja valinnan on oltava eksplisiittinen ja perusteltu.

**Laajuus ja aikahorisontti**
Kädenjälkivaikutukset voivat toimia hyvin erilaisilla aikaskaalavilla. Reittioptimaatiojärjestelmän säästämä polttoaine realisoituu välittömästi, kun taas puhtaan energian tutkimuksen nopeuttamisen vaikutus voi materialisoitua vuosikymmenten kuluessa. Pitkät aikahorisontit lisäävät epävarmuutta. Ole eksplisiittinen kädenjälkiväitteen laajuuden suhteen: mitkä vaikutukset sisällytetään, millä ajanjaksolla ja millä epävarmuudella.

## 9.4 Nettovaikutus ja jalanjäljen ja kädenjäljen suhde

Useimmissa ohjelmistojärjestelmissä suora energiajalanjälki on pieni suhteessa toimitettuun toiminnalliseen arvoon. Hyvin suunniteltu älytermostaattijärjestelmä saattaa esimerkiksi kuluttaa muutaman kilowattitunnin vuodessa viestintään ja laskentaan mahdollistaessaan samalla lämmitys- ja jäähdytyssäästöjä sadoissa kilowattitunneissa vuodessa hallinnoimissaan rakennuksissa. Tällaisissa tapauksissa kädenjälki dominoi selvästi jalanjälkeä, ja ohjelmiston nettopäästövaikutus on selvästi myönteinen.

Tämä suotuisa suhde ei kuitenkaan ole universaali, eikä sitä pitäisi olettaa. Ohjelmisto, joka kuluttaa merkittävästi energiaa — kuten laajamittainen datankäsittely, jatkuva videosuoratoisto tai tekoälymallien koulutus — voi vaatia positiivista nettovaikutusta vain, jos sen kädenjälkivaikutukset ovat todellisia, additionaalisia ja riittävän suuria jalanjäljen kumoamiseen. Tähän suuntaan esitetyt väitteet tulisi pitää samaan todistushaarkan tasoon kuin mikä tahansa muu empiirinen väite.

Ohjelmistokehittäjille ja arkkitehdeille käytännön implikaatio on tämä: **jalanjäljen vähentäminen ja kädenjäljen ymmärtäminen ovat toisiaan täydentäviä, eivät kilpailevia huolenaiheita**. Järjestelmä, jolla on aito myönteinen kädenjälki, voi maksimoida nettohyötynsä minimoimalla myös toiminnallisen jalanjälkensä. Ympäristövaikutusten tiukka, kvantitatiivinen, kontrafaktuaalinen ja rebound-vaikutukset huomioiva arviointikuri on sama kuri riippumatta siitä, sovelletaanko sitä jalanjälkeen vai kädenjälkeen.

---

## Viitteet

Biggs, R., de Vos, A., Preiser, R., Clements, H., Maciejewski, K. and Schlüter, M. (eds.) (2020) *The Routledge Handbook of Research Methods for Social-Ecological Systems*. Routledge.

Hilty, L.M. and Aebischer, B. (2015) *ICT for sustainability: An emerging research field*. In: Hilty, L.M. and Aebischer, B. (eds.) ICT Innovations for Sustainability. Springer, pp.3–36.

Hook, M., Sovacool, B.K. and Sorrell, S. (2020) *A systematic review of the energy and climate impacts of teleworking*. Environmental Research Letters, 15(9), 093003.

Plepys, A. (2002) *The grey side of ICT*. Environmental Impact Assessment Review, 22(5), pp.509–523.
