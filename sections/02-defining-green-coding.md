---
title: "Vihreän koodauksen määritelmä"
nav_order: 2
permalink: /defining-green-coding/
---

# 2. Vihreän koodauksen määritelmä

## 2.1 Mitä on vihreä koodaus?

Termiä *vihreä koodaus* käytetään sekä akateemisissa että teollisuuden yhteyksissä kuvaamaan ohjelmistokehityskäytäntöjä, joiden tavoitteena on vähentää ohjelmistojärjestelmien ympäristövaikutuksia. Kirjallisuus kuitenkin osoittaa, että vihreälle koodaukselle ei ole **yhtä, yleisesti hyväksyttyä määritelmää**. Sen sijaan käsite esiintyy siihen läheisesti liittyvien termien alla, kuten *vihreä ohjelmisto*, *energiatehokas ohjelmisto* ja *kestävä ohjelmistotuotanto* (Hilty and Aebischer, 2015; Procaccianti et al., 2014).

Useat edustavat määritelmät havainnollistavat näkökulmien kirjoa:

- *Vihreä ja kestävä ohjelmisto* määritellään usein ohjelmistoksi, joka "minimoi kielteiset ympäristövaikutukset koko elinkaarensa aikana" (Penzenstadler et al., 2014).
- *Energiatehokas ohjelmisto* kuvataan yleisesti ohjelmistoksi, joka "vähentää energiankulutusta suorituksen aikana säilyttäen samalla hyväksyttävän suorituskyvyn" (Procaccianti et al., 2014).
- *Kestävä ohjelmistotuotanto* on määritelty tieteenalaksi, jossa ohjelmistojärjestelmiä suunnitellaan ottaen eksplisiittisesti huomioon pitkän aikavälin ympäristö-, sosiaaliset ja taloudelliset vaikutukset (Becker et al., 2016).

Nämä määritelmät eroavat toisistaan ensisijaisesti **laajuudeltaan**. Osa omaksuu laajan elinkaarinäkökulman, joka kattaa kehityksen, käyttöönoton, käytön ja käytöstäpoiston, kun taas toiset keskittyvät kapeammin **suorituksenaikaiseen käyttäytymiseen**. Tämän oppaan ensisijainen kohderyhmä koostuu ohjelmistokehittäjistä ja -arkkitehdeistä, jotka tekevät konkreettisia suunnittelu- ja toteutuspäätöksiä; tässä yhteydessä suoritukseen keskittyvä määritelmä tarjoaa suurimman käytännöllisen ja empiirisen arvon.

Tässä oppaassa käytetään seuraavaa työstöä helpottavaa määritelmää:

> **Vihreä koodaus on ohjelmiston suunnittelua ja toteutusta tavalla, joka välttää tarpeetonta suorituksenaikaista energian- ja resurssienkulutusta samalla kun toiminnalliset ja laadulliset vaatimukset täytetään.**

Tämä määritelmä korostaa vihreän koodauksen useita tärkeitä ominaisuuksia:

- Ohjelmiston energiajalanjälki on ensisijaisesti **suorituksenaikainen ominaisuus**, joka ilmenee ajon aikana eikä pelkästään kehitysvaiheessa.
- Energiankulutus on **kuormitusriippuvaista** ja vaihtelee syötedatan, käyttömallien ja käyttöönottokontekstin mukaan.
- Energiajalanjälkeä muovaavat **ohjelmistopäätökset**, mukaan lukien algoritmit, tietorakenteet, arkkitehtuurit, vuorovaikutusmallit ja konfigurointivalinnat.
- Energiaan liittyvät vaikutukset ovat **havaittavissa mittaamalla tai arvioimalla**, mikä mahdollistaa empiirisen arvioinnin intuition sijaan.

Keskittymällä suorituksenaikaiseen energiankulutukseen tämä määritelmä yhdistää vihreän koodauksen vakiintuneisiin ohjelmiston laadullisiin huolenaiheisiin, kuten suorituskykyyn ja skaalautuvuuteen, samalla kun se on yhteensopiva laajempien kestävyysnäkökulmien kanssa, jotka ottavat huomioon järjestelmätason ja yhteiskunnalliset vaikutukset. Vihreä koodaus soveltuu siksi useille abstraktiotasoille yksittäisistä funktioista ja metodeista kokonaisiin ohjelmistoarkkitehtuureihin.


## 2.2 Mitä vihreä koodaus *ei* ole

On olennaista selventää, mitä vihreä koodaus *ei* ole, jotta voidaan välttää yleisiä väärinkäsityksiä, jotka voivat rajoittaa sen tehokkuutta tai johtaa harhautuneisiin optimointipyrkimyksiin.

Ensinnäkin vihreä koodaus **ei ole synonyymi suorituskyvyn optimoinnille**. Vaikka suoritusaika ja energiankulutus korreloivat usein keskenään, ne eivät ole toistensa vastineita. Nopeampi suoritus voi kasvattaa tehonkulutusta CPU-prosessorien tai kiihdyttimien korkeamman käyttöasteen vuoksi, mikä joissain tapauksissa johtaa suurempaan kokonaisenergiankulutukseen (Hackenberg et al., 2015). Vastaavasti hieman hitaampi suoritus saattaa vähentää energiankulutusta sallimalla laitteistokomponenttien toimia energiatehokkaammissa tiloissa. Suorituskykymittarit yksinään ovat siksi riittämättömiä energiatehokkuuden välillisiksi mittareiksi.

Toiseksi vihreä koodaus **ei ole kertaluonteinen toimenpide**. Ohjelmistojärjestelmät kehittyvät jatkuvasti ominaisuuslisäysten, refaktoroinnin, riippuvuuspäivitysten ja konfigurointimuutosten myötä. Jokainen näistä muutoksista voi aiheuttaa energiaregressioita, vaikka toiminnallinen käyttäytyminen pysyisi muuttumattomana. Tästä näkökulmasta vihreä koodaus on **jatkuva huolenaihe**, joka on verrattavissa suorituskyvyn virittämiseen tai tietoturvan vahvistamiseen, eikä yksittäinen optimointivaihe.

Kolmanneksi vihreä koodaus **ei tarkoita ennenaikaista mikrooptimointia**. Eristyksissä tehdyt matalan tason optimoinnit ilman empiiristä validointia lisäävät usein koodin monimutkaisuutta tuottamatta merkityksellisiä vähennyksiä energiajalanjäljessä. Kestävän ohjelmistotuotannon empiiriset tutkimukset korostavat, että merkittävimmät energiasäästöt syntyvät tyypillisesti korkean tason suunnittelu- ja arkkitehtuuripäätöksistä, jotka on validoitu mittaamalla (Procaccianti et al., 2016).

Lopuksi vihreä koodaus ei tarkoita oikeellisuuden, ylläpidettävyyden tai muiden laadullisten ominaisuuksien oletusarvoista uhraamista. Sen sijaan se edistää **eksplisiittisiä ja harkittuja kompromisseja**, joissa energiatehokkuus otetaan huomioon toiminnallisten ja ei-toiminnallisten vaatimusten rinnalla. Tämä näkökulma asettaa vihreän koodauksen vastuullisen ohjelmistotuotannon olennaiseksi osaksi pikemminkin kuin valinnaiseksi tai toissijaiseksi huolenaiheeksi.



## Viitteet

Becker, C., Betz, S., Chitchyan, R., Duboc, L., Easterbrook, S.M., Penzenstadler, B., Seyff, N. and Venters, C.C. (2016) *Requirements: The key to sustainability*. IEEE Software, 33(1), pp.56–65.

Hackenberg, D., Schöne, R., Ilsche, T., Molka, D., Schuchart, J. and Geyer, R. (2015) *An energy efficiency feature survey of the Intel Haswell processor*. IEEE International Parallel and Distributed Processing Symposium Workshops, pp.896–904.

Hilty, L.M. and Aebischer, B. (2015) *ICT for sustainability: An emerging research field*. In: Hilty, L.M. and Aebischer, B. (eds.) ICT Innovations for Sustainability. Springer, pp.3–36.

Penzenstadler, B., Bauer, V., Calero, C. and Franch, X. (2014) *Sustainability in software engineering: A systematic literature review*. Proceedings of the 16th International Conference on Evaluation & Assessment in Software Engineering (EASE), pp.1–10.

Procaccianti, G., Lago, P. and Vetro', A. (2014) *Energy efficiency in software: A systematic literature review*. Journal of Systems and Software, 97, pp.135–155.

Procaccianti, G., Lago, P. and Lewis, G. (2016) *Green architectural tactics for the cloud*. Proceedings of the 2016 IEEE/ACM International Conference on Software Architecture (ICSA), pp.41–50.
