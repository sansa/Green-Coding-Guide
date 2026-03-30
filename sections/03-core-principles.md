---
title: "Keskeiset periaatteet"
nav_order: 3
permalink: /core-principles/
---

# 3. Vihreän koodauksen keskeiset periaatteet

Seuraavat periaatteet muodostavat teknologiariippumattoman perustan ohjelmistojärjestelmien suorituksenaikaisen energiajäljen pienentämiselle. Ne soveltuvat kaikille aloille ja abstraktiotasoille yksittäisistä funktioista hajautettuihin arkkitehtuureihin. Näitä periaatteita toteuttavat toimialakohtaiset käytännöt on kuvattu [luvussa 4](/domain-practices/).

## 3.1 Vältä tarpeetonta työtä

Tehokkain tapa vähentää energiankulutusta on välttää laskentaa, tietojenkäsittelyä ja viestintää, joilla ei ole toiminnallista arvoa. Tähän kuuluvat mm. logiikka, joka suoritetaan jokaisen pyynnön yhteydessä mutta on merkityksellinen vain harvoin, data, joka haetaan mutta jota ei koskaan käytetä, sekä operaatiot, jotka käynnistyvät tapahtumista, jotka eivät niitä edellytä.

Tarpeeton työ on usein näkymätöntä, koska se jakautuu moniin pieniin operaatioihin, joista kukin vaikuttaa yksittäin harmittomalta. Kumulatiivinen vaikutus useiden käyttäjien, pyyntöjen tai laitteiden välillä voi kuitenkin olla huomattava. Sen tunnistaminen edellyttää profilointia pelkän tarkastelun sijaan, sillä kallein koodi ei ole harvoin ilmeisin.

Tämä periaate koskee myös arkkitehtuuritasoa. Käsittely, jota voitaisiin välttää paremmalla järjestelmäsuunnittelulla — kuten saman tuloksen toistuvalla uudelleenlaskemisella tietokannasta — edustaa tarpeetonta työtä riippumatta siitä, kuinka tehokkaasti se suoritetaan.

## 3.2 Vältä toistuvaa työtä

Saman työn toistuva suorittaminen kasvattaa kumulatiivista energiajälkeä lisäämättä toiminnallista arvoa. Uudelleenkäyttö, välimuistitus ja muistiointi voivat merkittävästi vähentää redundanttia resurssien käyttöä, kun niitä sovelletaan harkitusti ja tuloksia mitataan huolellisesti. Empiirinen arviointi API-suunnitteluvarianteista on osoittanut, että muistinsisäisen välimuistituken käyttöönotto toistuviin kyselyihin voi vähentää tehonkulutusta noin 15 %, kun taas tehottoman algoritmisen logiikan korvaaminen optimoiduilla vastineilla voi tuottaa jopa 23 %:n säästöjä (Joof et al., 2025; Joof, 2025).

Oleellinen varaus on *harkitusti ja mitaten*. Välimuistitus ei ole yleisesti hyödyllistä: se kuluttaa muistia, aiheuttaa vanhentuneen datan riskin, ja säästää energiaa vain silloin, kun osumataajuus on riittävän korkea kattaakseen lisäkuorman. Sama koskee esikäsittelyä, joka siirtää työn pyyntöajasta rakennus- tai käynnistysaikaan. Tämä saattaa vähentää pyyntikohtaista energiaa, mutta lisää järjestelmätason energiaa, jos esikäsitellyt tulokset ovat harvoin käytössä.

Toistuva työ on erityisen yleinen ongelma hajautetuissa järjestelmissä, joissa useat komponentit hakevat saman datan itsenäisesti, sekä frontend-sovelluksissa, joissa komponentit renderöityvät uudelleen tilaan muutoksiin vastatessa, vaikka muutos ei vaikuta niihin.

## 3.3 Siirrä ja tallenna vähemmän dataa

Datan siirto ja tallennus kuluttavat merkittävästi energiaa erityisesti hajautetuissa järjestelmissä ja mobiiliympäristöissä. Datan siirtäminen verkon yli, levylle kirjoittaminen ja sieltä lukeminen sekä datan kopiointi muistialueiden välillä kuluttavat energiaa siirrettyyn volyymiin suhteutettuna.

Hyötykuormien koon pienentäminen, redundantin serialisoinnin ja deserialisoinnin poistaminen, datan pakkaaminen silloin kun pakkauksen CPU-kustannus on pienempi kuin pienemmästä siirrosta saatava energiasäästö, sekä tarpeettoman pysyvyistalletuksen välttäminen ovat kaikki tämän periaatteen käytännön ilmentymiä.

Periaate soveltuu kaikissa mittakaavoissa: tietokantakyselyssä valittujen sarakkeiden määrän vähentäminen, API:n palauttamien kenttien karsiminen, lokitulosteen suodattaminen ja käyttämättömien riippuvuuksien poistaminen ohjelmistoniputuksesta ovat kaikki esimerkkejä vähemmän datan siirtämisestä ja tallentamisesta.

## 3.4 Suosi tehokkaita abstraktioita

Abstraktiot parantavat ylläpidettävyyttä, mutta voivat piilottaa kalliita operaatioita. Kätevä metodikutsu, korkean tason kehysominaisuus tai laajasti käytetty kirjasto saattaa peittää merkittävän laskennallisen lisäkuorman, erityisesti kun sitä käytetään kuumassa polulla, suuressa mittakaavassa tai resurssirajoitteisessa ympäristössä.

Vihreä koodaus kannustaa tietoisuuteen käytössä olevien abstraktioiden resurssi- ja energiavaikutuksista. Tämä ei tarkoita abstraktioista luopumista matalan tason koodin hyväksi, sillä useimmissa tapauksissa siitä aiheutuva ylläpidettävyyden kustannus ylittää selvästi energiahyödyn. Se tarkoittaa harkitsevuutta: ymmärrystä siitä, mitä abstraktio tekee kulissien takana, kevyempien vaihtoehtojen valitsemista silloin kun ne ovat toiminnallisesti vastaavia, ja profilointia sen sijaan, että oletetaan tutun työkalun olevan tehokas.

Tämä periaate on erityisen merkityksellinen kehyksiä, suoritusympäristöjä, serialisointiformaatteja ja viestintäprotokollia valittaessa, sillä valinta vaikuttaa koko järjestelmään yksittäisen operaation sijaan.

## 3.5 Tee energiajälki näkyväksi

Ilman mittausta energiatehokkuus jää spekulatiiviseksi. Kehittäjät eivät pysty luotettavasti tunnistamaan järjestelmän energiaintensiivisimpiä osia pelkän tarkastelun avulla, eikä energiaparannuksia voida todentaa ilman vertailuperustaa.

Näkyvyys profiloinnin ja vertailun kautta on välttämätöntä näyttöön perustuvalle vihreälle koodaukselle. Tämä tarkoittaa energia- tai resurssilähtötasojen asettamista ennen optimointia, muutosten vaikutuksen mittaamista oletusten sijaan, sekä energiaan liittyvän tiedon saattamista tiimin saataville koontinäyttöjen, CI-mittareiden tai koodikatselmusartefaktien kautta, jotta se voi ohjata päätöksiä pitkällä aikavälillä.

Näkyvyys tarkoittaa myös epävarmuuden myöntämistä. Energianmittaustyökaluilla on rajoituksensa (ks. [luku 7](/tradeoffs/)), ja tuloksia tulisi tulkita vertailevasti ja läpinäkyvästi eikä täsmällisinä absoluuttisina arvoina. Prototyypitutkimus, jossa energiaprofilointi integroitiin suoraan versionhallinnan työnkulkuihin, on osoittanut, että commit-tason energiapalaute lisää kehittäjien tietoisuutta kestävyydestä ja vaikuttaa suunnittelupäätöksiin — jopa niiden kehittäjien kohdalla, jotka eivät aiemmin olleet pitäneet energiaa laatutekijänä (Joof et al., 2025; Joof, 2025).

---

## Viitteet

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
