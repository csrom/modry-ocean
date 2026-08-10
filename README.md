# Modrý oceán — web občianskeho združenia

Jednostránkový web občianskeho združenia **Modrý oceán** (Žilina) o jeho charitatívnej, komunitnej a osvetovej činnosti.

## Súbory

```
index.html      celý web v jednom súbore
obrazky/        fotky z podujatí
```

Web je jeden samostatný HTML súbor. Nepotrebuje npm ani build — otvorí sa dvojklikom na `index.html` a nasadí sa nahratím `index.html` spolu s priečinkom `obrazky/`.

React, Tailwind CSS a Lucide sa načítavajú z CDN, font Geist z Google Fonts.

## Obsah stránky

Hero s video pozadím → Kto sme → Tri piliere činnosti → Anjelský Zebra Night Run → galéria → kontakt.

## Úpravy

Najčastejšie zmeny sú v konštantách na začiatku skriptu v `index.html`:

| konštanta | význam |
|---|---|
| `EMAIL` | kontaktný e-mail (používa sa vo všetkých tlačidlách „Napíšte nám") |
| `VIDEO_SRC` | adresa videa v pozadí hero sekcie |
| `VIDEO_LOOP_END` | kde končí slučka videa v sekundách; `null` = celá dĺžka |
| `VIDEO_RATE` | rýchlosť prehrávania; `1` = pôvodná |

Texty sekcií sú v poli `PILIERE` a priamo v komponentoch `ONas`, `Podujatia` a `Kontakt`.

### Poznámka k videu v pozadí

Video sa neprehráva natívnym `loop`, pretože ten na konci tvrdo strihne. Namiesto toho sú v hero sekcii dve prekryté kópie videa, ktoré sa do seba prelínajú (crossfade).

Súčasný klip má 8,04 s, ale skutočná slučka v ňom trvá len ~4,96 s — na tom mieste je do videa zapečený strih späť na začiatok. Preto `VIDEO_LOOP_END = 4.92`, aby sa ten strih nikdy neprehral. **Pri výmene videa treba túto hodnotu upraviť alebo nastaviť na `null`.**

Ak sa video nenačíta, zobrazí sa záložná fotka `obrazky/bezci-noc.jpg` a stránka ostane plne čitateľná.

## Kde web beží

| kde | adresa |
|---|---|
| **ostrá doména** | **https://www.modryocean.sk** |
| Vercel | https://modry-ocean.vercel.app |
| GitHub Pages | https://csrom.github.io/modry-ocean/ |

Adresa bez `www` (`modryocean.sk`) sa presmeruje na tvar s `www`, takže zdieľať sa dá ktorákoľvek.

Vercel aj GitHub Pages sa nasadzujú samy pri každom pushi do vetvy `main`.

## Doména

Doména je zaregistrovaná na WebGlobe a nasmerovaná na Vercel. Nastavené DNS záznamy, riešenie problémov aj postup pri výmene domény sú v [DOMENA.md](DOMENA.md).

## Kontakt

Tibor Lettrich — tibor.lettrich@gmail.com

MODRÝ OCEÁN, Hollého 618/46, 010 01 Žilina, IČO: 56461305
