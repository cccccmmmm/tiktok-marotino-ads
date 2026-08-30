# TikTok Pixel — marotino.com

- **Pixel ID:** `D9UF6A3C77U31VPN3CM0`
- **Wpięty w:** `src/layouts/BaseLayout.astro` (repo `cccccmmmm/marotino_www_astro1`, lokalnie `/Users/ceez/marotino_www_astro1`), linie ~109-121.
- **Consent:** kategoria "marketing" w cookieconsent (`src/scripts/cookieconsent-config.ts:38-46`), razem z Google Ads. EEA: `ttq.holdConsent()` do zgody użytkownika. Poza EEA: odpala się od razu (`ccMode='opt-out'`).
- **Eventy:** tylko bazowy `ttq.page()` (PageView) przy starcie. Brak custom eventów (np. Contact / lead form submit) na dzień 30.08.2026.

## TikTok Ads Manager

- Login: konto TikTok (`tiktok.com`) jest zalogowane w przeglądarce Chrome sesji, ale **TikTok Ads Manager / Business Center wymaga osobnego logowania** (`ads.tiktok.com`) — sama sesja tiktok.com nie wystarcza, próba "Log in with TikTok" wymaga ręcznego potwierdzenia.
- **Kampania Dubaj** (ustalona z Mohammadem Naser Eldine w DM 12.08.2026): Spark Ad z filmu portfolio `tiktok.com/@marotino_com/video/7673115280951971094`, formalny arabski z akcentem GCC, plan start "od poniedziałku" — brak potwierdzenia że kampania faktycznie ruszyła w Ads Manager.

## Do zrobienia

- Dodać custom event (np. `ttq.track('Contact')`) na submit formularza kontaktowego, analogicznie do `generate_lead` w GA4.
- Zweryfikować w TikTok Events Manager czy pixel faktycznie dostaje ruch (Test Events / Diagnostics).
- Ustawić/zweryfikować Spark Ad Code dla filmu portfolio i podpiąć budżet+targeting na Dubaj.
