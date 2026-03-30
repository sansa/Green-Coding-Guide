---
title: "Ohjelmiston energiajalanjälki"
nav_order: 8
permalink: /footprint/
---

# 8. Ohjelmiston energiajalanjälki

Ohjelmiston energiajalanjäljen ymmärtäminen edellyttää siirtymistä pois intuitiivisista käsityksistä siitä, mikä koodi on "nopeaa" tai "tehokasta", kohti systemaattista ymmärrystä siitä, miten ohjelmiston toiminta muuntuu energiankulutukseksi. Tämä luku tarjoaa käsitteellisen perustan tuolle ymmärrykselle.

## 8.1 Mitä ohjelmiston energiajalanjälki tarkoittaa?

**Ohjelmiston energiajalanjälki** on se kokonaisenergiamäärä, jonka laitteistojärjestelmä kuluttaa suoraan seurauksena tietyn ohjelmiston suorittamisesta määritellyllä ajanjaksolla tai työkuormalla. Se ei ole lepotilassa olevan koodin ominaisuus; se ilmenee suorituksen aikana ja riippuu siitä, mitä ohjelmisto tekee, kuinka usein, millä laitteistolla ja millaisissa olosuhteissa.

Tämä suoritusaikainen luonne erottaa ohjelmiston energiajalanjäljen elinkaarilähtöisistä näkökulmista, jotka ottavat huomioon myös laitteistovalmistukseen, konesalirakentamiseen tai käytöstäpoistoon sitoutuneen energian. Nämä näkökohdat ovat todellisia ja merkittäviä, mutta ne ovat suurelta osin ohjelmistokehittäjän toteutuspäätösten ulottumattomissa. Suoritusaikainen jalanjälki sen sijaan muotoutuu suoraan ohjelmiston suunnitteluvalinnoista ja on siksi vihreän koodauksen käytännön ensisijainen kohde.

Jalanjälki on myös luonteeltaan **työkuormariippuvainen**. Sama koodipohja voi tuottaa huomattavasti erilaisen energiajalanjäljen syötteen koon, samanaikaisten käyttäjien määrän, pyyntöjen tiheyden ja välimuistissa olevan datan määrän mukaan. Tämä tarkoittaa, että energiaa ei voida mielekkäästi luonnehtia pelkästään koodia tarkastelemalla; se edellyttää mittaamista edustavissa olosuhteissa.

## 8.2 Miten ohjelmisto kuluttaa energiaa?

Ohjelmisto ei kuluta energiaa suoraan. Se saa laitteistokomponentit tekemään työtä, ja nuo komponentit kuluttavat energiaa. On olennaista ymmärtää, mitkä komponentit ovat osallisina ja mikä ajaa niiden kulutusta, jotta voidaan tunnistaa, missä optimointityöllä on suurin vaikutus.

**Prosessori (CPU/GPU/TPU)**
Prosessori on tyypillisesti hallitseva energiankuluttaja aktiivisen laskennan aikana. Dynaaminen teho, eli transistorien kytkennästä aiheutuva energiankulutus, skaalautuu kellotaajuuden ja aktiivisten piirien määrän mukaan. Modernit prosessorit käyttävät taajuus- ja jänniteskaalausta tehon vähentämiseksi alhaisella kuormituksella, mutta korkea käyttöaste ajaa ne kohti huipputehon rajaa. Ohjelmisto, joka pitää prosessorit korkean käyttöasteen tilassa pitkiä aikoja, vaikuttaa suhteettomasti energiankulutukseen.

**Muisti**
Dynaaminen RAM kuluttaa energiaa sekä käytön että valmiustilan aikana. Muistin luku- ja kirjoitusoperaatiot, erityisesti ne, jotka ohittavat CPU-välimuistin ja ulottuvat päämuistiin, ovat operaatiokohtaisesti huomattavasti energiaintensiivisempiä kuin L1- tai L2-välimuistissa pysyvät operaatiot. Ohjelmisto, jolla on heikko datan paikallisuus, suuri työskentelymäärä tai korkea allokointitahti ja roskienkeruu, lisää DRAM-energiankulutusta.

**Verkkoliitännät**
Datan lähettäminen ja vastaanottaminen verkkoliitännän kautta vaatii energiaa suhteessa siirretyn datan määrään, lisättynä kiinteällä ylikuormituksella yhteyden muodostamisesta ja protokollaneuvottelusta. Mobiiliympäristöissä radioliitännöillä (LTE, 5G, Wi-Fi) on erityisen korkeat energiakustannukset suhteessa siirrettyyn dataan, ja ne pysyvät kohonneessa tehotilassa jonkin aikaa lähetyksen päättymisen jälkeen — ilmiö tunnetaan nimellä *radiokaista* — mikä tekee monista pienistä lähetyksistä huomattavasti kalliimpia kuin harvemmista suuremmista (Pathak et al., 2012).

**Tallennus**
Puolijohdetallennus kuluttaa energiaa luku- ja kirjoitusoperaatioiden aikana, ja kirjoitukset ovat tyypillisesti kalliimpia kuin luvut. Pyörivällä levyllä on lisäksi mekaanisia energiakustannuksia. Ohjelmisto, joka suorittaa tarpeetonta I/O:ta — kuten kirjoittaa tarpeettomia lokeja, lukee uudelleen muuttumattomia tiedostoja tai suorittaa usein pieniä satunnaisia kirjoituksia — lisää tallennuksen energiankulutusta suhteessa käyttötiheyteen ja -volyymiin.

**Näyttö**
Käyttäjälle näkyvissä sovelluksissa näyttö voi olla merkittävä energiankuluttaja, erityisesti mobiililaitteissa, joissa näytön kirkkaus ja renderöinnin monimutkaisuus vaikuttavat suoraan akun kulumiseen. Ohjelmisto, joka pitää näytön tarpeettomasti aktiivisena, renderöi täydessä resoluutiossa kun alempi riittäisi, tai käynnistää tiheästi näyttöpäivityksiä näkymättömiin muutoksiin, lisää näytön energiankulutusta.

## 8.3 Energian kohdentaminen ohjelmistolle

Keskeinen haaste vihreässä koodauksessa on se, että energia mitataan järjestelmätasolla kokonaisvirtalähteen tehona, kun taas vastuu kulutuksesta jakautuu käyttöjärjestelmän, ajonaikaisympäristön, kehysten, sovellusloodin ja samanaikaisesti suoritettavien prosessien kesken. Tietyn osuuden kohdentaminen järjestelmäenergiasta tietylle ohjelmistokomponentille on teknisesti vaikeaa ja sisältää merkittävää epävarmuutta.

Käytännössä käytetään useita lähestymistapoja:

**Laitteiston suorituskykylaskurit**
Modernit prosessorit paljastavat energianmittausrekisterit, joita ohjelmisto voi lukea. Intelin Running Average Power Limit (RAPL) -rajapinta tarjoaa esimerkiksi pakettikohtaisia ja toimialuekohtaisia (CPU, DRAM, uncore) energialukemia suurella tarkkuudella. Nämä ovat suorimmat ohjelmistosta käytettävissä olevat energiamittaukset kaupallisella laitteistolla, mutta ne kattavat vain prosessori- ja muistialijärjestelmät eivätkä ole saatavilla kaikissa virtualisoiduissa tai pilviympäristöissä.

**Järjestelmätason tehomittaus**
Ulkoiset tehomittarit mittaavat järjestelmän kokonaisenergiaa sähköpistokkeesta tai laitekotelon tasolta. Tämä kattaa kaikki komponentit, mutta ei kohdistu yksittäisiin prosesseihin tai ohjelmistokomponentteihin. Se soveltuu parhaiten kahden identtisissä olosuhteissa eristyksessä suoritettavan työkuormaversion vertailuun.

**Mallipohjainen arviointi**
Kun suora mittaus ei ole saatavilla — kuten pilvi- ja jaetussa infrastruktuurissa on yleistä — energiankulutus voidaan arvioida CPU-käyttöasteesta, muistin käytöstä ja työkuorman ominaisuuksista empiirisillä malleilla. Tällaisten arvioiden tarkkuus vaihtelee huomattavasti ja riippuu taustalla olevan laitteistokarakterisaation laadusta. Cloud Carbon Footprint -projekti ja Green Software Foundationin Software Carbon Intensity (SCI) -määrittely ovat esimerkkejä kehyksistä, jotka formalisoivat tämän lähestymistavan.

Mittauslähestymistavasta riippumatta kohdentaminen on luotettavinta, kun yksittäinen työkuorma suoritetaan eristettynä tunnetulla laitteistolla. Tuotantoympäristöissä kohdentaminen on aina osittaista, ja tuloksia tulee tulkita sen mukaisesti (ks. myös [Luku 7, Mittausepävarmuus](/tradeoffs/)). Käytännöllinen demonstraatio API-tason kohdentamisesta yhdistää kontaineroidun suorituksen — joka eristää kohdetyökuorman taustapalveluista — ohjelmiston energiaprofiloijaan kuten PowerJoular, jolla voidaan kohdentaa tehonkulutus tiettyihin päätepisteisiin ja commit-versioihin. Tämä lähestymistapa saavuttaa hyvän yhteneväisyyden suorien profiloijalukemien kanssa (Pearsonin r = 0,94) vaatien vain kaupallista laitteistoa ja avoimen lähdekoodin työkaluja (Joof et al., 2025; Joof, 2025).

## 8.4 Energia, teho ja hiili: käsitteiden selventäminen

Nämä kolme toisiinsa liittyvää käsitettä sekoitetaan usein toisiinsa, mutta ne ovat erillisiä ja jokaisella on eri merkitys mittaamisen ja optimoinnin kannalta.

**Teho** (mitataan watteina, W) on energiankulutuksen nopeus hetkellä. Prosessi, joka kuluttaa 50 W, kuluttaa energiaa sillä nopeudella. Teho yksin ei kerro kokonaiskulutettua energiaa, sillä se riippuu siitä, kuinka kauan tehoa otetaan.

**Energia** (mitataan jouleina, J, tai kilowattitunteina, kWh) on tehon ja ajan tulo: *E = P × t*. Prosessi, joka kuluttaa 50 W 10 sekunnin ajan, kuluttaa 500 J. Energia on oikea yksikkö työkuorman suorituksen kokonaiskustannuksen vertailuun, koska se ottaa huomioon sekä tehonkulutuksen että keston. Nopeampi mutta tehoahnaampi toteutus ei välttämättä ole energiatehokkaampi kuin hitaampi, pienempikulutteinen vaihtoehto, koska kokonaisenergia — tehon integraali ajan yli — määrää jalanjäljen.

**Hiilidioksidipäästöt** (mitataan grammoina tai kilogrammoina CO₂-ekvivalenttia, CO₂e) kuvaavat energiankulutuksen ilmastovaikutusta. Sama kilowattitunti sähköä tuottaa erilaisia hiilidioksidipäästöjä lähteestä riippuen: lähes nollaan aurinko- tai tuulivoimalla, ja huomattavasti enemmän hiilellä tai kaasulla. Hiilivoimakkuus, mitattuna grammoina CO₂e per kilowattitunti, vaihtelee sähköverkkoalueen, vuorokauden ajan ja vuodenajan mukaan. Energiajalanjäljen ja hiilijalanjäljen optimointi osoittavat yleensä samaan suuntaan, mutta suhde ei ole kiinteä, ja työkuormien maantieteellinen tai ajallinen siirto voi vähentää hiilipäästöjä energiansäästöstä riippumatta.

Useimmissa ohjelmiston optimointitöissä **energia** on suoraan toimenpidekelpoisin mittari. Hiilitilitys on relevanteinta infrastruktuuri- ja käyttöönottostratategian tasolla, jossa pilvipalvelualueen, energiahankinnan ja työkuorman ajoituksen valinnat ovat vuorovaikutuksessa sähköverkon hiilivoimakkuuden kanssa.

## 8.5 Ohjelmiston energiajalanjälkeä muovaavat tekijät

Ohjelmiston energiajalanjälkeä ei määrää koodi yksin. Se on ohjelmiston toiminnan ja suoritusympäristön välisen vuorovaikutuksen tuote. Keskeisimmät tekijät ovat:

**Algoritminen monimutkaisuus**
Algoritmin asymptoottinen monimutkaisuus määrää, miten sen resurssienkulutus skaalautuu syötteen koon myötä. O(n²)-algoritmi sovellettuna suureen tietojoukkoon voi kuluttaa kertaluokkia enemmän energiaa kuin O(n log n) -vaihtoehto. Algoritmivalinnoilla on siten suurin vaikutus energiajalanjälkeen mittakaavassa, ja ne tehdään kehityksen alkuvaiheessa, mikä tekee niistä energianäkökulmasta tärkeimpiä arvioitavia päätöksiä (Pereira et al., 2017). Tämä vaikutus on mitattavissa jopa pienessä mittakaavassa: mukautetun sisäkkäissilmukkaisen lajittelun korvaaminen kielen sisäänrakennetulla lajittelulla backend-API:ssa vähensi tehonkulutusta 23 % ja CPU-käyttöastetta 34,5 %:sta 26,1 %:iin standardoidulla pyyntötyökuormalla (Joof et al., 2025; Joof, 2025).

**Datan käyttökuviot**
Sillä, toimivatko laskelmat CPU-välimuistissa, päämuistissa vai levyllä olevalla datalla, on suuri vaikutus energiankulutukseen. Välimuistiystävälliset tietorakenteet ja käyttökuviot, erityisesti ne, jotka osoittavat spatiaalista ja ajallista paikallisuutta, ovat huomattavasti energiatehokkaampia kuin ne, jotka hajottavat muistikäyttöjä suurille osoitealueille.

**Rinnakkaisuus ja rinnakkaislaskenta**
Rinnakkaislaskenta lisää laitteiston käyttöastetta ja voi lyhentää reaaliaikaista suoritusaikaa, mutta se ei välttämättä vähennä energiankulutusta. Jos useat säikeet suorittavat redundanttia työtä tai jos synkronointiylimäärä on suuri, rinnakkaissuoritus voi kuluttaa enemmän energiaa kuin saman tehtävän peräkkäinen suoritus. Toisaalta hyvin jäsennelty rinnakkaislaskenta voi parantaa energiatehokkuutta suorittamalla työn nopeammin ja mahdollistamalla laitteiston palautumisen alhaisempiin tehotilaan aiemmin.

**Laitteiston ja ajonaikaisen ympäristön ominaisuudet**
Sama koodi tuottaa erilaisia energiajalanjälkiä eri laitteistoilla. Prosessorin mikroarkkitehtuuri, muistihierarkian rakenne ja laitteistoalustan energiaproporitionaalisuus (kuinka läheisesti valmiusteho skaalautuu kuorman mukaan) vaikuttavat kaikki mitattuun jalanjälkeen. Ajonaikaiset ominaisuudet kuten JIT-käännös, roskienkeruu ja tulkin ylikuormitus lisäävät lisää vaihtelua.

**Käyttöönotto ja skaalaus**
Käynnissä olevien instanssien määrä, niiden palvelema kuorma ja pyyntöjen välinen valmiusaika vaikuttavat kaikki kokonaisenergiankululukseen. Erittäin optimoitu yksittäinen instanssi, joka toimii hyvin alhaisella käyttöasteella, voi kuluttaa enemmän energiaa kuin kohtuullisesti optimoitu instanssi, joka toimii korkealla käyttöasteella, palvelimen käynnissäpitämisen kiinteän energiakustannuksen vuoksi kuormasta riippumatta.

---

## Viitteet

Green Software Foundation (2023) *Software Carbon Intensity (SCI) Specification*. Version 1.0. Available at: https://sci.greensoftware.foundation

Pathak, A., Hu, Y.C. and Zhang, M. (2012) *Where is the energy spent inside my app? Fine-grained energy accounting on smartphones with Eprof*. Proceedings of the 7th ACM European Conference on Computer Systems (EuroSys), pp.29–42.

Pereira, R., Couto, M., Ribeiro, F., Rua, R., Cunha, J., Fernandes, J.P. and Saraiva, J. (2017) *Energy efficiency across programming languages: How do energy, time, and memory relate?* Proceedings of the 10th ACM SIGPLAN International Conference on Software Language Engineering, pp.256–267.

Treiber, M. (2015) *RAPL in action: Experiences in using RAPL for power measurements*. ACM Transactions on Modeling and Performance Evaluation of Computing Systems, 1(1), pp.1–26.

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
