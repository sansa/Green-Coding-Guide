---
title: "Kompromissit ja rajoitukset"
nav_order: 7
permalink: /tradeoffs/
---

# 7. Kompromissit, rajoitukset ja suunnittelujännitteet

Vihreä koodaus edellyttää energiajäljen tasapainottamista suhteessa muihin ohjelmiston laatuominaisuuksiin. Tässä kuvatut jännitteet ovat todellisia, eikä niitä voida ratkaista pelkillä nyrkkisäännöillä. Jokainen niistä vaatii empiiristä arviointia kussakin asiayhteydessä.

## 7.1 CPU vs. muisti

CPU-työn vähentäminen lisää usein muistinkäyttöä. Tulosten ennakkolaskenta ja välimuistiin tallentaminen esimerkiksi välttää toistuvan laskennan heap-tilan kustannuksella. Nettovaikutus energiankulutukseen riippuu käyttömallista: muistia on suhteellisen edullista *pitää varattuna*, mutta kallista *allokoida, kopioida ja kerätä roskana* suurella tahdilla.

**Päätösheuristiikka:** Suosi muisti-intensiivistä vaihtoehtoa, kun dataan viitataan toistuvasti rajatun aikajakson sisällä ja välimuistin osumataajuus on osoitetusti korkea. Suosi laskenta-intensiivistä vaihtoehtoa, kun viittaukset ovat harvoja, datan volyymi tekee välimuistiin tallentamisesta epätaloudellista tai välimuistissa olevien objektien aiheuttama GC-paine ylittää laskennan säästöt. Mittaa molemmat lähestymistavat edustavan kuorman alla ennen päätöksentekoa.

## 7.2 Verkko vs. laskenta

Tiedonsiirron vähentäminen voi vaatia lisälaskentaa — esimerkiksi hyötykuorman pakkaamista ennen lähetystä tai datan esikoostamista palvelinpuolella raakatietueiden siirtämisen sijaan. Toisaalta laskennan ohittaminen datan nopeampaa lähettämistä varten pitää verkon aktiivisena kauemmin ja voi lisätä laitteen puoleisia käsittelykustannuksia.

**Päätösheuristiikka:** Kun käyttäjän kokema viive ei ole rajoittava tekijä, suosi siirrettävän datamäärän vähentämistä. Kun viive on kriittinen, vertaile, palautuuko lisäkäsittelyn (pakkaus, koostaminen) energiakustannus lyhentyneen lähetysajan kautta. Huomaa, että verkon energiakustannukset vaihtelevat merkittävästi langallisen, Wi-Fi- ja mobiiliradioympäristöjen välillä. Tämä kompromissi on havaittu empiirisesti API-tason profiloinnissa: GZIP-pakkauksen lisääminen API-vastauksiin pienensi verkon hyötykuormaa mutta kasvatti CPU-kuormaa verrattuna välimuistiratkaisuun, mikä johti korkeampaan tehonkulutukseen (250 mW vs. 240 mW) siirtokoon pienenemisestä huolimatta — tämä osoittaa, ettei pakkaaminen ole ehdottomasti hyödyllistä (Joof et al., 2025; Joof, 2025).

## 7.3 Tehokkuus vs. ylläpidettävyys

Voimakkaasti optimoitu koodi on usein vaikeampi ymmärtää, testata ja muuttaa. Energiaoptimointi, joka kytkee komponentit tiukasti yhteen, poistaa hyödyllisiä abstraktioita tai nojaa dokumentoimattomaan laitteistokäyttäytymiseen, voi pienentää energiajälkeä nyt mutta kasvattaa sitä myöhemmin vikojen ja uudelleentyöstämisen kautta.

**Päätösheuristiikka:** Sovella ensin energiaperusteisia optimointeja, jotka eivät merkittävästi lisää koodin monimutkaisuutta. Kun invasiivisempi optimointi on välttämätön, dokumentoi säilytettävä suorituskykyominaisuus, eristä optimoitu koodi selkeärajaiseen rajapintaan ja lisää vertailutestit, jotka paljastavat regressiot, jos optimointi poistetaan tai ohitetaan myöhemmin.

## 7.4 Paikalliset vs. järjestelmänlaajuiset vaikutukset

Yksittäisen komponentin optimointi eristyksissä voi siirtää energiankulutusta muualle järjestelmässä sen sijaan, että se poistaisi kulutuksen kokonaan. Nopeampi asiakaspuolen operaatio voi tuottaa enemmän palvelinpyyntöjä. Tehokkaampi tietokantakysely voi lisätä sovelluskerroksen käsittelyä. Pienennetty verkon hyötykuorma voi vaatia enemmän CPU:ta vastaanottavalla päällä.

**Päätösheuristiikka:** Jäljitä ennen komponentin optimointia tietovuo järjestelmän läpi alaspäin suuntautuvien vaikutusten tunnistamiseksi. Priorisoi optimointeja, jotka vähentävät koko järjestelmän energiankulutusta, eivät vain paikallisia mittareita. Kun järjestelmänlaajuinen mittaus ei ole käytännöllistä, varmista vähintään, etteivät ylä- ja alajuoksun komponentit kuormitu haitallisesti muutoksen seurauksena.

## 7.5 Mittausepävarmuus

Energianmittaukseen liittyy luontaisia rajoituksia. Ohjelmistosta luettavat laskurit (kuten Intel RAPL) mittaavat prosessori- ja DRAM-alijärjestelmiä, mutta jättävät ulkopuolelle tallennusmedian, verkon ja oheislaitteet. Pilvi- ja virtualisointiympäristöt eivät usein tarjoa lainkaan pääsyä laitteiston energialaskureihin. Tulokset vaihtelevat CPU-taajuudensäädön, lämpörajoittamisen, taustakuorman ja mittaustarkkuuden mukaan.

**Päätösheuristiikka:** Raportoi suhteelliset parannukset lähtötason ja ehdokkaan välillä identtisissä olosuhteissa absoluuttisten energiaarvojen sijaan. Toista mittaukset useilla ajoilla ja mahdollisuuksien mukaan eri laitteistokokoonpanoilla ennen johtopäätösten tekemistä. Ilmoita mittausolosuhteet eksplisiittisesti: käytetty työkalu, järjestelmän tila, kuorma ja toistojen lukumäärä. Pidä tulosta alustavana, kunnes se on validoitu edustavan tuotantoympäristön olosuhteissa.

## 7.6 Vähenevät tuotot

Kun merkittävimmät tehottomuudet — kuten turha laskenta, tarpeeton tiedonsiirto ja resurssien joutokäynti — on poistettu, lisäoptimointi tuottaa asteittain pienempiä energiasäästöjä kasvavilla toteutuskustannuksilla. Ensimmäinen 20 % optimointipanostuksesta kattaa usein 80 % saavutettavissa olevasta parannuksesta.

**Päätösheuristiikka:** Priorisoi toimenpiteet arvioidun energiavaikutuksen mukaan suhteessa toteutusvaivaan. Käytä profilointia energiajäljen pääasiallisten tekijöiden tunnistamiseen ennen optimointiin panostamista. Lopeta komponentin optimointi, kun lisäparannukset edellyttävät kohtuutonta monimutkaisuutta tai riskiä, ja suuntaa panostus mittauksen perusteella tunnistettuihin suuremman vaikutuksen alueisiin.

---

## Lähteet

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
