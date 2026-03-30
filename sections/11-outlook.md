---
title: "Tulevaisuudennäkymät"
nav_order: 11
permalink: /outlook/
---

# 11. Tulevaisuudennäkymät ja kehityssuunnat

Vihreä koodaus on kehittyvä tieteenala. Tässä oppaassa kuvatut periaatteet ja käytännöt edustavat näyttöön perustuvan ymmärryksen nykytilaa, mutta useita merkittäviä puutteita on edelleen — työkaluissa, koulutuksessa, organisatorisessa käytännössä ja tutkimuksessa — ja ne tulevat muokkaamaan alan kehitystä.

**Paremmat mittaustyökalut.** Useimmat olemassa olevat energiaprofiloijat on suunniteltu järjestelmätason tai prosessitason analyysiin, eikä niillä ole integraatiota kehittäjien päivittäin käyttämiin työkaluihin. Kevyt, työnkulkuihin integroitu profilointi, joka yhdistää energiamittaukset versionhallinnan committeihin, konteistettuihin testiympäristöihin ja visuaalisiin koontinäyttöihin, on aktiivisesti kehittymässä oleva suunta. Prototyypitutkimus on osoittanut commit-tietoisen, toistettavan API-energiaprofiloinnin teknisen toteutettavuuden saavutettavalla laitteistolla, ja jatkotyö tähtää pilvipohjaisiin CI/CD-putkistoihin, usean profiloijan orkestrointiin ja hiili-intensiteettikartoitukseen (Joof et al., 2025; Joof, 2025).

**Koulutus ja osaamisen kehittäminen.** Kehittäjät eivät usein tiedosta, miten heidän suunnittelu- ja toteutuspäätöksensä vaikuttavat energiankulutukseen — ei siksi, että kiinnostusta puuttuisi, vaan koska energia ei ole kuulunut osaksi tavanomaista ohjelmistotekniikan koulutusta tai työkalujen palautesilmukoita. Energiatietoisuuden upottaminen opetussuunnitelmiin, koodikatselmoinnin käytäntöihin ja IDE-työkaluihin on välttämätöntä, jotta vihreä koodaus siirtyisi erityisasiantuntijoiden käytännöstä yleiseksi insinöörityön normiksi.

**Organisatorinen yhtenäistäminen.** Tekniset työkalut ovat tehokkaita vain silloin, kun organisaatiot kohtelevat energiatehokkuutta ensisijaisena laatutekijänä — suorituskyvyn, luotettavuuden ja ylläpidettävyyden rinnalla. Tämä edellyttää määriteltyjä kestävyystavoitteita, joita tukevat mittarit ja jotka on integroitu projektisuunnitteluun eikä kohdeltu valinnaisina tai jälkikäteisinä.

**Tutkimus–teollisuus-yhteistyö.** Vihreän koodauksen merkittävimmät avoimet kysymykset — mukaan lukien se, miten energiaa voidaan mitata kohdistettavasti hajautetuissa ja pilvinatiiveissa järjestelmissä, miten koko järjestelmän laajuisista kompromisseista voidaan päätellä, ja miten vihreän koodauksen käytäntöjen pitkäaikainen tehokkuus voidaan validoida suuressa mittakaavassa — edellyttävät jatkuvaa yhteistyötä tutkijoiden ja käytännön ammattilaisten välillä. Aloitteet, jotka ankkuroivat tutkimuksen todellisiin kehitysympäristöihin ja tuottavat artefakteja, joita ammattilaiset voivat omaksua, ovat välttämättömiä akateemisten löydösten ja soveltavan käytännön välisen kuilun umpeen kuromiseksi.

Kohtelemalla energiajälkeä ensisijaisena ohjelmiston laatuattribuuttina vihreä koodaus mahdollistaa paremmat päätökset — ei täydellisyyttä, vaan jatkuvaa näyttöön perustuvaa parantamista. Tarvittavat työkalut, käytännöt ja organisatoriset olosuhteet tämän parantamisen ylläpitämiseksi ovat saavutettavissa, ja tämä opas on tarkoitettu panokseksi niiden kehittämiseen.

---

## Viitteet

Joof, M.B. (2025) *Green Coding in Practice: A Software Framework for API Energy Efficiency Measurement and Feedback*. Master's thesis. Lappeenranta–Lahti University of Technology LUT.

Joof, M.B., Khan, M.A., Oyedeji, S. and Porras, J. (2025) *Integrating API Energy Profiling into Developer Workflows: The VerdeFlow Prototype*. In Proceedings of the IEEE/ACM International Conference on Software Engineering: Workshop on Green and Sustainable Software (ICSE-GREENS). ACM.
