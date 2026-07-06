# Viikkolista v2

Mobiilikäyttöinen viikkoruokalista- ja ostoslistasovellus. Toimii GitHub Pagesissa ilman palvelinta — kaikki data tallentuu selaimen localStorageen.

**Live:** https://rrba-fin.github.io/Meal-Planner/

## Mitä sovellus tekee

- Arpoo viikon ruokalistan: pääruuat 2–3 päivän satseina (yksi kokkaus, monta päivää), aamu- ja iltapalat päiväkohtaisesti.
- Sovittaa päivät kaloritavoitteeseen (1500 / 2000 / 2500 / 3000 kcal/pv, toleranssi ±300 kcal).
- Koostaa ostoslistan koko viikosta, maustekaappitavarat omassa osiossaan.
- Reseptipankki suodattimilla: gluteeniton, maidoton, VHH ja punaisen lihan rajoitus.

## v2-muutokset

- **Kaloritavoite asetuksissa** (1500/2000/2500/3000 kcal/pv, toleranssi ±300). Annoskoot, kalorit ja ostoslista skaalautuvat tavoitteeseen annoskertoimella (tavoite ÷ 1750, esim. 2500 kcal → ×1,43) — näin isotkin tavoitteet täyttyvät ilman välipalakikkailua. Arvonta tasapainottaa viikkoa: jos yksi päivä jää kevyeksi, seuraavia ohjataan raskaammiksi niin että viikkokeskiarvo pysyy tavoitteessa.
- **Lautasmalli reseptikortin lopussa** (lounas/päivällinen): komponenttiruuille ½ kasviksia + ¼ proteiinia + ¼ lisäkettä, sekoiteruuille (keitot, padat, pastat...) ⅔ ruokaa + ⅓ kasviksia. Proteiini- ja lisäkemäärät skaalautuvat kaloritavoitteen mukaan.
- **Viikonloppupainotus:** lauantain–sunnuntain satsiin arvonta suosii herkkuruokia (pitsa, tacot, burgerit, kiusaukset...) ja sallii työläämmät reseptit; arkena painottuvat nopeat ruuat.
- **Punainen liha -asetus:** Saa olla / Vähän (korkeintaan yksi punaisen lihan pääruokasatsi viikossa) / Ei ollenkaan (poistaa myös kinkun ja pekonin sisältävät reseptit).
- **Aamu- ja iltapalat päiväkohtaisia.** Ei satseja, ei 28 päivän vaihteluhistoriaa — sama aamupala saa toistua vaikka joka päivä. Vaihteluhistoria koskee vain pääruokia.
- **Manuaalivalinta:** jokaisen aterian ⟳-arvontanapin vieressä on ☰-nappi, josta aukeaa kategorian reseptilista sopivuusjärjestyksessä. Jokaisen vaihtoehdon kohdalla näkyy, mihin päivän kalorit asettuvat valinnalla: **punainen** = yli tavoitteen, **sininen** = toleranssissa (±300), **vihreä** = alle tavoitteen. Sama värikoodi näkyy päiväkortin summassa ja käsin valittujen aterioiden kaloreissa.
- **Välipala** on yksi vapaaehtoinen valinta per päivä — sen kalorivaikutus näkyy värikoodilla jo valintalistassa.
- **Valmisruuat korvausvaihtoehtona:** 13 proteiinipitoista kaupan valmisateriaa (broileriwokki, lohikeitto, kaalilaatikko, ateriasalaatti...). Arvonta ei koskaan arvo niitä — ne valitaan tietoisesti ☰-valitsimesta, jossa on "Vain valmisruuat" -suodatin. Valmisruoka korvaa koko satsin ja ostoslistaan tulee annospakkaukset kappaleina. Kevyisiin valmisruokiin on merkitty proteiinintäydennysvinkki.
- **Proteiinivinkit:** vähäproteiinisiin resepteihin (esim. kikhernecurry, viili) on lisätty konkreettinen täydennysohje huom-kenttään ("lisää 200 g raejuustoa" tms.) sporttiruoka-ajattelun mukaisesti.
- **115+13 reseptiä** (aamupala 28, lounas 56, päivällinen 54, iltapala 33, välipala 25). Pääpaino: helppo, edullinen, terveellinen suomalainen arkiruoka.
- **Gluteeniton laajennettu:** reseptit, joissa ainoa gluteenin lähde on suoraan gluteenittomana myytävä tuote (pasta, leipä, tortilla, nuudeli, näkkäri, mysli...), on merkitty gluteenittomiksi ja huom-kentässä lukee mikä ainesosa vaihdetaan. Näin gluteeniton-suodatin ei tiputa esim. pastaruokia pois. Vain 4 reseptiä jää suodattimen ulkopuolelle.

> **Huom:** v2 nollaa selaimeen tallennetun viikon kertaalleen (datamalli muuttui). Ensimmäisellä avauksella arvotaan uusi viikko.

## Reseptin lisääminen (recipes.json)

```json
{
 "id": "uniikki-tunniste",
 "name": "Ruuan nimi",
 "categories": ["lounas", "paivallinen"],
 "tags": ["gluteeniton", "maidoton", "vhh"],
 "redMeat": 2,
 "ready": true,
 "huom": "Vapaaehtoinen vinkki. Esim. Gluteeniton: vaihda pasta gluteenittomaan.",
 "prepMinutes": 25,
 "servings": 4,
 "kcal": 450,
 "protein": 30,
 "ingredients": [
  {"name": "Ainesosa", "qty": 400, "unit": "g"},
  {"name": "Suolaa", "qty": null, "unit": "", "pantry": true}
 ],
 "steps": ["Vaihe 1.", "Vaihe 2."]
}
```

- `categories`: aamupala, lounas, paivallinen, iltapala, valipala (voi olla useita)
- `ready: true` → kaupan valmisateria: ei koskaan mukana arvonnassa, valittavissa ☰-valitsimen kautta
- `redMeat`: jätä pois jos ei punaista lihaa; `1` = sisältää vähän (esim. kinkkua tai pekonia lisukkeena), `2` = punainen liha pääosassa
- `kcal` ja `protein` per annos (arvioita)
- `pantry: true` → ainesosa listautuu ostoslistan maustekaappiosioon
- `qty: null` → ainesosa ilman määrää ("maun mukaan")

## Julkaisu GitHub Pagesiin

1. Vie `index.html`, `recipes.json` ja `README.md` repon juureen
2. Luo juureen tyhjä `.nojekyll`-tiedosto (ohittaa Jekyll-buildin)
3. Settings → Pages → Deploy from a branch → `main` / `(root)`
4. Sivu aukeaa muutaman minuutin päästä osoitteessa `https://<käyttäjä>.github.io/<repo>/`

## Tekniikka

Yksi HTML-tiedosto, vanilla JS, ei riippuvuuksia. Reseptit ladataan `recipes.json`-tiedostosta fetchillä (vaatii palvelimen, ei toimi file://-osoitteesta). localStorage-avaimet: `vl_settings`, `vl_week`, `vl_history`, `vl_checked`.
