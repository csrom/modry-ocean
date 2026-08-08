# Nasmerovanie vlastnej domény z WebGlobe na Vercel

Web beží na dvoch miestach, obe z tohto repozitára:

| kde | adresa |
|---|---|
| Vercel | https://modry-ocean.vercel.app |
| GitHub Pages | https://csrom.github.io/modry-ocean/ |

**Vlastná doména môže viesť len na jedno z nich** — DNS záznam ukazuje na jeden cieľ. Tento návod ju smeruje na **Vercel**. GitHub Pages ostane bežať na svojej adrese ako záloha.

V návode je `vasadomena.sk` len zástupný názov — nahraďte ho svojou doménou.

---

## Časť 1 — Vercel: odtiaľ získate hodnoty

1. Prihláste sa na **vercel.com** a otvorte projekt **modry-ocean**.
2. Hore kliknite **Settings**, v ľavom stĺpci **Domains**.
3. Tlačidlo **Add Domain**, napíšte `vasadomena.sk`, potvrďte.
4. Vercel ponúkne pridať aj `www.vasadomena.sk` — **prijmite to.** Vyrieši tým presmerovanie medzi tvarom s `www` a bez neho.
5. Vercel teraz zobrazí kartu s presnými hodnotami. **Opíšte si ich** — sú jedinečné pre tento projekt:

   - pre `vasadomena.sk` → typ **A** + IP adresa (býva `76.76.21.21`, pri novších projektoch `216.198.79.1`)
   - pre `www.vasadomena.sk` → typ **CNAME** + cieľ v tvare `nieco.vercel-dns-0XX.com`

> **Dôležité:** Vercel generuje tieto hodnoty pre každý projekt zvlášť. Ak sa to, čo vidíte na obrazovke, líši od príkladov v tomto návode, **platí to, čo ukazuje Vercel.**

---

## Časť 2 — WebGlobe: tam ich zapíšete

1. Prihláste sa na **admin.webglobe.sk**.
2. **Domény** → kliknite na svoju doménu → **DNS** → **DNS záznamy**.
3. Zelené tlačidlo **Nový DNS záznam** a vyplňte prvý:

   | pole | čo zadať |
   |---|---|
   | Typ | `A` |
   | Názov | *nechať prázdne* — ide o koreňovú doménu. Ak panel pole vyžaduje, zadajte `@` |
   | Hodnota | IP adresa opísaná z Vercelu |
   | TTL | `3600` |

4. Znovu **Nový DNS záznam**, druhý záznam:

   | pole | čo zadať |
   |---|---|
   | Typ | `CNAME` |
   | Názov | `www` |
   | Hodnota | reťazec z Vercelu, napr. `d1d4fc829fe7bc7c.vercel-dns-017.com` |
   | TTL | `3600` |

5. **Skontrolujte, či tam už nejaké A alebo CNAME záznamy pre `@` a `www` nie sú.** WebGlobe ich pridáva automaticky a smerujú na ich parkovaciu stránku. Ak tam sú, upravte ich alebo zmažte — inak si budú so záznamami pre Vercel odporovať a doména bude fungovať náhodne.

---

## Časť 3 — Čakanie a kontrola

- Zmena sa prejaví **rádovo v hodinách**, úplná propagácia môže trvať až 24 hodín.
- Vo Verceli na karte domény sa stav sám prepne na **Valid Configuration**.
- **HTTPS certifikát vystaví Vercel automaticky** — netreba nič kupovať ani nahrávať.
- Overenie z príkazového riadka:

  ```
  nslookup vasadomena.sk
  nslookup www.vasadomena.sk
  ```

  Prvý má vrátiť IP adresu od Vercelu, druhý má ukazovať na `vercel-dns` adresu.

---

## Poznámky

**Zdieľanie odkazu.** Dlhé adresy, ktoré ponúka Vercel dashboard (`modry-ocean-9f1upgrtv-roman-fa5e.vercel.app`), sú za prihlásením — návštevníkovi ukážu stránku „Login – Vercel", nie web. Zdieľajte `modry-ocean.vercel.app`, neskôr vlastnú doménu. Nastavuje sa to vo Vercel → Settings → Deployment Protection.

**Po nasadení domény** má zmysel doplniť do `index.html` absolútnu adresu v `og:image` a pridať `og:url`, aby náhľad pri zdieľaní na Facebooku fungoval spoľahlivo. Teraz je `og:image` relatívna cesta.

**Keby ste doménu chceli radšej na GitHub Pages**, záznamy by sa museli prepísať na GitHub a vo Verceli doménu odobrať. Nedá sa to naraz na oboch.

---

Zdroje: [Vercel — Adding & Configuring a Custom Domain](https://vercel.com/docs/domains/working-with-domains/add-a-domain), [Vercel — Can I use my domain with A records?](https://vercel.com/kb/guide/a-record-and-caa-with-vercel), [WebGlobe — Nastavení DNS A a CNAME záznamů](https://www.webglobe.cz/poradna/nastaveni-dns-a-cname)
