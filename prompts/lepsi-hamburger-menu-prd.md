# PRD: Lepší hamburger menu

[✨🍉] @@@

- **Product/Repo:** `hejny/test`
- **Dokument:** Product Requirements Document (PRD)
- **Datum:** 2026-03-03
- **Status:** Draft

> Poznámka: V repozitáři už jsou 2 podobné PRD (`prompts/hamburger-menu-mobile-prd.md`, `prompts/mobile-menu-ui-ux-prd.md`). Tento dokument je „master“ verze pro zadání „Lepší hamburger menu“ a v další iteraci ho buď nahradíme jedním z existujících, nebo je konsolidujeme do jednoho.

---

## 1) Shrnutí
Cílem je zlepšit mobilní navigaci (hamburger menu) tak, aby:
- uživatel rychleji našel klíčové sekce,
- snížil se počet omylů při tappování,
- menu bylo přístupné (WCAG 2.2 AA) a konzistentně ovladatelné,
- zlepšily se měřitelné metriky navigace.

Dodávkou bude mobilní „drawer“ (boční panel) se správným chováním otevření/zavření, jasnou hierarchií položek a základním měřením.

## 2) Kontext a problém
Aktuální popis problému je zatím obecný („lepší hamburger menu“). Jako výchozí hypotézy bereme typické potíže:
- nejasná hierarchie položek, bez grouping,
- malé tap targety → mis-tapy,
- chybí aktivní stav stránky,
- nekonzistentní zavírání (tap mimo, ESC, back),
- slabá a11y (focus management, ARIA).

@@@ doplnit konkrétní pain pointy z aktuálního webu / aplikace.

## 3) Cíle a necíle
### Cíle
1. Zrychlit navigaci: méně tapů a kratší čas k dosažení top sekcí.
2. Zvýšit úspěšnost: více kliků na položky po otevření menu, méně „open→close“ bez akce.
3. Zajistit a11y: WCAG 2.2 AA, klávesnice, screen reader.

### Necíle
- Kompletní redesign vizuální identity.
- Přestavba celé IA webu (mimo restrukturalizace položek menu) bez explicitního schválení.

## 4) Uživatelé a scénáře (JTBD)
- Nový návštěvník: „Chci pochopit co tu je a kam kliknout.“
- Returning user: „Chci se dostat do často používané sekce co nejrychleji.“
- Uživatel se znevýhodněním / assistive tech: „Chci menu ovládat pohodlně a bezpečně.“

## 5) Návrh řešení (UX/UI)
### 5.1 Komponenty
- Trigger v hlavičce (hamburger) + volitelný text „Menu“.
- Drawer z boku + scrim overlay.
- Hlavička draweru: logo/název + tlačítko „Zavřít“.
- Seznam položek, rozdělený na Primární / Sekundární (+ případně Account).

### 5.2 Obsah a hierarchie (placeholder)
Primární (Top 4–6):
- @@@

Sekundární:
- @@@

Account (pokud existuje):
- @@@

> PO dodá finální seznam položek a pořadí podle byznys priorit.

### 5.3 Interakce
- Otevření: tap na trigger.
- Zavření:
  - tap na scrim,
  - tlačítko Zavřít,
  - ESC,
  - „back“ na Androidu / browseru: pokud je menu otevřené, zavře ho před navigací zpět.
- Klik na položku provede navigaci a menu zavře.
- Body scroll lock (pozadí se při otevřeném menu nehýbe).
- Aktivní položka je zvýrazněna.

### 5.4 Responsivita
- Breakpoint pro hamburger: @@@ (např. `<= 768px`).
- Drawer má interní scroll pro dlouhý seznam.

## 6) Přístupnost (WCAG 2.2 AA)
- Tap target min. 44×44 px.
- Kontrast min. 4.5:1.
- Trigger:
  - `aria-label` (pokud není text),
  - `aria-expanded`,
  - `aria-controls`.
- Drawer:
  - `role="dialog"` + `aria-modal="true"` (nebo semantické `nav` s focus trapem),
  - focus trap,
  - při zavření vrátit focus na trigger,
  - viditelný focus outline.

## 7) Analytika (event plan)
Implementovat eventy (přizpůsobit GA4/Mixpanel/Amplitude):
1. `menu_open` props: `page`, `viewport_w`, `source`
2. `menu_close` props: `method`, `open_duration_ms`
3. `menu_item_click` props: `item_id`, `item_label`, `item_position`, `from_page`

KPI návrh:
- `menu_open → menu_item_click` konverze.
- drop-off: otevření bez akce.
- time-to-navigate po otevření.

## 8) Akceptační kritéria
### Funkční
- [ ] Hamburger trigger je dostupný na mobilních breakpointech.
- [ ] Menu se otevírá jako drawer a zamyká scroll pozadí.
- [ ] Menu lze zavřít scrimem, Zavřít tlačítkem, ESC a back tlačítkem (když je otevřené).
- [ ] Kliknutí na položku naviguje a zavře menu.
- [ ] Aktivní stránka je v menu zvýrazněná.

### UX
- [ ] Položky mají min. 44×44 px tap target.
- [ ] Primární položky jsou prioritizované a rychle viditelné.

### A11y
- [ ] ARIA atributy na triggeru jsou správně.
- [ ] Focus trap funguje, focus se vrací na trigger.

### Analytika
- [ ] Eventy se odesílají dle event planu.

## 9) Edge cases
- Dlouhé názvy položek (wrap/ellipsis).
- Hodně položek (grouping, sticky header, interní scroll).
- iOS Safari scroll/overflow.
- Stavy přihlášen/nepřihlášen.

## 10) Otevřené otázky
1. Kde přesně: web / PWA / app? A v jakém frameworku (Next.js/React/Vue/@@@)?
2. Jaké jsou konkrétní problémy dnes (3–5 bodů)?
3. Jaký je finální seznam položek (primární/sekundární) + jejich pořadí?
4. Má být v menu vyhledávání? (doporučeno pokud > 8–10 položek)
5. Jaké měření používáte (GA4/@@@)?
6. Má být v draweru CTA (např. „Objednat“, „Začít“)?

## 11) Rollout
- Phase 0: baseline (pokud máme analytiku) – současné metriky.
- Phase 1: implementace draweru + a11y + eventy.
- Phase 2: iterace dle dat (pořadí, grouping, CTA, search).
