# PRD: Vylepšení hamburger menu na mobilu (responsive web)

- **Product/Repo:** `hejny/test`
- **Dokument:** Product Requirements Document (PRD)
- **Datum:** 2026-03-01
- **Status:** Draft

## 1) Shrnutí
Cílem je zlepšit použitelnost mobilní navigace (hamburger menu) na responsive webu: rychlejší dohledání klíčových sekcí, méně omylů při ovládání, lepší přístupnost a měřitelné zlepšení v navigačních metrikách.

Dodávkou je mobilní „drawer“ (boční panel) s jasnou hierarchií položek, konzistentním chováním při otevření/zavření, a11y implementací a základním měřením.

## 2) Kontext a problém
### Předpokládané UX problémy (typické)
Pokud nemáme popsaný současný stav, vycházíme z častých problémů hamburger menu:
- Položky jsou nejasně seskupené a uživatelé nevidí prioritu.
- Malé tap targety / těsné rozestupy → mis-tapy.
- Chybí indikace aktivní stránky → ztráta kontextu.
- Nekonzistentní zavírání (tap mimo / scroll / „back“ na Androidu).
- Nedostatečná přístupnost pro klávesnici a screen readery.

### Proč teď
- Mobilní návštěvnost bývá majoritní.
- Navigace přímo ovlivňuje dosažení hlavních konverzních kroků.

## 3) Cíle a necíle
### Cíle
1. **Zrychlit navigaci:** snížit počet tapů a čas k dosažení top sekcí.
2. **Zvýšit úspěšnost:** více navigací po otevření menu, méně „open→close bez akce“.
3. **Zajistit a11y:** WCAG 2.2 AA, ovladatelnost klávesnicí, správné ARIA.

### Necíle
- Kompletní redesign brandu.
- Změna celé informační architektury webu (mimo strukturaci menu), pokud nebude schválena.

## 4) Uživatelé a scénáře (JTBD)
- **Nový návštěvník:** potřebuje rychle pochopit „co tady najdu“ a kam kliknout.
- **Returning user:** chce se dostat na často používané sekce co nejrychleji.
- **Uživatel se asistivními technologiemi / znevýhodněním:** potřebuje velké cíle, čitelnost, ovladatelnost.

## 5) Návrh řešení (UX/UI)
### 5.1 Komponenty
- **Trigger (hamburger) v hlavičce**
  - Ikona + volitelný text „Menu“ (doporučeno pro srozumitelnost).
  - Stav `aria-expanded`.
- **Drawer (boční panel)**
  - Šířka: ~80–90 % viewportu (mobil).
  - Pozadí: **scrim** (poloprůhledný overlay) pro jasný fokus.
  - Uvnitř: hlavička (logo/název), volitelně uživatelská sekce.
- **Položky menu**
  - Primární (top 4–6) nahoře.
  - Sekundární níže (O nás, Kontakt, Podpora, Nastavení, atd.).
  - Volitelně „sticky“ primární CTA (např. „Začít“, „Objednat“).

### 5.2 Hierarchie a obsah (šablona)
- Sekce **Primární**: Home, Produkt, Ceník, Reference, Kontakt
- Sekce **Sekundární**: O nás, FAQ/Podpora, Podmínky, Ochrana soukromí
- Sekce **Account** (pokud existuje): Přihlásit / Profil / Odhlásit

> PO dodá finální seznam položek a jejich pořadí podle byznys priorit.

### 5.3 Chování (interakce)
- Otevření: tap na trigger.
- Zavření:
  - tap na scrim,
  - tlačítko „Zavřít“ v draweru,
  - klávesa ESC,
  - po kliknutí na položku (navigace).
- **Scroll management:** při otevřeném menu se ne-scrolluje background (body lock).
- **Back button (Android / browser):** pokud je menu otevřené, „back“ nejdřív zavře menu (bez navigace zpět).
- **Aktivní položka:** vizuálně zvýraznit aktuální stránku.

### 5.4 Responsivita
- Menu se aktivuje pro breakpoint např. `<= 768px` (upřesnit podle designu).
- V landscape zajistit rozumnou výšku a interní scroll seznamu.

## 6) Přístupnost (WCAG 2.2 AA)
### Požadavky
- Tap target min. **44×44 px**.
- Kontrast textu min. **4.5:1**.
- Trigger:
  - `aria-label="Menu"` (pokud není text),
  - `aria-expanded` (true/false),
  - `aria-controls` (id draweru).
- Drawer:
  - `role="dialog"` + `aria-modal="true"` (nebo semantické `nav` + focus trap, dle implementace),
  - **focus trap** uvnitř,
  - při zavření vrátit focus na trigger,
  - viditelný focus outline.

## 7) Telemetrie / analytika (event plan)
### Eventy
1. `menu_open`
   - props: `page`, `viewport_w`, `viewport_h`, `source` (hamburger)
2. `menu_close`
   - props: `method` (scrim|close_btn|escape|navigate|back), `open_duration_ms`
3. `menu_item_click`
   - props: `item_id`, `item_label`, `item_position`, `from_page`

### KPI (návrh)
- % sessions s `menu_open`.
- `menu_open → menu_item_click` konverze.
- Drop-off: `menu_open → menu_close` bez kliknutí.
- Time-to-navigate po otevření.

## 8) Akceptační kritéria
### Funkční
- [ ] Na mobilních breakpointech je dostupný hamburger trigger v hlavičce.
- [ ] Menu se otevře jako drawer a zablokuje scroll pozadí.
- [ ] Menu lze zavřít scrimem, tlačítkem Zavřít, klávesou ESC a back tlačítkem (když je otevřené).
- [ ] Klik na položku provede navigaci a zavře menu.
- [ ] Aktivní stránka je v menu zvýrazněna.

### UX
- [ ] Primární položky jsou viditelné bez scrollu na běžných mobilech (~700–800px výška), nebo je jasně indikováno scrollování.
- [ ] Položky mají tap target min. 44×44 px.

### A11y
- [ ] Trigger má korektní ARIA (`aria-expanded`, `aria-controls`, label).
- [ ] Drawer má focus trap a po zavření vrací focus na trigger.
- [ ] Kontrast splňuje WCAG AA.

### Analytika
- [ ] Emitují se eventy `menu_open`, `menu_close`, `menu_item_click` s definovanými properties.

## 9) Edge cases
- Dlouhé názvy položek (wrap/ellipsis).
- Velké množství položek (skupiny + sticky header v draweru).
- Stav přihlášen/nepřihlášen.
- iOS Safari: scroll/overflow chování.

## 10) Otevřené otázky (k finalizaci)
1. Jaké jsou **aktuální problémy** (3–5 konkrétních bodů) na vašem webu?
2. Jaký je **finální seznam položek** (primární/sekundární) a jejich priorita?
3. Má menu obsahovat účetní sekci (login/profil) a jaké stavy?
4. Jaké používáte měření (GA4/…)?
5. Má drawer obsahovat vyhledávání (pokud položek > 8–10)?

## 11) Rollout plán
- Phase 0: Baseline měření (současný stav, pokud existuje).
- Phase 1: Implementace draweru + a11y + eventy.
- Phase 2: Iterace podle dat (pořadí položek, skupiny, CTA, search).
