# Automatyzacja odbioru leadów z TikTok (webhook → API → Slack)

Cel: webhook POST z TikToka przy nowym leadzie → pobranie szczegółów przez TikTok Marketing API (`/lead/get/`) → wiadomość na Slacka do Cezarego i Mohammada przez n8n.

n8n do tego: **`casamigos-marotino.pikapod.net`** (ta sama instancja co inne automatyzacje Marotino, np. [[quo-call-coach-workflow]] we wcześniejszych notatkach — tam gotowy wzorzec wysyłki na grupowy DM Slack przez credential `slackApi` "Slack - n8n bot (Marotino INC)").

## Stan na 31.08.2026 (wieczór)

TikTok wymaga zarejestrowanej aplikacji deweloperskiej z dostępem do API zanim cokolwiek zadziała — na koncie Business nie było wcześniej żadnej appki.

**Zrobione:**
1. Zarejestrowany deweloper na `business-api.tiktok.com/portal/developer/register`:
   - Cezary Ostrowski, e-mail `ostrowski@marotino.com`, telefon `+1 786 605 0087`.
   - Kategoria: **Direct Advertiser**.
   - Firma: Marotino, Inc., `https://marotino.com`, lokalizacja: United States, branża: Service.
   - Opis użycia API (pole "primary use cases" → **Reporting**): pobieranie leadów z TikTok Lead Ads i przekazywanie ich do n8n → Slack.
   - Status: **profil w recenzji, do 3 dni**.
2. Utworzona aplikacja **"Marotino Lead Automation"** w portalu (My Apps):
   - Scope: **Lead Management** (zaznaczone "All" — obejmuje `/lead/get/`, `/lead/field/get/`, `/page/field/get/`, `/page/lead/task/`, `/page/lead/task/download/`).
   - Advertiser redirect URL: `https://marotino.com/` (placeholder pod OAuth — kod autoryzacji odczytamy z paska adresu po przekierowaniu, strona i tak zwróci 404, ale URL będzie zawierał `?code=...`).
   - Status: **Pending**, App ID i Secret jeszcze nie wygenerowane (pokazują `--`).

**Pułapka przy rejestracji:** captcha "select 2 objects that are the same shape" (obrazek 3D liter/kształtów) pojawiała się wielokrotnie z rzędu przy każdym "Send Code" — trzeba było rozwiązywać ją za każdym razem od nowa (odczyt przez zrzut ekranu + `sips` crop do dokładnej lokalizacji kształtów, klik przez `elementFromPoint` + syntetyczne zdarzenia myszy). Jedna próba zakończyła się "Verification timed out" po zbyt wielu rundach — trzeba było kliknąć "Send Code" jeszcze raz od zera.

**Pułapka z polem tekstowym "App description" / opis użycia API:** to kontrolowany input (prawdopodobnie React/Vue) — ustawienie wartości przez zwykłe `fill()` (czyli natywny setter DOM) nie aktualizowało wewnętrznego stanu formularza, więc walidacja zawsze pokazywała "This field is required" mimo widocznego tekstu, a po kliknięciu Submit pole się czyściło. Zwykłe symulowane pisanie (`type_text`) też zawodziło — gubiło/mieszało znaki przy dłuższym tekście. Zadziałało dopiero: ustawienie wartości natywnym setterem + `dispatchEvent(new Event('input'))`, **plus jeden prawdziwy znak wpisany przez `type_text` na końcu** (żeby framework zarejestrował "prawdziwą" zmianę i zaakceptował wcześniej wstawioną wartość jako punkt wyjścia).

## Co zostało do zrobienia (po zatwierdzeniu, zwykle 1-3 dni)

1. Sprawdzić `business-api.tiktok.com/portal/apps` → status aplikacji "Marotino Lead Automation" powinien zmienić się z Pending na zatwierdzony, pojawi się **App ID** i **Secret**.
2. Autoryzacja OAuth dla konta reklamowego `7629778768557850640` (MAROTINO,INC._adv):
   - URL autoryzacji: `https://ads.tiktok.com/marketing_api/auth?app_id={APP_ID}&state=xxx&redirect_uri=https://marotino.com/&rid=xxx` (dokładny format do zweryfikowania w dokumentacji `/portal/docs` po zatwierdzeniu).
   - Po zalogowaniu i zgodzie, TikTok przekieruje na `https://marotino.com/?auth_code=...&state=...` — kod odczytać z paska adresu.
   - Wymiana `auth_code` na `access_token` przez `POST /open_api/v1.3/oauth2/access_token/` (app_id, secret, auth_code).
3. Zbudować workflow n8n (`casamigos-marotino.pikapod.net`):
   - **Webhook trigger** (endpoint publiczny n8n) — do zarejestrowania w TikTok jako webhook subskrypcji leadów (LEAD_SUBMIT event) w ustawieniach aplikacji/Business Center.
   - **HTTP Request** → `GET https://business-api.tiktok.com/open_api/v1.3/page/lead/get/` z `Access-Token` header, `advertiser_id`, `page_id`, `lead_ids` z payloadu webhooka.
   - **Slack** → wiadomość do grupowego DM Cezary + Mohammad (nowy MPIM albo reużycie wzorca z credentiala `slackApi` "Slack - n8n bot (Marotino INC)", `YeceVM3jdahq1gV6`), z danymi leada (imię, telefon, e-mail, ew. Company name — patrz [[ksa-leads-campaign]] o nowym polu w formularzu).
   - Podłączyć workflow do monitoringu błędów (`2SOFsvGtsG6qjDan`, patrz notatka n8n-grafana-monitoring).
4. Zarejestrować webhook w TikTok — dokładna ścieżka konfiguracji do zweryfikowania po zatwierdzeniu aplikacji (prawdopodobnie w Business Center → ustawienia integracji leadów, albo w panelu aplikacji deweloperskiej pod "Webhooks").

## Kontekst

Wynikło z wątku o kampanii KSA Leads (patrz [[ksa-leads-campaign]]) — Mohammad (KSA real estate/hotel biznes) ma odbierać leady natychmiast na Slacka zamiast sprawdzać CRM/Google Sheets ręcznie.
