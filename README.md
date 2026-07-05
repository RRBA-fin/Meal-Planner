# Viikkolista

Mobiilikäyttöön suunniteltu viikon ruokalista- ja ostoslistatyökalu. Arpoo viikon ruuat 2–3 päivän satseina (aamupala, lounas, päivällinen, iltapala + valinnaiset välipalat) ja kokoaa niistä ostoslistan. Ei palvelinta, ei riippuvuuksia — pelkkä HTML + JSON.

## Käyttöönotto GitHub Pagesissa

1. Luo uusi repo ja lataa sinne `index.html`, `recipes.json` ja tämä `README.md`.
2. Repon asetuksissa: **Settings → Pages → Source: Deploy from a branch → main / (root)**.
3. Sivu aukeaa hetken päästä osoitteessa `https://<käyttäjä>.github.io/<repo>/`.
4. Lisää puhelimen kotinäytölle selaimen "Lisää aloitusnäyttöön" -toiminnolla.

Paikallinen testaus: `python3 -m http.server` projektin kansiossa ja avaa `http://localhost:8000`. (Suoraan tiedostosta avattuna selain estää `recipes.json`-tiedoston lataamisen.)

## Ominaisuudet

- **Viikon arvonta** 2–3 päivän satseina — samaa ruokaa syödään 2–3 päivää, kuten batch-kokkauksessa kuuluu. Väripalkki yhdistää saman satsin päivät.
- **Lounas & päivällinen**: asetuksista valittavissa joko yksi iso satsi molempiin tai omat ruuat kummallekin.
- **Yksittäisen ruuan uudelleenarvonta** ⟳-napista ilman että koko viikko menee uusiksi.
- **Rotaatiomuisti**: arvonta välttää 28 päivän sisällä käytettyjä reseptejä, kun vaihtoehtoja riittää.
- **Välipalat**: himmennetty "+ Lisää välipala" -rivi joka päivässä; valinnat saa mukaan ostoslistaan.
- **Ostoslista**: ainekset yhdistettynä ja skaalattuna (henkilömäärä × päivät × satsit), ruksattavat rivit, kopiointi leikepöydälle. Maustekaappitavarat omassa taittuvassa osiossa ilman määriä.
- **Suodattimet**: gluteeniton / maidoton / VHH, yhdisteltävissä.
- **Ravintotiedot**: kcal ja proteiini per annos (arvioita) sekä päiväkohtainen summa.
- Kaikki tallentuu selaimen localStorageen — lista säilyy laitteella.

## Reseptien lisääminen

Avaa `recipes.json` ja lisää `recipes`-taulukkoon uusi olio:

```json
{
  "id": "uniikki-tunniste",
  "name": "Ruuan nimi",
  "categories": ["lounas", "paivallinen"],
  "tags": ["gluteeniton", "maidoton", "vhh"],
  "huom": "Vapaaehtoinen huomautus (esim. gluteeniton kaura).",
  "prepMinutes": 25,
  "servings": 4,
  "kcal": 450,
  "protein": 30,
  "ingredients": [
    { "name": "Jauhelihaa", "qty": 400, "unit": "g" },
    { "name": "Maitorahkaa", "qty": 1, "unit": "prk (200–250 g)" },
    { "name": "Suolaa ja pippuria", "qty": null, "unit": "", "pantry": true }
  ],
  "steps": ["Vaihe 1.", "Vaihe 2."]
}
```

Kentistä:

- `categories`: yksi tai useampi arvoista `aamupala`, `lounas`, `paivallinen`, `iltapala`, `valipala`. Sama resepti voi olla useassa.
- `tags`: vain ne joita resepti aidosti täyttää — suodattimet luottavat näihin.
- `servings`: montako annosta perusresepti tuottaa. Ostoslista skaalaa tästä.
- `qty: null` = "sopivasti / maun mukaan"; `pantry: true` siirtää aineksen maustekaappiosioon pois varsinaiselta ostoslistalta.
- `kcal` ja `protein` ilmoitetaan per annos, suurpiirteinen arvio riittää.

Pilkut ja lainausmerkit tarkasti — virheellinen JSON estää koko sovelluksen latautumisen. Voit tarkistaa tiedoston esim. osoitteessa jsonlint.com ennen pushia.

## Tiedostot

| Tiedosto | Tehtävä |
|---|---|
| `index.html` | Koko käyttöliittymä ja logiikka (ei build-työkaluja) |
| `recipes.json` | Reseptidata — ainoa tiedosto jota muokataan reseptejä lisätessä |
