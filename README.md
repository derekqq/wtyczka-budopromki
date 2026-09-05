# Budopromki – porównywarka cen materiałów budowlanych

Budopromki to lekkie rozszerzenie Chrome, które pomaga porównać ceny tego samego
produktu w popularnych sklepach budowlanych. Na stronie produktu odczytuje EAN/GTIN,
pyta serwis Budopromki o aktualne oferty i pokazuje ceny z Leroy Merlin, Castoramy
oraz Bricomarché.

> Projekt jest podstawowym repozytorium demonstracyjnym rozszerzenia Manifest V3.
> Dane cenowe pochodzą z API Budopromki, a nie z lokalnego pliku w rozszerzeniu.

## Dlaczego Budopromki?

Przed zakupem materiałów budowlanych użytkownik chce wiedzieć nie tylko, **gdzie
najtańszy jest produkt**, ale też czy jest dostępny, czy można odebrać go lokalnie
i ile wyniesie dostawa. Rozszerzenie skraca drogę od oglądanej oferty do
porównania cen w innych sklepach.

Najważniejsze zastosowania:

- porównywarka cen materiałów budowlanych;
- porównanie cen Leroy Merlin, Castorama i Bricomarché;
- sprawdzenie, gdzie kupić produkt najtaniej;
- szybkie wyszukiwanie promocji budowlanych;
- porównanie ceny online, odbioru osobistego i dostępności lokalnej;
- weryfikacja ofert po EAN/GTIN zamiast po podobnie brzmiącej nazwie.

## Funkcje

- wykrywanie EAN/GTIN z metadanych strony i JSON-LD;
- przycisk **Porównaj ceny w innych sklepach** na obsługiwanych stronach;
- sortowanie ofert od najtańszej;
- oznaczenie najtańszej znalezionej oferty;
- link do źródła oferty otwierany w nowej karcie;
- prosty panel rozszerzenia z listą obsługiwanych sklepów;
- Manifest V3 i minimalny zakres uprawnień;
- brak zdalnego kodu JavaScript.

## Obsługiwane sklepy

W wersji podstawowej rozszerzenie działa na stronach:

- Leroy Merlin;
- Castorama;
- Bricomarché.

Lista może zostać rozszerzona, jeśli API oraz sposób identyfikacji produktu danego
sklepu zostaną przygotowane i przetestowane. Samo dodanie domeny do manifestu nie
gwarantuje poprawnego odczytu EAN-u.

## Instalacja z GitHub jako rozszerzenie Chrome

To jest instalacja developerska rozszerzenia z kodu źródłowego. Przed instalacją
sprawdź repozytorium, uprawnienia w `manifest.json` i domeny, z którymi rozszerzenie
się komunikuje.

### 1. Pobierz repozytorium

Przez Git:

```bash
git clone https://github.com/TWOJ-LOGIN/wtyczka-budopromki.git
cd wtyczka-budopromki
```

Albo wybierz na GitHubie **Code → Download ZIP** i rozpakuj archiwum.

### 2. Zbuduj katalog instalacyjny

Wymagany jest Node.js 18 lub nowszy:

```bash
npm install
npm run check
npm run build
```

Powstanie katalog `dist/`. Jeśli pobrany ZIP zawiera już `dist/`, krok budowania
możesz pominąć.

### 3. Wczytaj rozszerzenie w Chrome

1. Otwórz `chrome://extensions/`.
2. Włącz **Tryb dewelopera** w prawym górnym rogu.
3. Kliknij **Załaduj rozpakowane**.
4. Wybierz folder `dist/` z tego repozytorium.
5. Przypnij Budopromki do paska rozszerzeń, jeśli chcesz mieć szybki dostęp.

Po zmianach w kodzie uruchom ponownie `npm run build`, a następnie kliknij
**Odśwież** na karcie rozszerzenia w `chrome://extensions/`.

## Instalacja bez Node.js

Jeżeli repozytorium ma przygotowany katalog `dist/`, można zainstalować je bez
instalowania zależności:

1. pobierz ZIP z GitHuba;
2. rozpakuj go;
3. w `chrome://extensions/` wybierz **Załaduj rozpakowane**;
4. wskaż katalog `dist/`.

W tym wariancie nie edytuj plików wygenerowanych po kompilacji bezpośrednio — zmiany
wprowadzaj w `src/`, a następnie buduj projekt lokalnie.

## Używanie rozszerzenia

1. Otwórz kartę produktu w Leroy Merlin, Castoramie lub Bricomarché.
2. Poczekaj na załadowanie strony.
3. Kliknij **Porównaj ceny w innych sklepach**.
4. Sprawdź sklep, cenę, oznaczenie najtańszej oferty i link do źródła.

Jeżeli przycisk się nie pojawia, strona prawdopodobnie nie udostępnia poprawnego
EAN/GTIN albo produkt nie jest stroną produktu. W takim przypadku sprawdź konsolę
DevTools i upewnij się, że oferta istnieje w API.

## Jak porównywać sklepy uczciwie?

Porównanie powinno dotyczyć tego samego produktu, a nie tylko produktów podobnych.
Budopromki używa EAN/GTIN jako podstawowego identyfikatora. Przed wskazaniem
najtańszej oferty warto sprawdzić:

1. markę, model i wariant;
2. pojemność, wymiary i liczbę sztuk w opakowaniu;
3. cenę brutto oraz cenę jednostkową;
4. koszt dostawy i możliwość darmowego odbioru;
5. dostępność w wybranym sklepie;
6. datę ostatniej aktualizacji ceny;
7. czy ofertę sprzedaje sklep, czy zewnętrzny marketplace.

Szczegółowa metodologia znajduje się w [docs/comparison-methodology.md](docs/comparison-methodology.md).

## API

Service worker korzysta z endpointu:

```text
GET https://ext.budopromki.pl/api/products/ean/{EAN}
```

Oczekiwany skrócony format odpowiedzi:

```json
{
  "ean": "0590123456789",
  "offers": [
    {
      "network": "castorama",
      "price": 149.99,
      "currency": "PLN",
      "productUrl": "https://example.com/produkt"
    }
  ]
}
```

Jeżeli domena lub kształt API się zmieni, zaktualizuj `API_BASE_URL` w
`src/background.js` oraz opis w tym README.

## SEO i strategia treści projektu

Repozytorium jest przygotowane tak, aby wspierać stronę produktu i dokumentację
widoczną w wyszukiwarce. Najważniejsze klastry słów kluczowych to:

### Porównywarka i narzędzie

- porównywarka cen materiałów budowlanych;
- porównywarka cen sklepów budowlanych;
- porównaj ceny w sklepach budowlanych;
- gdzie najtaniej kupić materiały budowlane;
- najniższa cena produktu budowlanego.

### Sklepy i porównania

- Leroy Merlin czy Castorama — gdzie taniej;
- Leroy Merlin Castorama porównanie cen;
- Castorama czy Bricomarché;
- ceny materiałów budowlanych w sklepach;
- promocje budowlane online.

### Produkt i intencja zakupowa

- gdzie najtaniej kupić [produkt];
- [produkt] cena Leroy Merlin Castorama;
- [produkt] promocja sklep budowlany;
- cena online a cena w sklepie stacjonarnym;
- odbiór materiałów budowlanych w sklepie.

Pełna mapa fraz i rekomendowanych stron znajduje się w
[`docs/seo-keywords.md`](docs/seo-keywords.md). Nie należy publikować wielu stron,
które różnią się tylko nazwą sklepu lub miasta. Strona porównawcza powinna mieć
unikalną metodologię, aktualne dane, źródła i rzeczywistą wartość dla użytkownika.

### Zalecana architektura stron serwisu

- `/porownywarka-cen/` — strona narzędzia i wyjaśnienie działania;
- `/porownaj/leroy-merlin-vs-castorama/` — realne porównanie sklepów;
- `/promocje-budowlane/` — aktualne promocje z datą odświeżenia;
- `/produkt/{slug}/` — porównanie konkretnego produktu po EAN/GTIN;
- `/blog/jak-porownywac-ceny/` — poradnik edukacyjny;
- `/wtyczka/` — instalacja, prywatność i FAQ użytkownika.

Dla publicznych stron produktowych można stosować JSON-LD `Product` i `Offer`, a
dla list ofert `ItemList`, pod warunkiem że dane strukturalne są zgodne z treścią
widoczną na stronie. Nie dodawaj danych o cenie lub dostępności, których użytkownik
nie może zweryfikować.

## SEO GitHuba

Przed publikacją repozytorium uzupełnij:

- opis repozytorium: `Chrome extension for comparing building-material prices`;
- tematy GitHuba: `chrome-extension`, `manifest-v3`, `price-comparison`,
  `building-materials`, `ecommerce`, `seo`, `poland`;
- link do strony Budopromki i instrukcji instalacji;
- release z numerem wersji i krótkim changelogiem;
- screenshot panelu porównywarki, gdy będzie gotowy;
- sekcję bezpieczeństwa i politykę prywatności.

Opis repozytorium i README powinny używać języka użytkownika. Sama lista słów
kluczowych nie zastępuje działającego narzędzia, aktualnych danych ani dobrego
opisu porównania.

## Prywatność i uprawnienia

Rozszerzenie:

- odczytuje tylko dane potrzebne do rozpoznania produktu na obsługiwanej stronie;
- wysyła do API numer EAN/GTIN, aby pobrać oferty;
- nie wymaga logowania ani dostępu do historii przeglądania;
- nie używa zdalnego kodu JavaScript;
- otwiera link afiliacyjny/ofertowy dopiero po działaniu użytkownika.

Przed publikacją produkcyjną dodaj osobny dokument polityki prywatności i upewnij
się, że opis faktycznie odpowiada kodowi oraz konfiguracji API.

## Struktura repozytorium

```text
.
├── docs/
│   ├── comparison-methodology.md
│   └── seo-keywords.md
├── scripts/
│   └── build.mjs
├── src/
│   ├── background.js
│   ├── content.css
│   ├── content.js
│   ├── popup.css
│   ├── popup.html
│   └── popup.js
├── manifest.json
├── package.json
└── README.md
```

## Rozwój

```bash
npm run check
npm run build
```

Po zbudowaniu testuj co najmniej:

- stronę produktu z poprawnym EAN-em;
- produkt bez EAN-u;
- brak ofert w API;
- błąd API;
- ofertę z ceną i bez ceny;
- działanie przy powiększeniu strony i na małym ekranie.

## Roadmap

- preferowany sklep i lokalizacja odbioru;
- filtrowanie online / odbiór osobisty;
- historia zmian ceny;
- obsługa większej liczby sklepów;
- testy selektorów EAN dla każdego sklepu;
- wersja Firefox po przygotowaniu osobnego manifestu;
- screenshoty, demo i publiczne release'y na GitHubie.

## Licencja

Kod jest dostępny na licencji MIT. Zobacz [LICENSE](LICENSE).

