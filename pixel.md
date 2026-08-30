# TikTok Pixel — marotino.com

- **Pixel ID:** `D9UF6A3C77U31VPN3CM0`
- **Wpięty w:** `src/layouts/BaseLayout.astro` (repo `cccccmmmm/marotino_www_astro1`, lokalnie `/Users/ceez/marotino_www_astro1`), linie ~109-121.
- **Consent:** kategoria "marketing" w cookieconsent (`src/scripts/cookieconsent-config.ts:38-46`), razem z Google Ads. EEA: `ttq.holdConsent()` do zgody użytkownika. Poza EEA: odpala się od razu (`ccMode='opt-out'`).
- **Eventy:** tylko bazowy `ttq.page()` (PageView) przy starcie. Brak custom eventów (np. Contact / lead form submit) na dzień 30.08.2026.

## TikTok Ads Manager

- Login: konto TikTok (`tiktok.com`) jest zalogowane w przeglądarce Chrome sesji, ale **TikTok Ads Manager / Business Center wymaga osobnego logowania** (`ads.tiktok.com`) — sama sesja tiktok.com nie wystarcza. Login przez email+hasło do konta Ads Managera (nie "Log in with TikTok") zadziałał 31.08.2026.
- **Kampania "KSA Leads - Spark Ad - Arabic"** uruchomiona 31.08.2026 — patrz [`ksa-leads-campaign.md`](ksa-leads-campaign.md). Użyto tego samego filmu portfolio co planowana wcześniej kampania Dubaj (`tiktok.com/@marotino_com/video/7673115280951971094`), ale finalnie targetowana na Arabię Saudyjską (taniej niż UAE) z celem Lead Generation zamiast Traffic.

## Do zrobienia

- Dodać custom event (np. `ttq.track('Contact')`) na submit formularza kontaktowego, analogicznie do `generate_lead` w GA4.
- Zweryfikować w TikTok Events Manager czy pixel faktycznie dostaje ruch (Test Events / Diagnostics).
- Śledzić wyniki kampanii KSA Leads w Leads Center i ewentualnie dodać więcej kreatywów (TikTok sugeruje min. 6 dla lepszego kosztu/lead).
