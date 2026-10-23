# CDN checker import fixer

Zet de Dexis CDN failures om naar een Akeneo-importbestand voor
**Import profile - asset attribute import (NOT FOR IMAGES IMPORT)**.

## Gebruik

1. Open **https://hbendima.github.io/cdn-import-fixer/** of lokaal `index.html` in Chrome of Edge.
2. Sleep de 3 bestanden in het vak, tegelijk of één voor één:
   - `dexis-cdn-failures-*.csv` (altijd CSV)
   - Akeneo quick export met **labels** (kolom `Afbeeldingen`), als xlsx of csv
   - Akeneo quick export met **codes** (kolom `images`), als xlsx of csv
   De tool herkent zelf welk bestand welk is.
3. Bekijk het resultaat in de tool (tabs *Import*, *Gematcht*, *Niet gevonden*, *SKU problemen*).
4. Klik op **Download Akeneo import (.xlsx)** en importeer dat bestand in Akeneo.

Alles draait lokaal in de browser. Er wordt niets geüpload.
Het bestand `lib/xlsx.full.min.js` (SheetJS) moet naast de HTML blijven staan.

## Logica

- Alleen URL's die beginnen met `https://cdn.dexis.be/images/` worden verwerkt. CloudFront wordt overgeslagen.
- Het label is het voorlaatste deel van de URL:
  `.../images/<uuid>/makita_1913u9_9_12/makita_1913u9_9_12_532x532.webp` geeft `makita_1913u9_9_12`.
- Per SKU worden labels en codes op positie gekoppeld, want de volgorde in beide exports is gelijk.
  Verschilt het aantal labels en codes bij een SKU, dan wordt die SKU overgeslagen en gemeld.
- Eerst wordt gezocht op SKU + label. Lukt dat niet, dan op het label alleen, als het precies één code heeft.
- Elke code komt maar één keer in het importbestand (`images | <code> | YES`).

