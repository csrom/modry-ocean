# Doména modryocean.sk

## Aktuálny stav — nastavené a funkčné

Doména je zaregistrovaná na **WebGlobe** a nasmerovaná na **Vercel**. **Netreba na nej už nič meniť.**

| adresa | čo robí |
|---|---|
| **https://www.modryocean.sk** | ostrá adresa webu |
| https://modryocean.sk | presmeruje (308) na tvar s `www` |
| https://modry-ocean.vercel.app | pôvodná adresa od Vercelu, funguje ďalej |
| https://csrom.github.io/modry-ocean/ | GitHub Pages, záloha |

Zdieľať sa dá ktorýkoľvek tvar — bez `www` sa návštevník presmeruje sám.

### DNS záznamy vo WebGlobe

Toto je presne to, čo je uložené v paneli WebGlobe. **Ak by sa niekedy stratili, sem sa vracajte** — hodnoty sú jedinečné pre tento Vercel projekt:

| Typ | Názov | Hodnota | TTL |
|---|---|---|---|
| `A` | *prázdne* (koreňová doména) | `216.198.79.1` | 3600 |
| `CNAME` | `www` | `8439ecdd50fa0c05.vercel-dns-017.com` | 3600 |

Autoritatívne nameservery domény: `ns1.webglobe.sk`, `ns2.webglobe.sk`, `ns3.webglobe.com`. Záznamy sú podpísané DNSSEC.

HTTPS certifikát vystavuje a obnovuje Vercel automaticky — netreba nič kupovať ani nahrávať.

---

## Riešenie problémov

### „Pridal som CNAME a on zmizol."

**CNAME záznam nesmie na tom istom názve existovať spolu so žiadnym iným záznamom.** Je to pravidlo samotného DNS, nie obmedzenie WebGlobe. Ak je na `www` ešte A záznam (WebGlobe si ho pridáva sám a smeruje na ich parkovaciu stránku), panel CNAME buď odmietne, alebo ho ticho zahodí.

Riešenie: najprv zmazať **všetky** záznamy na názve `www`, až potom pridať CNAME.

### „Vercel mi už hodnotu CNAME neukazuje."

To nie je chyba a **netreba doménu mazať a pridávať nanovo.** Vercel zobrazuje požadované DNS záznamy len dovtedy, kým je doména v stave *Invalid Configuration*. Keď sa prepne na **Valid Configuration**, inštrukcie schová, lebo ich už netreba.

Hodnoty sú vždy v tabuľke vyššie. Ak by ste ich chceli prečítať priamo zo živého DNS, opýtajte sa **autoritatívneho servera** — obíde tým akúkoľvek cache a ukáže presne to, čo je uložené v paneli WebGlobe:

```
nslookup -type=CNAME www.modryocean.sk ns1.webglobe.sk
nslookup -type=A modryocean.sk ns3.webglobe.com
```

V PowerShelli to isté a čitateľnejšie:

```powershell
Resolve-DnsName www.modryocean.sk -Type CNAME -Server ns1.webglobe.sk -DnsOnly
Resolve-DnsName modryocean.sk -Type A -Server ns3.webglobe.com -DnsOnly
```

### „Stále sa mi zobrazuje stará parkovacia stránka WebGlobe."

Zaseknutá DNS cache na vašom počítači alebo u poskytovateľa internetu. Web funguje, len sa k vám ešte nedostala zmena.

1. `ipconfig /flushdns` v príkazovom riadku (Windows).
2. Ak to nepomôže, cachuje poskytovateľ internetu — stačí počkať do vypršania TTL, teda hodinu.
3. Kontrola, ktorá cache obchádza: `nslookup modryocean.sk 8.8.8.8`. Musí vrátiť `216.198.79.1`. Ak áno, je všetko v poriadku a ide naozaj len o cache.

Parkovaciu stránku WebGlobe spoznáte podľa toho, že ju obsluhuje `nginx` s `PHP` — Vercel posiela hlavičku `Server: Vercel`.

### „Neviem, či to funguje."

```powershell
curl.exe -sS -I https://www.modryocean.sk/
```

Musí vrátiť `HTTP/1.1 200 OK` a `Server: Vercel`.

---

## Ako sa to nastavovalo (referencia pri výmene domény)

Postup nižšie je hotový a vykonaný. Zídeme sa k nemu, len ak by sa pridávala ďalšia doména.

### Časť 1 — Vercel: odtiaľ sa získajú hodnoty

1. Prihlásiť sa na **vercel.com**, otvoriť projekt **modry-ocean**.
2. Hore **Settings**, v ľavom stĺpci **Domains**.
3. Tlačidlo **Add Domain**, zadať doménu, potvrdiť.
4. Vercel ponúkne pridať aj `www.` verziu — **prijať.** Vyrieši tým presmerovanie medzi tvarom s `www` a bez neho.
5. Vercel zobrazí kartu s presnými hodnotami. **Opísať si ich** — sú jedinečné pre projekt:
   - pre koreňovú doménu → typ **A** + IP adresa
   - pre `www.` → typ **CNAME** + cieľ v tvare `nieco.vercel-dns-0XX.com`

> **Dôležité:** Vercel generuje tieto hodnoty pre každý projekt zvlášť. Univerzálne tabuľky z internetu (`76.76.21.21`, `cname.vercel-dns.com`) nemusia platiť. **Platí to, čo ukazuje Vercel.**

### Časť 2 — WebGlobe: tam sa zapíšu

1. Prihlásiť sa na **admin.webglobe.sk**.
2. **Domény** → kliknúť na doménu → **DNS** → **DNS záznamy**.
3. Zelené tlačidlo **Nový DNS záznam**, pridať A záznam: *Typ* `A`, *Názov* nechať prázdne (ak panel pole vyžaduje, zadať `@`), *Hodnota* IP z Vercelu, *TTL* `3600`.
4. Znovu **Nový DNS záznam**, pridať CNAME: *Typ* `CNAME`, *Názov* `www`, *Hodnota* reťazec z Vercelu, *TTL* `3600`.
5. **Najprv skontrolovať, či na `@` a `www` už nejaké záznamy nie sú** a zmazať ich — inak si budú s Vercelom odporovať a CNAME sa nedá pridať (viď Riešenie problémov).

### Časť 3 — Čakanie

Zmena sa prejaví rádovo v hodinách, úplná propagácia môže trvať do 24 hodín. Vo Verceli sa stav domény sám prepne na **Valid Configuration**.

---

## Poznámky

**Zdieľanie odkazu.** Dlhé adresy, ktoré ponúka Vercel dashboard (`modry-ocean-9f1upgrtv-roman-fa5e.vercel.app`), sú za prihlásením a návštevníkovi ukážu stránku „Login – Vercel", nie web. Zdieľajte `www.modryocean.sk`. Nastavuje sa to vo Vercel → Settings → Deployment Protection.

**Prečo A + CNAME a nie Vercel nameservery.** Správa DNS ostáva na WebGlobe. Keby si združenie neskôr zriadilo e-mailové schránky na doméne, MX záznamy sa nastavia v tom istom paneli. Pri prechode na Vercel nameservery by sa celá správa DNS presunula do Vercelu.

**Náhľad pri zdieľaní.** `index.html` má `og:url` aj `og:image` zapísané absolútne na `https://www.modryocean.sk/`. Pri zmene domény treba tie dve značky v hlavičke prepísať, inak náhľad na Facebooku prestane ukazovať obrázok.

---

Zdroje: [Vercel — Adding & Configuring a Custom Domain](https://vercel.com/docs/domains/working-with-domains/add-a-domain), [Vercel — Can I use my domain with A records?](https://vercel.com/kb/guide/a-record-and-caa-with-vercel), [WebGlobe — Nastavení DNS A a CNAME záznamů](https://www.webglobe.cz/poradna/nastaveni-dns-a-cname)
