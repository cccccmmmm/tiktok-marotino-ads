# Kampania — KSA Leads (Spark Ad, Arabic)

Uruchomiona 31.08.2026 w TikTok Ads Manager (konto `MAROTINO,INC._adv`, aadvid `7629778768557850640`).

## Setup

- **Cel:** Lead Generation, format Instant Form (natywny formularz TikTok, nie strona www)
- **Kreatywa:** istniejący organiczny film z konta `@marotino_com` — spot o AI recepcjoniście hotelowym "Xenia" po arabsku (formalny, akcent GCC), 10s. To ten sam film, który wcześniej planowano na Dubaj z Mohammadem Naser Eldine (`tiktok.com/@marotino_com/video/7673115280951971094`), ale ostatecznie skierowany na Arabię Saudyjską zamiast Dubaju.
- **Dlaczego KSA a nie Dubaj/UAE:** dane rynkowe (Meta/paid media ogólnie) wskazują UAE jako wyraźnie droższy rynek reklamowy niż KSA (UAE CPM ~125-165% KSA CPM) — większa populacja i niższy koszt jednostkowy w Arabii Saudyjskiej powinny dać tańszy koszt/lead.
- **Formularz (Instant Form):** pola imię + email, nagłówek "تواصل مع فريق ماروتينو", link do `https://marotino.com/privacy-policy`, firma "Marotino". Sekcja "About us" wypełniona krótkim opisem firmy po arabsku.
- **Targeting:** Arabia Saudyjska, wiek 25-54 (odznaczono 18-24 i 55+ — B2B, decydenci budżetów), język: arabski, płeć: all.
- **Budżet:** 20 USD/dzień (minimum wymagane przez TikTok dla tego typu ad group — user chciał 10 USD, nie przechodzi).
- **Płatność:** karta Mastercard ...0084 (Marotino Inc., Miami, FL — billing pod entity Inc, nie Cyprus Ltd), plus 30 USD kredytu reklamowego (promo "Upgrade to Autopay").
- **Status po starcie:** "Ad created and in review" — czeka na standardową weryfikację TikToka.

## Stary draft (usunięty)

Był przeterminowany draft "Xenia Dubai TikTok - AR/EN - 17-20.08" (ostatnia edycja 13.08.2026, daty dawno minęły, nigdy nie opublikowany) — usunięty przed startem tej kampanii.

## Pułapki napotkane przy budowie w Ads Managerze (UI oparty o web components/shadow DOM)

- Kreator kampanii ma tryb "simplified" (bez opcji Lead Generation) i "full version" — trzeba przełączyć na full version żeby zobaczyć cel Lead generation.
- Pola formularzy (budget, targeting inputs) czasem nie reagują na zwykłe `fill`/`click` z automatyzacji przeglądarki — cała UI jest w setkach shadow rootów. Trzeba było przechodzić przez `evaluate_script` z rekurencyjnym przeszukiwaniem shadow DOM.
- Sekcja Instant Form "Introduction" (logo/headline) wymaga obrazka jeśli włączona — prościej wyłączyć cały toggle "Logo" (lub sekcję Introduction) niż wgrywać plik przez ukryty `<input type=file>` (Chrome DevTools automation nie widzi go w a11y tree, trzeba było tymczasowo odsłaniać go CSS-em).
- Kliknięcie nie tego elementu (np. StaticText zamiast właściwego kontenera) potrafiło przypadkiem trafić w ukryty przycisk "Exit" i wywołać dialog "Are you sure you'd like to exit?" — trzeba zaznaczyć "Don't show again" i kliknąć "Stay".
- Ad-level "Text" pole czasem nie przyjmowało pełnego wpisanego tekstu przez `fill` (ucinało do przypadkowego fragmentu) — trzeba było ustawiać `.value` bezpośrednio przez JS + dispatch `input`/`change` eventów.

## Odrzucenie reklamy — błędna klasyfikacja branży (31.08.2026)

Status ad group zmienił się na "Not delivering" / "Review not approved" krótko po starcie. Powód podany przez TikToka: *"This ad group belongs to 'Real Estate Agency' industry, which requires additional documents or qualification checks"* (reguła `SA-Real Estate`, region: Saudi Arabia).

- To błędna automatyczna klasyfikacja — prawdopodobnie wyzwolona przez słowa związane z hotelem/nieruchomością w arabskim tekście reklamy lub opisie firmy. Marotino to SaaS/AI dla hotelarstwa, nie agencja nieruchomości.
- Nie ma pola "Industry" do ręcznej zmiany ani na poziomie kampanii, ani ad group, ani w ustawieniach konta — klasyfikacja jest przypisywana automatycznie przez system przeglądu treści.
- **Rozwiązanie: przycisk "Appeal"** w panelu szczegółów odrzucenia (Ads Manager → zakładka "Ad" → kliknięcie statusu "Review not approved" → "View more" → "Appeal"). Złożono odwołanie 31.08.2026 z powodem "I don't think there's a violation" i opisem wyjaśniającym, że produkt to AI recepcjonista dla hoteli, nie usługi nieruchomości. Po submicie cała ad group wraca do ponownej weryfikacji.
- Do sprawdzenia: status apelacji w ciągu 24-48h.
