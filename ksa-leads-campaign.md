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

## Brak numeru telefonu w leadach — dodane pole Phone number (31.08.2026)

Pierwszy lead (Nasru Dheen, `nasrudheen.ad56@hotmail.com`) przyszedł z samym mailem — formularz miał tylko pola imię + email, telefon nigdy nie był zbierany. Ten lead zostaje bez numeru (nie da się uzupełnić retroaktywnie).

Naprawa: w Ads Managerze → Edit ad → Instant Form → kliknięcie ikony edycji otworzyło edytor formularza (`instant_page/editor/main?type=copy&page_id=7679941286060622088`). W sekcji **Personal Information** dodano nowe pole **Q3 Phone number** (dropdown Add → Contact information → Phone number). Pola Personal Information (Email/Name/Phone) są wymagane domyślnie — nie mają przełącznika "Optional" jak pola Custom.

Ponieważ formularz był już aktywny/używany, TikTok nie pozwolił edytować go w miejscu — otworzył edytor w trybie `type=copy` i po zapisaniu ("Complete") podmienił formularz na nową wersję: **"Untitled form 8/31/26, 00:17 Copy"**, podpiętą automatycznie pod istniejący ad. Po Submit reklama wróciła do statusu "Active — Creative in edit review" (ponowna weryfikacja po edycji kreacji).

Znana pułapka UI: przycisk "+ Add" pod Personal Information to popper-trigger (`byted-popper-trigger-click`) zagnieżdżony w kilku warstwach divów — zwykły klik w a11y-tree trafiał w wewnętrzny div bez handlera; zadziałał dopiero klik na zewnętrznym `<span class="byted-popper-trigger-click byted-dropdown-trigger">` z pełną sekwencją pointerdown/mousedown/pointerup/mouseup/click.

Nierozwiązane: nowe pole ma etykiety po angielsku ("Phone number", "Enter phone number") w formularzu, który poza tym jest cały po arabsku — istniejące ostrzeżenie Ads Managera "form language is inconsistent" (sprzed tej zmiany, bo form language było ustawione na "English" mimo arabskiej treści) to uwypukla. Do rozważenia: przetłumaczenie etykiety na arabski dla spójności/konwersji.

## Odrzucenie reklamy — błędna klasyfikacja branży (31.08.2026)

Status ad group zmienił się na "Not delivering" / "Review not approved" krótko po starcie. Powód podany przez TikToka: *"This ad group belongs to 'Real Estate Agency' industry, which requires additional documents or qualification checks"* (reguła `SA-Real Estate`, region: Saudi Arabia).

- To błędna automatyczna klasyfikacja — prawdopodobnie wyzwolona przez słowa związane z hotelem/nieruchomością w arabskim tekście reklamy lub opisie firmy. Marotino to SaaS/AI dla hotelarstwa, nie agencja nieruchomości.
- Nie ma pola "Industry" do ręcznej zmiany ani na poziomie kampanii, ani ad group, ani w ustawieniach konta — klasyfikacja jest przypisywana automatycznie przez system przeglądu treści.
- **Rozwiązanie: przycisk "Appeal"** w panelu szczegółów odrzucenia (Ads Manager → zakładka "Ad" → kliknięcie statusu "Review not approved" → "View more" → "Appeal"). Złożono odwołanie 31.08.2026 z powodem "I don't think there's a violation" i opisem wyjaśniającym, że produkt to AI recepcjonista dla hoteli, nie usługi nieruchomości. Po submicie cała ad group wraca do ponownej weryfikacji.
- Do sprawdzenia: status apelacji w ciągu 24-48h.

## Odwołanie odrzucone (31.08.2026, 06:46)

Mail "Advertisement appeal status" od TikTok For Business: *"Twoje odwołanie zostało odrzucone, ponieważ reklama jest niezgodna z naszymi zasadami reklamowymi."* (Ad ID `1874987240205426`).

W panelu (Ads Manager → Ad → status "Review not approved" → "View more") widać teraz konkretny dalszy krok zamiast opcji kolejnego odwołania:

> "This ad group belongs to 'Real Estate Agency' industry, which requires additional documents or qualification checks. **Upload documents or complete the quality check for 'SA-Real Estate' to deliver your ads.**" + przycisk **"Fix it"**.

Czyli TikTok podtrzymał klasyfikację "Real Estate Agency" dla regionu Saudi Arabia i teraz wymaga albo (a) przesłania dokumentów/kwalifikacji nieruchomościowych (nierealne — Marotino nie jest agencją nieruchomości), albo (b) zmiany treści reklamy/opisu firmy tak, żeby nie wyzwalać tej klasyfikacji, i złożenia nowej reklamy od zera zamiast dalszego odwoływania się od tej samej.

Do zrobienia: zdecydować, czy przerabiać kreatywę/opis (usunąć słowa kojarzone z nieruchomościami/hotelami po arabsku) i złożyć nową reklamę, czy spróbować innego regionu/formatu.

## Przeróbka kreacji (31.08.2026)

Sprawdzony faktyczny tekst reklamy (Ads Manager → Ad → Edit → sekcja "Text (1/5)", pole 82/100 znaków, ad-level, edytowalne niezależnie od organicznego posta) — oryginał w 100% dotyczył aplikacji hotelowej, zero real estate:

> "لماذا يدعو ضيوفك الاستقبال حتى الآن؟ 📲🏨 احصل على تطبيق فندقك الخاص بنظام 'وايت ليبل'..." + hashtagi #فنادق #إدارة_الفنادق #سياحة_وفنادق itd. (dolny caption wideo, część organicznego posta TikTok @marotino_com — to jest Spark Ad, reklama recyklinguje istniejący post).

**Zmieniono edytowalne pole "Ad text"** na neutralne, bez słów hotel/turystyka/nieruchomości:
> "اكتشف تطبيق ماروتينو الذكي لخدمة العملاء بالذكاء الاصطناعي على مدار الساعة لأعمالك" (Odkryj inteligentną aplikację Marotino do obsługi klienta AI 24/7 dla Twojej firmy) — zapisane i potwierdzone w panelu.

**Ważne ograniczenie znalezione przy edycji:** to jest Spark Ad — reklama korzysta z ISTNIEJĄCEGO organicznego posta TikTok (@marotino_com, wideo 0:10s). Panel Ads Managera pokazuje ostrzeżenie: *"Selected TikTok posts will continue to use their original text."* Oznacza to, że **oryginalny caption posta (z hashtagami #فنادق/#إدارة_الفنادق/#سياحة_وفنادق) nadal jest tym, co faktycznie wyświetla się w reklamie i co przechodzi przez automatyczną klasyfikację branżową TikToka — edytowalne pole "Ad text" to tylko dodatkowy, wtórny tekst.**

Realna naprawa wymaga jednego z:
1. **Edycji podpisu oryginalnego posta** na koncie TikTok @marotino_com (poza Ads Managerem, w TikTok Studio/aplikacji) — usunięcie hashtagów #فنادق/#سياحة_وفنادق. Wpłynie to na publiczny organiczny post, nie tylko reklamę.
2. **Wgrania wideo jako świeżej kreacji** (nie jako recykling posta) w zakładce "Creative library" — obecnie pusta, więc wymaga pobrania pliku wideo i ponownego wgrania jako samodzielny asset reklamowy z czystym, kontrolowanym tekstem bez dziedziczenia captiona posta.

Sprawdzono zakładkę "Creative library" w Ads Managerze — pusta, brak alternatywnego assetu bez hashtagów. Do decyzji: czy edytować publiczny post organiczny, czy pobrać i wgrać wideo jako osobny asset.

## Rozwiązanie wdrożone (31.08.2026, druga sesja)

TikTok (web) nie pozwala edytować opisu już opublikowanego posta — sprawdzone w TikTok Studio ("..." menu → tylko Pin/Playlist/Download/Delete, brak Edit) i na stronie posta. Próba usunięcia starego posta zablokowana komunikatem: *"Videos with commercial content can only be edited on the TikTok app"* — post był powiązany z aktywną reklamą.

Wdrożone rozwiązanie:
1. Cezary dostarczył lokalny plik źródłowy `202608121355.mp4` (ten sam klip co oryginalny post, bez wypalonych hashtagów w opisie).
2. Wgrano go jako **nowy** post na @marotino_com w TikTok Studio z czystym, neutralnym opisem — bez żadnych słów hotel/turystyka/nieruchomość:
   > "لماذا يظل ضيوفك بانتظار الرد؟ 📲🤖 احصل على تطبيقك الخاص بنظام "وايت ليبل" وباسم علامتك التجارية فوراً — دون انتظار لشهور من التطوير! 🚀✨ ارتقِ بتجربة عملائك وسهّل عمل فريقك الآن. #الذكاء_الاصطناعي #تطبيقات_ذكية #وايت_ليبل #تقنية_الأعمال #خدمة_العملاء #حلول_ذكية #تطوير_تطبيقات #أعمال"
   Nowy post: `tiktok.com/@marotino_com/video/7680114383590313219`.
3. W Ads Managerze (Ad → Edit → Creative assets → Edit selections → zakładka "TikTok posts") podmieniono kreację reklamy ze starego posta na nowy.
4. Kliknięto Submit — reklama wróciła do statusu **"Pending — Review for modification"**.
5. Dopiero po podmianie kreacji dało się skasować stary post (blokada commercial-content znika, gdy żadna aktywna reklama go nie używa) — do zrobienia jako ostatni krok.

Pułapka techniczna przy edycji opisu: pole opisu w TikTok Studio to edytor Draft.js z obsługą RTL — wpisywanie przez symulowane zdarzenia klawiatury (`type_text`/pojedyncze `Backspace`) powodowało przekręcanie kolejności znaków arabskich. Zadziałało tylko `document.execCommand('delete')` + `document.execCommand('insertText', false, tekst)` w jednym wywołaniu.

Do zrobienia: sprawdzić za 24-48h czy nowa reklama przeszła weryfikację i czy zniknęła klasyfikacja "Real Estate Agency"; potem skasować stary post (7673115280951971094, 605 wyświetleń).

**Aktualizacja:** próba skasowania starego posta po podmianie kreacji dalej zablokowana tym samym komunikatem ("Videos with commercial content can only be edited on the TikTok app") — mimo że reklama już go nie używa. To trwałe ograniczenie TikToka: post raz użyty jako Spark Ad dostaje flagę "commercial content" i można go skasować **tylko z aplikacji mobilnej TikTok**, nie z web/Studio. Cezary musi skasować go ręcznie z telefonu (Profil → film → "..." → Delete).
