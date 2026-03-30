---
title: "Tarkistuslistat"
nav_order: 10
permalink: /checklists/
---

# 10. Vihreän koodauksen tarkistuslistat

Nämä tarkistuslistat tukevat pohdintaa ja keskustelua katselmointien, suunnitteluarviointien tai retrospektiivien yhteydessä — ne eivät ole vaatimustenmukaisuustarkastuksia. Käytä niitä apuvälineinä energiaan liittyvien riskien tunnistamiseen, ei hyväksymis- tai hylkäämiskriteereinä.

## 10.1 Frontend – Web
- Onko uudelleenrenderöinti rajattu vain todella muuttuneisiin komponentteihin?
- Onko bundle-koko mitattu hiljattain, ja onko se perusteltu?
- Haetaanko dataa vain tarvittaessa, eikä samaa sisältöä haeta useampaan kertaan?
- Tarjoillaanko kuvat ja media sopivassa resoluutiossa ja moderneissa formaateissa (WebP, AVIF)?
- Onko kolmannen osapuolen skriptien tarpeellisuus ja latausvaikutus arvioitu?
- Suositaanko CSS-siirtymiä tai Web Animations API:a JavaScript-animaatiosilmukoiden sijaan?
- Onko polling- tai intervallipohjaiset mallit korvattu tapahtumaohjatulla tai push-pohjaisella lähestymistavalla, missä se on mahdollista?
- Onko HTTP-välimuisti määritetty oikein staattisille ja puolistaattisille resursseille?

## 10.2 Frontend – Mobiili
- Onko taustatehtävät minimoitu, konsolidoitu ja siirretty järjestelmän ajoittamiin ikkunoihin?
- Poistetaanko sensorien ja herätysvastaanotinten rekisteröinnit, kun niitä ei aktiivisesti tarvita?
- Tuetaanko offline-käyttäytymistä tarpeettomien verkon edestakaisin-pyyntöjen välttämiseksi?
- Niputetaanko verkkopyynnöt niin, että radio voi sammua aktiivisten jaksojen välillä?
- Onko renderöinti rajattu näkyvään sisältöön näkymäkierrätystä tai laiskoja listamalleja käyttäen?
- Onko paikannustarkkuus sovitettu todellisiin vaatimuksiin (karkea vs. tarkka)?
- Käytetäänkö push-ilmoituksia tilaan liittyvässä viestinnässä pollingin sijaan?
- Onko animaatiot ja siirtymät toteutettu laitteistokiihdytettyjä API:ja käyttäen?

## 10.3 Backend – Palvelut
- Onko pyyntökohtainen työ profiloitu, ja ovatko kuumat polut vapaita vältettävistä operaatioista?
- Ovatko tietokantakyselyt valikoivia, indeksoituja ja sivutettuja koko taulujen hakemisen sijaan?
- Onko N+1-kyselymallit poistettu innokkaalla latauksella tai niputetuilla kyselyillä?
- Onko hyötykuorman koko minimoitu, ja palautetaanko vain tarvittavat kentät?
- Ovatko välimuistit rajoitettuja, ja mitattiinko osumataajuuksia niiden muistikustannuksen oikeuttamiseksi?
- Onko kriittiseltä polulta siirretty pois synkroniset operaatiot, jotka voidaan turvallisesti lykätä?
- Ovatko joutilaiden yhteyksien, säiealtaiden ja suorittimien rajat asetettu resurssien loppumisen estämiseksi alhaisella kuormalla?
- Onko serialisointiformaatti sopiva käyttötapaukseen (esim. binäärimuodot sisäisille suuritehokkuuksisille poluille)?

## 10.4 Dataputket
- Vältetäänkö muuttumattomien osioiden uudelleenlaskenta tarkistussummien, aikaleimamerkintöjen tai inkrementaalisten kehysten avulla?
- Käytetäänkö tarkistuspisteitä, jotta epäonnistuneet ajot voivat jatkua alusta aloittamisen sijaan?
- Onko käsittelytaajuus sovitettu todelliseen alajuoksun kulutusnopeukseen?
- Onko kylmä tai harvoin käytetty data siirretty vähäenergisempään tallennuskerrokseen?
- Onko datan säilyttämistä ohjattu määritellyllä elinkaaripolitiikalla, joka estää rajoittamattoman kasvun?
- Onko putken vaiheet profiloitu sen vaiheen tunnistamiseksi, joka hallitsee energiaa ja suoritusaikaa?
- Onko rinnakkaisuus sovitettu käytettävissä olevaan laitteistoon, välttäen sekä ali- että yliprovisiointia?

## 10.5 Tekoälyn kouluttaminen
- Onko mallin kapasiteetti perusteltu tehtävän perusteella, vai täyttäisikö pienempi arkkitehtuuri vaatimukset?
- Onko kokeenseuranta käytössä kaksoiskappaleiden tai lähes identtisten koulutusajojen estämiseksi?
- Onko GPU/TPU-käyttöaste jatkuvasti korkea, vai onko datan lataus- tai esikäsittelypullon kauloja?
- Sovelletaanko varhaista pysäyttämistä, kun validointimittarit tasaantuvat?
- Käytetäänkö sekatalkkitarkkuuskoulutusta (FP16/BF16), missä laitteisto ja kehys tukevat sitä?
- Onko aineistot suodatettu tai deduplikoitu mallilaatuun vaikuttamattomien näytteiden poistamiseksi?
- Onko hyperparametrien haut rajattu ja pysäytetään, kun vähenevät tuotot ovat ilmeisiä?

## 10.6 Tekoälyn päättely
- Käytetäänkö tuotannossa pienintä mallia, joka täyttää laatuvaatimukset?
- Ovatko syötteet ja kehotteet mahdollisimman tiiviit, ja onko turha konteksti poistettu?
- Sovelletaanko välimuistia deterministisiin tai toistuviin kyselyihin tarpeettoman päättelyn välttämiseksi?
- Niputetaanko pyynnöt, missä viivevaatimukset sen sallivat?
- Onko mallin kvantisointia arvioitu pyyntökohtaisen laskennan vähentämiskeinona?
- Seurattiinko päättelypisteen käyttöastetta, ja säädetäänkö kapasiteettia pitkittyneen joutokäynnin välttämiseksi?
- Onko upotuslaskelmat tallennettu välimuistiin sen sijaan, että ne lasketaan uudelleen jokaisella pyynnöllä?
