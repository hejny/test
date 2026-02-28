# PRD: Vylepšení UI/UX mobilního menu

- **Produkt/Repo:** `hejny/test`
- **Dokument:** PRD
- **Autor:** AI (Typescript fullstack dev + PO)
- **Datum:** 2026-02-28
- **Stav:** Draft (čeká na doplnění kontextu)

## 1) Shrnutí
Cílem je zlepšit použitelnost a dohledatelnost navigace na mobilu (menu), snížit tření při přechodu na klíčové stránky a zlepšit metriky navigačních interakcí.

Tento PRD definuje cíle, rozsah, UX návrhy a akceptační kritéria pro vylepšení mobilního menu. Repo aktuálně obsahuje jednoduchý web (`index.html`), takže návrh je obecný a aplikovatelný pro responzivní web/PWA.

## 2) Kontext a problém
### Pozorované/typické problémy (hypotézy k ověření)
Protože nebyly dodány konkrétní pain pointy, vycházíme z nejčastějších problémů mobilních menu:
- Uživatelé nenachází hlavní akce (příliš hluboko v menu / špatná hierarchie).
- Menu je dlouhé, bez skupin a vizuálních oddělovačů.
- Malé tap targety a nízký kontrast.
- Menu se zavírá „nečekaně“ (scroll, tap mimo) a uživatel ztrácí kontext.
- Chybí stav aktivní stránky a jasné breadcrumb/kontext.

### Proč teď
- Mobilní traffic typicky tvoří většinu návštěvnosti.
- Navigace je klíčová pro aktivaci (přechod na core flows).

## 3) Cíle a non-cíle
### Cíle
1. Zrychlit cestu uživatele ke klíčovým sekcím (méně tapů, lepší IA).
2. Zvýšit míru využití navigace a snížit bounce po otevření menu.
3. Zajistit přístupnost a ovladatelnost (WCAG 2.2 AA, a11y best practices).

### Non-cíle
- Kompletní redesign vizuální identity.
- Změny informační architektury celého webu (mimo menu) bez schválení.

## 4) Uživatelské scénáře (JTBD)
- **Jako návštěvník** chci rychle najít hlavní sekce, abych dokončil svůj úkol bez hledání.
- **Jako vracející se uživatel** chci mít po ruce poslední/časté položky.
- **Jako uživatel s omezením** (motorika/čtečka) chci menu pohodlně ovládat.

## 5) Návrh řešení (UX/UI)
> Finální variantu vybereme podle typu menu (hamburger drawer vs bottom nav) a obsahu.

### Varianta A: Hamburger (drawer) + zvýrazněné primární akce
- Ikona hamburgeru v headeru + jasný label (volitelně) „Menu“.
- Drawer přes 80–90% šířky, s pozadím (scrim).
- **Sekce:**
  - Primární položky (Top 4–6)
  - Sekundární (Nastavení, Nápověda, Kontakt)
  - Účet (Přihlášení/Profil/Odhlásit)
- Aktivní položka zvýrazněná.
- Přidat vyhledávání v menu, pokud položek > ~8–10.

### Varianta B: Bottom navigation pro top úkoly + „More“ drawer
- 3–5 tabů pro nejčastější cíle.
- Poslední tab „Více“ otevírá drawer se zbytkem.
- Výhoda: rychlý přístup palcem, menší potřeba otevírat menu.

### Doporučení (heuristika)
- Pokud máte **≤5 klíčových destinací**, preferovat bottom nav.
- Pokud máte **hodně sekcí / hierarchii**, preferovat drawer + vyhledávání/skupiny.

## 6) Informační architektura (IA)
### Minimální struktura (template)
1. **Domů**
2. **Hlavní sekce A**
3. **Hlavní sekce B**
4. **Hlavní akce (CTA)**
5. **Kontakt / Podpora**

Sekundární:
- Nastavení
- O aplikaci
- Podmínky / GDPR

> Tady doplníme vaše reálné položky (Top 5) a případné role (guest vs logged-in).

## 7) Interakční detaily
- Otevření menu tapem na ikonu.
- Zavření menu:
  - tap na scrim,
  - tlačítko „Zavřít“ (pro a11y),
  - klávesa ESC (na webech s klávesnicí).
- Zachovat scroll pozici stránky při otevřeném menu.
- Focus management:
  - focus trap uvnitř draweru,
  - po zavření vrátit focus na trigger.

## 8) Přístupnost (WCAG)
- Tap target min. **44×44 px**.
- Kontrast textu min. **4.5:1** (AA).
- `aria-expanded`, `aria-controls` na triggeru.
- Drawer jako `role="dialog"` / `nav` dle implementace.
- Focus visible (outline) pro klávesnici.
- Podpora čteček: správné popisky ikon.

## 9) Telemetrie / analytika (event plan)
> Názvy eventů jsou návrh; upravíme podle GA4/Mixpanel konvencí.

### Eventy
1. `menu_open`
   - props: `source` (hamburger/bottom_more), `page`, `viewport`
2. `menu_close`
   - props: `method` (scrim/close_btn/escape/navigate)
3. `menu_item_click`
   - props: `item_id`, `item_label`, `item_level`, `from_page`
4. `bottom_nav_click` (pokud bottom nav)
   - props: `tab_id`, `from_page`

### KPI (návrh)
- CTR na top položky menu.
- % sessions s otevřeným menu.
- Drop-off po `menu_open` (uživatel zavře bez navigace).
- Čas do první navigace po `menu_open`.

## 10) Akceptační kritéria
### Funkční
- [ ] Menu je dostupné a funkční na mobilních breakpointech (např. ≤ 768px).
- [ ] Uživatel může menu otevřít i zavřít minimálně 3 způsoby (scrim, close, ESC).
- [ ] Klik na položku naviguje a menu se zavře.
- [ ] Aktivní stránka je vizuálně označena.

### UX
- [ ] Primární položky jsou viditelné bez scrollu na běžných mobilech (výška ~700–800px) – nebo je jasně naznačen scroll.
- [ ] Tap targety splňují 44×44px.

### A11y
- [ ] Trigger má `aria-expanded` a `aria-controls`.
- [ ] Drawer má focus trap a po zavření vrací focus.
- [ ] Kontrast textu splňuje AA.

### Analytika
- [ ] Odesílají se eventy `menu_open`, `menu_close`, `menu_item_click` s definovanými properties.

## 11) Edge cases
- Dlouhé názvy položek (zalomení / ellipsis).
- Velké množství položek (sekce + sticky header ve draweru).
- Stav přihlášen/odhlášen.

## 12) Otevřené otázky (nutné doplnit)
Prosím odpověz, a PRD aktualizuji z „template“ na konkrétní:
1. Je to **web / PWA / nativní appka**?
2. Typ menu: **hamburger drawer**, **bottom navigation**, nebo kombinace?
3. Jaké jsou konkrétní problémy dnes (3–5 bodů)?
4. Jakých je **Top 5 položek menu** + sekundární odkazy?
5. Jaký používáte design system (MUI/Tailwind/vlastní) a analytics (GA4/Amplitude/Mixpanel)?

## 13) Návrh rollout plánu
- Fáze 1: Implementace + a11y + základní eventy.
- Fáze 2: A/B test (Varianta A vs B) dle trafficu.
- Fáze 3: Iterace podle dat (zkrácení menu, re-order, sticky CTA).
