---
title: "Vihreän koodauksen käytännöt toimialoittain"
nav_order: 4
permalink: /domain-practices/
---

# 4. Vihreän koodauksen käytännöt toimialoittain

Vihreän koodauksen periaatteet soveltuvat kaikille toimialoille, mutta niiden käytännöllinen vaikutus riippuu suorituskontekstista. Tässä osiossa käytännöt on järjestetty toimialoittain keskittyen **suorituksenaikaisen energiajalanjäljen keskeisiin tekijöihin**. Alla esitetyt käytännöt laajentavat [osion 3](/core-principles/) ydinyperiaatteita ja tarjoavat käytännön lähtökohdan kullekin toimialalle.

---

## 4.1 Frontend – Verkkosovellukset

Energiajalanjäljen keskeisiä tekijöitä ovat JavaScript-suoritus, renderöinti ja verkkoliikenne. Koska sama koodi suoritetaan miljoonilla käyttäjien laitteilla, frontendin tehottomuus moninkertaistuu koko käyttäjäkunnan laajuudella, jolloin jopa pienet sivukohtaiset parannukset ovat merkittäviä laajassa mittakaavassa (Manotas et al., 2016).

**Minimoi tarpeettomat uudelleenrenderöinnit ja layout thrashing**
Komponenttipohjaisissa kehyksissä vanhemman tilan muutos voi käynnistää uudelleenrenderöinnin alipuissa, joihin muutos ei vaikuta. Jokainen uudelleenrenderöinti sisältää erojen laskemisen, DOM:n paikkaamisen, tyylin uudelleenlaskennan ja mahdollisesti layoutin, jotka kaikki ovat CPU-työtä, joka tuottaa lämpöä käyttäjän laitteessa. Käytä muistintallennusta (memoisation), valitsijafunktioita ja komponenttien pilkkomista varmistaaksesi, että vain todella muuttuneet komponentit päivittyvät. Vältä myös DOM-lukujen ja -kirjoitusten vuorottelua JavaScript-silmukoissa, mikä pakottaa selaimen laskemaan layoutin toistuvasti uudelleen (tunnetaan nimellä layout thrashing).

**Pienennä bundle-kokoa koodin jakamisella ja laiskalla latauksella**
Suuremmat JavaScript-bundlet vaativat enemmän jäsentämistä, kääntämistä ja suoritusaikaa, joista kaikki kuluttavat energiaa ennen kuin yhtäkään sovelluksen logiikan riviä ajetaan. Koodin jakaminen varmistaa, että käyttäjät lataavat ja suorittavat vain sen koodin, jota tarvitaan heidän nykyisessä näkymässään. Laiska lataus lykkää muun lataamista siihen asti, kun sitä todella tarvitaan. Tarkasta bundlen koostumus säännöllisesti rakennusanalyysityökaluilla ja poista käyttämättömät riippuvuudet.

**Vältä runsasta API-liikennettä**
Useat pienet, peräkkäiset API-kutsut pitävät verkkoliitännän aktiivisena ja lisäävät viivettä. Jokaisella verkkopyynnöllä on myös kiinteä energiakustannus hyötykuorman koosta riippumatta: yhteyden muodostaminen, DNS-selvitys ja TCP/TLS-kättelyn suorittaminen. Yhdistä toisiinsa liittyvät pyynnöt, käytä HTTP/2-multipleksointia tarvittaessa ja harkitse kyselykieliä (kuten GraphQL), joilla asiakas voi määrittää tarvitsemansa tiedot tarkasti.

**Käytä välimuistia ja HTTP-mekanismeja tehokkaasti**
Tarpeeton tiedonsiirto on yksi eniten vältettävistä frontendin energiankulutuksen lähteistä. `Cache-Control`-otsikoiden, ETagien ja service workereiden oikea käyttö voi poistaa kokonaan muuttumattomien resurssien uudelleenlataukset. Mittaa todelliset välimuistin osumataajuudet varmistaaksesi, että välimuisti toimii tarkoitetulla tavalla.

**Suosi CSS- ja laitteistokiihdytettyjä animaatioita JavaScript-silmukoiden sijaan**
JavaScript-ohjatut animaatiot toimivat pääsäikeessä ja kilpailevat layoutin ja renderöinnin kanssa. CSS-siirtymät ja Web Animations API delegoivat työn compositor-säikeelle, joka on tyypillisesti GPU-kiihdytetty eikä estä pääsäikeen suoritusta. Käytä animaatioissa `transform`- ja `opacity`-ominaisuuksia, sillä nämä ominaisuudet voidaan yhdistää ilman layoutin tai piirron käynnistämistä.

---

## 4.2 Frontend – Mobiilisovellukset

Mobiilin energiajalanjälki koostuu pääasiassa akkujen käytöstä CPU:n, verkkoradioiden, näytön ja antureiden toiminnassa. Laitteen herättäminen matalan virrankulutuksen tilasta on merkittävä kustannus: yksi tarpeeton herätys voi kuluttaa yhtä paljon energiaa kuin monta millisekuntia normaalia CPU-toimintaa (Pathak et al., 2012).

**Vähennä taustatoimintaa ja herätyksiä**
Jokainen taustaherätys pakottaa CPU:n ja mahdollisesti verkkoradion pois matalan virrankulutuksen unitilasta. Yhdistä taustatoiminnot alustan tarjoamia API:ja käyttäen, kuten `WorkManager` Androidilla ja `BGTaskScheduler` iOS:llä, jotka sallivat käyttöjärjestelmän ajoittaa työn laitteen jo aktiivisina tai latauksessa olevina jaksoina. Vältä erillisiä ajastimia ja hälytyksiä tehtäville, jotka sietävät eräajoa.

**Niputa verkkopyynnöt ja tue offline-työnkulkuja**
Matkapuhelinradio (LTE/5G) on yksi mobiililaitteen energiaintensiivisimmistä komponenteista ja kuluttaa merkittävästi virtaa myös siirron päätyttyä olevan odotusjakson aikana. Pyyntöjen niputtaminen harvempiin, suurempiin vuorovaikutuksiin lyhentää aikaa, jonka radio viettää aktiivisessa ja odotustilassa. Offline-työnkulkujen tukeminen tallentamalla ja toistamalla toiminnot, kun yhteys palautuu, vähentää edelleen tarpeetonta tiedonsiirtoa.

**Optimoi renderöinti ja vierittäminen**
Kuvaruudun ulkopuolinen renderöinti, liiallinen overdraw (saman pikselin piirtäminen useita kertoja per kuva) ja toistuvat layoutin läpikäynnit kuluttavat akkua suoraan. Käytä näkymäkierrätysmenetelmiä (esim. `RecyclerView`, `LazyColumn`) välttääksesi näytön ulkopuolisen sisällön renderöinnin. Vähennä overdrawn tasaistaen näkymähierarkioita ja poistaen etusisällön peittämät opaakit taustat.

**Käytä antureita säästeliäästi ja sopivalla tarkkuudella**
GPS täydellä tarkkuudella ja korkealla kyselytaajuudella on yksi mobiililaitteen energiaintensiivisimmistä antureista. Jos karkea sijainti riittää, käytä `PRIORITY_LOW_POWER`- tai geofencing-menetelmää jatkuvan tarkan seurannan sijaan. Sovella samaa periaatetta kiihtyvyysantureihin, gyroskooppeihin ja mikrofoneihin: pyydä alin näytteenottotaajuus ja tarkkuus, joka täyttää toiminnallisen vaatimuksen, ja poista kuuntelijoiden rekisteröinti, kun niitä ei enää tarvita.

---

## 4.3 Backend – Palvelut ja API:t

Backendin energiajalanjälkeä ohjaavat CPU-käyttö per pyyntö, tietojenkäyttömallit ja verkkoviestinnän volyymi. Koska yksi backend-instanssi palvelee monia samanaikaisia käyttäjiä, tehottomuudet kertautuvat nopeasti liikenteen kasvaessa.

**Vähennä pyyntökohtaista työtä**
Profiloi kuumat polut tunnistaaksesi operaatiot, jotka suoritetaan jokaisella pyynnöllä. Jopa yhden kalliin tietokantakutsun, laskennan tai ulkoisen palvelukutsun poistaminen tai lykkääminen vilkkaasta päätepisteestä voi tuottaa merkittäviä kokonaisenergian säästöjä palvelinflotassa. Kohtele pyyntökohtaista CPU-aikaa resurssipankkina, jota tulee puolustaa koodikannan kehittyessä. Hallitussa tutkimuksessa, jossa verrattiin API-suunnitteluvaihtoehtoja, O(n²)-sisäkkäissilmukkalajittelun korvaaminen optimoidulla sisäänrakennetulla vastineella vähensi tehonkulutusta noin 23 % ja vasteaikaa 43 % optimoimattomaan lähtötasoon verrattuna (Joof et al., 2025; Joof, 2025).

**Vältä N+1-kyselyitä ja liiallista hakemista**
Entiteettilistan lataaminen ja sen jälkeen yksittäisten kyselyiden tekeminen jokaiselle entiteetille on yksi yleisimmistä ja vältettävissä olevista energiantuhlauksen malleista backend-kehityksessä. Korvaa N+1-mallit yksittäisellä erä-kyselyllä tai ORM:n tai kyselynrakentajan tarjoamalla innokkaaseen lataamiseen tarkoitetulla mekanismilla. Yhtä tärkeää on valita vain tarvittavat sarakkeet, sillä kokonaisten rivien hakeminen, kun tarvitaan vain kaksi kenttää, tuhlaa I/O-kaistanleveyttä ja lisää serialisointiylikuormaa.

**Käytä rajattua, mitattua välimuistia**
Välimuisti on hyödyllinen vain, jos osumataajuus oikeuttaa muistijalanjäljen ja vanhenemisriskin. Rajoittamaton välimuisti lisää GC-painetta ja muistinkulutusta; väärällä poistokäytännöllä varustettu välimuisti saattaa pitää sisällään vanhentuneita tai harvoin käytettyjä merkintöjä. Instrumentoi välimuistit osumataajuuksien mittaamiseen ja aseta TTL:t ja kokorajoitukset havaittuihin käyttömalleihin eikä oletusarvoihin perustuen. Empiiriset tulokset API-tason profiloinnista osoittavat, että muistinsisäisen välimuistin käyttöönotto toistuviin kyselyihin vähentää tehonkulutusta noin 15 % ja vasteaikaa 36 % välimuistittomaan lähtötasoon verrattuna (Joof et al., 2025; Joof, 2025).

**Minimoi hyötykuorman koko ja serialisointiylikuorma**
JSON-serialisointi ja -deserialisointi on CPU-intensiivistä suuressa mittakaavassa. Palveluiden välisessä sisäisessä viestinnässä binäärimuodot kuten Protocol Buffers tai MessagePack voivat vähentää sekä CPU-aikaa että siirtomäärää. Ulkoisille API:lle tarjoa vastauksen kenttäsuodatus (esim. `fields`-kyselyparametrien tai GraphQL-valintojen kautta), jotta asiakkaita ei pakoteta vastaanottamaan ja hylkäämään tietoa, jota he eivät käytä. Huomaa, että hyötykuorman pakkaaminen sisältää selkeän kompromissin: GZIP-koodaus vähentää verkonsiirtoa mutta lisää CPU-kulutusta, ja nettoenergian vaikutus riippuu siirron ja käsittelyn suhteellisista kustannuksista käyttöympäristössä (Joof et al., 2025; Joof, 2025).

---

## 4.4 Backend – Datan käsittely ja tallennus

Dataputket kuluttavat energiaa pitkittyneen suorituksen ja laajamittaisen I/O:n kautta. Toisin kuin interaktiiviset palvelut, putkilinjat toimivat usein ilman vastaavan käyttäjän odottamista, mikä tarkoittaa, että niiden energiankulutus on vähemmän näkyvää mutta ei yhtään vähäisempää.

**Vältä muuttumattoman datan täydellistä uudelleenlaskentaa**
Putkilinjan uudelleenajaminen syötedatalle, joka ei ole muuttunut edellisestä ajosta, on puhdasta tuhlausta. Tunnista muuttumattomat osiot tarkistussummien, muokkausaikaleimamerkintöjen tai inkrementaalisten laskentakehysten (kuten Apache Sparkin structured streaming tai dbt:n inkrementaalimallit) avulla ja ohita ne. Jopa osittainen uudelleenlaskennan laajuuden vähennys voi merkittävästi pienentää putkilinjan kokonaisenergiaa.

**Käytä inkrementaalista ja tarkistuspistepohjaista käsittelyä**
Pitkään suorittavat eräajot, jotka käynnistyvät alusta epäonnistumisen jälkeen, tuhlaavat kaiken epäonnistuneeseen ajoon investoidun energian. Tarkistuspisteytys, joka tallentaa ajoittain välivaiheen tilan kestävään tallennustilaan, mahdollistaa putkilinjan jatkamisen viimeisestä vakaasta kohdasta alun sijaan. Tarkistuspisteiden kirjoittamisen energiakustannus on tyypillisesti pieni suhteessa täyden uudelleenkäynnistyksen kustannukseen.

**Sovella datan säilytyskäytäntöjä ja elinkaaripolitiikkoja**
Tallennusenergia kasvaa datamäärän mukana: enemmän dataa tarkoittaa enemmän levylukuja, enemmän indeksien ylläpitoa sekä enemmän varmuuskopiointi- ja replikointiylikuormaa. Määrittele ja noudata säilytyskäytäntöjä, jotka poistavat datan, kun sitä ei enää tarvita operatiivisiin, analyyttisiin tai vaatimustenmukaisuustarkoituksiin. Siirrä kylmä data, kuten harvoin kyselty historiallinen tieto, pienitehoisempiin tallennuskerroksiin sen sijaan, että se pidetään suorituskykyisessä pääasiallisessa tallennuksessa.

**Sovita käsittelytaajuus todellisiin tarpeisiin**
Tunnin välein ajettavan putkilinjan suorittaminen tulosten tuottamiseksi, joita kulutetaan päivittäin, on tarpeetonta työtä. Tarkista kunkin putkilinjan aikataulutustiheys suhteessa alajuoksun kuluttajien todelliseen päätöksen viivevaatimukseen. Kun viive on hyväksyttävissä, suosi tapahtumaohjattuja käynnistimiä kiinteiden aikataulujen sijaan välttääksesi tyhjien tai minimaalisten lisäysten käsittelyn.

---

## 4.5 Backend – Infrastruktuuri ja pilvi

Infrastruktuurivalinnat määrittävät ohjelmiston ajamisen perusenergian ennen kuin yhtään pyyntöä on palveltu. Yliprovisioidut tai huonosti konfiguroidut infrastruktuurit tuhlaa energiaa jatkuvasti, ei pelkästään kuormituksen aikana.

**Mitoita laskentaresurssit oikein**
Instanssi, joka toimii 5 % keskimääräisellä CPU-käyttöasteella, kuluttaa suunnilleen saman verran joutokäyntitehoa kuin instanssi 60 % käyttöasteella, tuottaen samalla huomattavasti vähemmän hyödyllistä työtä per watti. Profiloi todellinen resurssienkäyttö edustavien kuormitusjaksojen aikana ja mitoita instanssit sen mukaisesti. Pilviympäristöt tekevät tästä palautettavan toimenpiteen, joten aloita konservatiivisesti ja skaalaa havaitun kysynnän eikä teoreettisten huippujen perusteella.

**Käytä nollaan skaalautuvia ja palvelimettomia malleja purskuvaisille kuormituksille**
Palvelut, jotka ovat joutokäynnillä merkittävän osan päivästä, hyötyvät arkkitehtuureista, jotka mahdollistavat laskentaresurssien skaalaamisen nollaan käyttämättömänä aikana. Palvelimettomat funktiot, konttipohjainen automaattinen skaalaus (esim. Knative, AWS Lambda, Google Cloud Run) ja tuotantoympäristöjen ulkopuolisten ympäristöjen ajoitettu sammuttaminen vähentävät kaikki energiankulutusta alhaisen kysynnän jaksoina.

**Harkitse käyttöönottoalueiden hiili-intensiteettiä**
Sama kuormitus tuottaa erilaiset elinkaarihiilidioksidipäästöt riippuen siitä, missä se ajetaan. Pääosin uusiutuvilla energialähteillä toimivilla pilvipalvelualueilla on huomattavasti alhaisempi hiili-intensiteetti kuin kivihiileen tai kaasuun nojaavilla alueilla. Kuormituksille, joissa viivevaatimukset sallivat maantieteellisen joustavuuden, kuten eräajoille, asynkroniselle käsittelylle tai dataputkille, suosi alueita, joilla on alhaisempi hiili-intensiteetti. Useimmat suuret pilvipalveluntarjoajat julkaisevat reaaliaikaisia tai historiallisia hiili-intensiteettitietoja alueidensa osalta.

**Pakota automaattinen skaalaus ja vältä pitkittynyttä joutilaana yliprovisiointia**
Kiinteäkapasiteettisiin käyttöönottoihin, jotka on mitoitettu huippukuormitusta varten, jää täysi kapasiteetti varattuna ja energiaa kuluttavana hiljaisina tunteina. Konfiguroi automaattiset skaalauskäytännöt, jotka vähentävät replikoiden määrää, instanssikokoa tai klusterin solmumäärää pitkittyneiden matalan liikenteen jaksojen aikana. Yhdistä tämä ajoitettuun skaalaukseen ennustettaville liikennemalleille (esim. yön yli skaalaaminen alas palveluille, joilla on tunnettu vuorokautinen käyttömalli).

---

## 4.6 Tekoälyn kehittäminen – Kouluttaminen

Tekoälyn koulutuksen energiajalanjälkeä dominoi kiihdyttimen (GPU/TPU) suoritusaika. Suuren mallin kouluttaminen voi kuluttaa satoja tai jopa tuhansia kilowattitunteja, mikä usein ylittää monen käyttöönoton elinikäisen päättelykäytön energian (Strubell et al., 2019; Patterson et al., 2021).

**Mitoita mallit ja aineistot oikein**
Suuremmat mallit ja aineistot eivät aina tuota suhteessa parempia tuloksia tiettyä tehtävää varten. Arvioi, täyttääkö pienempi, hienosäädetty tai distilloitu malli laatuvaatimukset ennen laajamittaisen koulutuksen aloittamista. Tarkasta myös koulutusaineistot duplikaattien ja huonolaatuisten näytteiden varalta, jotka lisäävät laskenta-aikaa parantamatta yleistämiskykyä.

**Vältä tarpeettomat kokeet**
Koulutusajot lähes identtisillä hyperparametreilla tai keskeytyneen kokeen uudelleenajaminen ilman tarkistuspisteen tallentamista ovat yleisiä vältettävissä olevan energiankulutuksen lähteitä. Käytä kokeenseurantatyökaluja (kuten MLflow tai Weights & Biases) konfiguraatioiden ja tulosten kirjaamiseen ja aseta konvergenskriteerit ennen ajon käynnistämistä eikä jälkikäteen arvioiden.

**Sovella varhaista pysäyttämistä ja tehokkaita koulutustekniikkoja**
Seuraa validointimittareita koulutuksen aikana ja pysähdy, kun parannus tasaantuu, sen sijaan että ajettaisiin kiinteä määrä epocheja. Sekatarkkuuskoulutus (FP16 tai BF16) vähentää muistikaistanleveyskulutusta ja laskenta-aikaa per vaihe tuetuilla laitteistoilla tyypillisesti ilman merkittävää laadunylikuormaa. Oppimistahdin aikataulutus, gradienttien akkumulointi ja tehokkaat huomio-toteutukset ovat edelleen vakiintuneita tekniikoita kokonaislaskennan vähentämiseen.

**Maksimoi laitteiston käyttöaste**
Matala GPU-käyttöaste koulutuksen aikana, joka johtuu yleisesti datan latauksen pullonkauloista, CPU:n esikäsittelystä tai tehottomasta erästyksen, tarkoittaa, että energiaa kulutetaan ilman tuottavaa työtä. Profiloi GPU-käyttöaste ja korjaa putkilinjan hidastukset esihakemalla dataa, lisäämällä datan latausohjainten määrää tai esikäsittelemällä ja tallentamalla syötteet välimuistiin offline-tilassa.

---

## 4.7 Tekoälyn käyttö – Päättely

Päättelyn energiajalanjälki kasvaa pyyntömäärän mukana. Toisin kuin koulutus, joka tapahtuu kerran malliversiota kohti, päättely toimii jatkuvasti tuotannossa ja sen kumulatiivinen energiakustannus ylittää usein koulutuskustannuksen mallin käyttöönottokauden aikana.

**Käytä pienintä tehokasta mallia**
Mallin monimutkaisuus tulisi sovittaa tehtävävaatimuksiin. Suuri yleiskäyttöinen malli, jota sovelletaan kapeaan, hyvin määriteltyyn tehtävään, on todennäköisesti merkittävästi ylimitoitettu. Tekniikat kuten kvantisointi (numeerisen tarkkuuden pienentäminen), karsinta (pieniarvoisien yhteyksien poistaminen) ja distillaatio (pienemmän mallin kouluttaminen replikoimaan suuremman mallin käyttäytymistä) voivat merkittävästi vähentää päättelyenergiaa ilman suhteellista häviötä tulostenlaadussa.

**Minimoi kehotteiden ja syötteiden koko**
Transformer-pohjaisissa malleissa itseohjautuvan huomion monimutkaisuus kasvaa neliöllisesti syötteen pituuden mukaan. Pidemmät kehotteet ja suuremmat kontekstiikkunat lisäävät suoraan pyyntökohtaista laskentakustannusta. Karsii epäolennainen konteksti, vältä monisanaisia kehotemallipohjia ja arvioi, voidaanko haetut kontekstikatkelmia lyhentää vaikuttamatta vastauslaatuu.

**Tallenna deterministiset tulosteet ja upotukset välimuistiin**
Identtiset tai semanttisesti vastaavat kyselyt, jotka tuottavat luotettavasti saman tulosteen, tulisi palvella välimuistista eikä käynnistää uudelleenpäättelyä. Kiinteiden viitedokumenttien upotuslaskelmat tulisi vastaavasti esihakea ja tallentaa välimuistiin eikä laskea uudelleen jokaisella pyynnöllä. Semanttinen välimuistitus, joka täsmää toistensa parafraaseja olevat kyselyt, voi ulottaa välimuistin osumataajuudet tarkkojen vastaavuuksien ulkopuolelle.

**Niputa pyynnöt mahdollisuuksien mukaan**
Erästys jakaa mallipainojen muistista lataamisen kiinteän kustannuksen ja mahdollistaa kiihdyttimen toimimisen korkeammalla käyttöasteella. Asynkronisille tai lähes reaaliaikaisille käyttötapauksille kerää pyynnöt eriin ja käsittele ne yhdessä. Dynaaminen erästys, joka ryhmittelee lyhyen aikavälin sisällä saapuvat pyynnöt, voi saavuttaa suurimman osan läpäisykykyhyödystä ilman merkittävää lisäviivettä.

---

## Viitteet

Manotas, I., Bird, C., Zhang, R., Shepherd, D., Jaspan, C., Sadowski, C., Pollock, L. and Clause, J. (2016) *An empirical study of practitioners' perspectives on green software engineering*. Proceedings of the 38th International Conference on Software Engineering (ICSE), pp.237–248.

Pathak, A., Hu, Y.C. and Zhang, M. (2012) *Where is the energy spent inside my app? Fine-grained energy accounting on smartphones with Eprof*. Proceedings of the 7th ACM European Conference on Computer Systems (EuroSys), pp.29–42.

Patterson, D., Gonzalez, J., Le, Q., Liang, C., Munguia, L.M., Rothchild, D., So, D., Texier, M. and Dean, J. (2021) *Carbon emissions and large neural network training*. arXiv preprint arXiv:2104.10350.

Pereira, R., Couto, M., Ribeiro, F., Rua, R., Cunha, J., Fernandes, J.P. and Saraiva, J. (2017) *Energy efficiency across programming languages: How do energy, time, and memory relate?* Proceedings of the 10th ACM SIGPLAN International Conference on Software Language Engineering, pp.256–267.

Strubell, E., Ganesh, A. and McCallum, A. (2019) *Energy and policy considerations for deep learning in NLP*. Proceedings of the 57th Annual Meeting of the Association for Computational Linguistics (ACL), pp.3645–3650.

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
