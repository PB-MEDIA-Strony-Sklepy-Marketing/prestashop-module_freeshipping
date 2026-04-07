# KEDAR-WIHA Free Shipping — README.md

```markdown
# 🚚 KEDAR-WIHA Free Shipping

> **PrestaShop 9.0+ Module** — Automatyczna darmowa dostawa dla zamówień powyżej 1000 zł netto

[![PrestaShop 9.0+](https://img.shields.io/badge/PrestaShop-9.0+-orange.svg)](https://www.prestashop.com)
[![PHP 8.1+](https://img.shields.io/badge/PHP-8.1+-777BB4.svg)](https://www.php.net)
[![License](https://img.shields.io/badge/License-proprietary-blue.svg)](LICENSE)
[![KEDAR‑WIHA.pl](https://img.shields.io/badge/KEDAR%E2%80%91WIHA.pl-autoryzowany%20dystrybutor-8B0000.svg)](https://kedar-wiha.pl)

---

## 📋 Spis treści

- [Opis](#-opis)
- [Funkcjonalności](#-funkcjonalności)
- [Wymagania](#-wymagania)
- [Instalacja](#-instalacja)
- [Konfiguracja](#-konfiguracja)
- [Działanie modułu](#-działanie-modułu)
- [Struktura bazy danych](#-struktura-bazy-danych)
- [Hooki PrestaShop](#-hooki-prestashop)
- [Przykłady użycia](#-przykłady-użycia)
- [Rozwiązywanie problemów](#-rozwiązywanie-problemów)
- [Rozwój i Contributing](#-rozwój-i-contributing)
- [Licencja](#-licencja)
- [Wsparcie](#-wsparcie)

---

## 🔍 Opis

**KEDAR-WIHA Free Shipping** to autorski moduł dla PrestaShop 9.0+, który automatycznie przyznaje darmową dostawę dla zamówień o wartości powyżej **1000 zł netto**.

Moduł został zaprojektowany z myślą o strategii sprzedażowej sklepu [KEDAR-WIHA.pl](https://kedar-wiha.pl) — autoryzowanego dystrybutora profesjonalnych narzędzi WIHA Polska — aby motywować klientów B2B i B2C do zwiększania wartości koszyka, jednocześnie upraszczając proces naliczania kosztów dostawy.

### 🎯 Główne zastosowania

- ✅ **Automatyzacja promocji** — brak ręcznego tworzenia kodów rabatowych
- ✅ **Motywacja do większych zamówień** — widoczny pasek postępu w koszyku
- ✅ **Wsparcie segmentu B2B** — próg 1000 zł netto dopasowany do zamówień firmowych
- ✅ **Transparentność** — klient widzi, ile brakuje do darmowej dostawy
- ✅ **Brak zewnętrznych zależności** — czysty PHP + SQL, kompatybilny z PS9

---

## ✨ Funkcjonalności

### 🎁 Zarządzanie darmową dostawą

- ✅ **Automatyczne tworzenie CartRule** — reguła `KEDAR_FREESHIP_AUTO` generowana przy instalacji
- ✅ **Próg wartości netto** — 1000 zł netto (konfigurowalne w kodzie źródłowym)
- ✅ **Darmowa dostawa bez limitu** — `free_shipping = 1`, `quantity = 0` (nielimitowana)
- ✅ **Ważność długoterminowa** — reguła aktywna przez 10 lat od instalacji
- ✅ **Bezpieczeństwo** — walidacja przez `Db::getInstance()` i `pSQL()`

### 📊 Wizualizacja w koszyku i checkout

- ✅ **Pasek postępu** — dynamiczny wskaźnik `%` do progu darmowej dostawy
- ✅ **Komunikat kontekstowy** — informacja o pozostałej kwocie lub potwierdzenie aktywacji
- ✅ **Hooki frontend** — `displayShoppingCartFooter`, `displayCheckoutSummaryTop`
- ✅ **Szablon Smarty** — separacja logiki od prezentacji (`free_shipping_bar.tpl`)

### ⚙️ Panel administracyjny i zarządzanie

- ✅ **Automatyczna instalacja** — moduł tworzy regułę koszyka bez interwencji admina
- ✅ **Bezpieczne odinstalowanie** — dane CartRule pozostają w bazie (możliwość ręcznego usunięcia)
- ✅ **Logika idempotentna** — wielokrotna instalacja nie tworzy duplikatów reguł

### 🚀 Performance & Security

- ✅ **Brak zewnętrznych zależności** — czysty PHP, brak Composer packages
- ✅ **Query z indeksami** — szybkie pobieranie reguł przez `code` i `active`
- ✅ **Kontekstowa renderacja** — pasek wyświetlany tylko w koszyku/checkout
- ✅ **Output buforowany przez Smarty** — kompatybilny z systemem cachingu PS9

---

## 📦 Wymagania

| Komponent | Wymagana wersja | Uwagi |
|-----------|----------------|-------|
| **PrestaShop** | `≥ 9.0.0` | Testowano na 9.0.3 z motywem Optima 3.3.0 |
| **PHP** | `≥ 8.1` | Zalecane 8.2+ dla optymalnej wydajności |
| **MySQL/MariaDB** | `≥ 5.7` / `≥ 10.3` | Engine: InnoDB, charset: `utf8mb4_unicode_ci` |
| **Smarty** | `≥ 4.x` | Domyślny silnik szablonów w PS9 |

---

## 🚀 Instalacja

### Metoda 1: Przez Back Office (rekomendowana)

1. Pobierz archiwum modułu: `kedarwiha_freeshipping.zip`
2. Zaloguj się do Back Office PrestaShop
3. Przejdź do: **Moduły → Menedżer modułów**
4. Kliknij **"Prześlij moduł"** i wybierz plik `.zip`
5. Po instalacji moduł automatycznie:
   - Utworzy regułę koszyka `KEDAR_FREESHIP_AUTO`
   - Zarejestruje hooki `displayShoppingCartFooter` i `displayCheckoutSummaryTop`

### Metoda 2: Manualna (FTP/SFTP)

```bash
# 1. Sklonuj repozytorium lub pobierz pliki
git clone https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/prestashop-module_freeshipping.git

# 2. Przenieś folder do katalogu modułów PrestaShop
cp -r prestashop-module_freeshipping /path/to/prestashop/modules/kedarwiha_freeshipping

# 3. Ustaw uprawnienia
chmod -R 755 /path/to/prestashop/modules/kedarwiha_freeshipping

# 4. Zainstaluj przez Back Office lub CLI
php bin/console prestashop:module:install kedarwiha_freeshipping
```

### ✅ Weryfikacja instalacji

1. Przejdź do: **Zasady cenowe → Reguły koszyka**
2. Wyszukaj regułę o kodzie: `KEDAR_FREESHIP_AUTO`
3. Powinieneś zobaczyć:
   - Nazwa: `Darmowa dostawa KEDAR-WIHA`
   - Darmowa dostawa: ✅ Tak
   - Minimalna kwota zakupu: `1000.00 zł` (netto)
   - Status: ✅ Włączona

---

## ⚙️ Konfiguracja

### Domyślne ustawienia (hardcoded)

Moduł używa stałych zdefiniowanych w klasie:

```php
class Kedarwiha_Freeshipping extends Module
{
    const FREE_SHIPPING_THRESHOLD = 1000.00; // zł netto
    // ...
}
```

### Zmiana progu darmowej dostawy

Aby zmienić próg z 1000 zł na inną wartość:

1. Otwórz plik: `modules/kedarwiha_freeshipping/kedarwiha_freeshipping.php`
2. Znajdź linię:
   
   ```php
   const FREE_SHIPPING_THRESHOLD = 1000.00;
   ```
3. Zmień wartość, np.:
   
   ```php
   const FREE_SHIPPING_THRESHOLD = 500.00; // 500 zł netto
   ```
4. Zapisz plik i wyczyść cache PrestaShop:
   - **Zaawansowane → Wydajność → Wyczyść cache**

> ⚠️ **Uwaga:** Po zmianie progu należy **przeinstalować moduł** lub ręcznie zaktualizować regułę `KEDAR_FREESHIP_AUTO` w tabeli `PREFIX_cart_rule`, aby zmiana została zastosowana.

### Wyłączenie modułu

1. Przejdź do: **Moduły → Menedżer modułów**
2. Znajdź: **KEDAR-WIHA Free Shipping**
3. Kliknij: **Wyłącz**

> ℹ️ Wyłączenie modułu nie usuwa reguły koszyka — klient nadal może otrzymać darmową dostawę, jeśli reguła jest aktywna. Aby całkowicie wyłączyć promocję, należy ręcznie dezaktywować regułę `KEDAR_FREESHIP_AUTO` w **Zasady cenowe → Reguły koszyka**.

---

## 🛠️ Działanie modułu

### Architektura przepływu danych

```
[Koszyk klienta]
       │
       ▼
[Hook: displayShoppingCartFooter]
       │
       ▼
[Kedarwiha_Freeshipping::renderBar()]
       │
       ├── Pobierz total netto koszyka: $cart->getOrderTotal(false, Cart::ONLY_PRODUCTS)
       ├── Porównaj z progiem: FREE_SHIPPING_THRESHOLD
       ├── Oblicz: remaining = threshold - total, percent = (total/threshold)*100
       │
       ▼
[Smarty Template: free_shipping_bar.tpl]
       │
       ├── Jeśli total >= threshold → pokaż "✅ Darmowa dostawa aktywna!"
       ├── Jeśli total < threshold → pokaż pasek postępu + "Brakuje X zł do darmowej dostawy"
       │
       ▼
[Renderowany HTML w koszyku / checkout]
```

### Struktura renderowanego outputu (przykładowy HTML)

```html
<!-- free_shipping_bar.tpl -->
<div class="kw-free-shipping-bar" data-threshold="1000" data-currency="zł">

  <!-- Stan: brak darmowej dostawy -->
  <div class="kw-fs-progress">
    <div class="kw-fs-label">
      Brakuje <strong>245,00 zł</strong> do darmowej dostawy
    </div>
    <div class="kw-fs-bar">
      <div class="kw-fs-fill" style="width: 75.5%"></div>
    </div>
    <small class="kw-fs-hint">Dodaj produkty za 245,00 zł netto, aby otrzymać darmową dostawę</small>
  </div>

  <!-- Stan: darmowa dostawa aktywna (alternatywa) -->
  <!--
  <div class="kw-fs-success">
    <i class="material-icons">check_circle</i>
    <strong>Gratulacje!</strong> Otrzymujesz darmową dostawę dla tego zamówienia.
  </div>
  -->
</div>
```

### Logika naliczania wartości koszyka

Moduł używa metody PrestaShop do pobrania sumy netto produktów:

```php
$totalNett = (float) $cart->getOrderTotal(false, Cart::ONLY_PRODUCTS);
```

| Parametr              | Wartość        | Opis                                         |
| --------------------- | -------------- | -------------------------------------------- |
| `false`               | Bez podatku    | Obliczenia na kwotach netto (B2B)            |
| `Cart::ONLY_PRODUCTS` | Tylko produkty | Wyklucza koszty dostawy, opakowania, podatki |

> 💡 **Dlaczego netto?** Sklep KEDAR-WIHA.pl obsługuje klientów B2B z cenami netto. Próg 1000 zł netto zapewnia spójność z polityką cenową i fakturami VAT.

---

## 🗄️ Struktura bazy danych

### Tabela: `PREFIX_cart_rule` (standard PrestaShop)

Moduł nie tworzy własnych tabel — wykorzystuje istniejącą strukturę `cart_rule` PrestaShop.

#### Przykładowy rekord utworzony przez moduł:

```sql
INSERT INTO `PREFIX_cart_rule` (
  `code`, `name`, `active`, `quantity`, `quantity_per_user`,
  `minimum_amount`, `minimum_amount_tax`, `minimum_amount_shipping`,
  `free_shipping`, `date_from`, `date_to`, `highlight`
) VALUES (
  'KEDAR_FREESHIP_AUTO',
  'Darmowa dostawa KEDAR-WIHA',
  1, 0, 0,
  1000.00, 0, 0,
  1,
  '2025-03-01 00:00:00',
  '2035-03-01 00:00:00',
  0
);
```

#### Opis kluczowych pól:

| Kolumna                 | Wartość                      | Opis                                                             |
| ----------------------- | ---------------------------- | ---------------------------------------------------------------- |
| `code`                  | `KEDAR_FREESHIP_AUTO`        | Unikalny identyfikator reguły (używany do sprawdzania istnienia) |
| `name`                  | `Darmowa dostawa KEDAR-WIHA` | Nazwa widoczna w koszyku i podsumowaniu zamówienia               |
| `active`                | `1`                          | Status: aktywna/nieaktywna                                       |
| `minimum_amount`        | `1000.00`                    | Próg wartości zamówienia (netto, bo `minimum_amount_tax = 0`)    |
| `free_shipping`         | `1`                          | Flag: darmowa dostawa dla reguły                                 |
| `date_from` / `date_to` | 10-letni zakres              | Okres ważności reguły                                            |

### Indeksy wydajności (domyślne w PrestaShop)

```sql
-- Sprawdzenie indeksów na tabeli cart_rule
SHOW INDEX FROM `PREFIX_cart_rule`;
-- Oczekiwane indeksy:
-- PRIMARY KEY (`id_cart_rule`)
-- KEY `code` (`code`) ← kluczowe dla szybkiego lookupu reguły
-- KEY `active` (`active`)
```

---

## 🔗 Hooki PrestaShop

Moduł rejestruje i obsługuje następujące hooki:

### Frontend hooks

| Hook                        | Plik szablonu                                | Opis                                           |
| --------------------------- | -------------------------------------------- | ---------------------------------------------- |
| `displayShoppingCartFooter` | `views/templates/hook/free_shipping_bar.tpl` | Pasek postępu na dole strony koszyka           |
| `displayCheckoutSummaryTop` | `views/templates/hook/free_shipping_bar.tpl` | Pasek postępu na górze podsumowania zamówienia |

### Rejestracja hooków (kod źródłowy)

```php
private function registerHooks(): bool
{
    $hooks = [
        'displayShoppingCartFooter',
        'displayCheckoutSummaryTop',
    ];

    foreach ($hooks as $hook) {
        if (!$this->registerHook($hook)) {
            continue; // Nie przerywaj instalacji jeśli hook nie istnieje
        }
    }
    return true;
}
```

> 💡 **Tip:** Jeśli potrzebujesz wyświetlać pasek w dodatkowych miejscach (np. `displayCartExtraContent`), dodaj nazwę hooka do tablicy `$hooks` i zaimplementuj metodę `hookDisplayCartExtraContent()` analogicznie do istniejących.

---

## 💡 Przykłady użycia

### 🎯 Przykład 1: Klient dodaje produkty za 750 zł netto

**Stan koszyka:**

- Wartość netto: `750.00 zł`
- Próg darmowej dostawy: `1000.00 zł`
- Brakująca kwota: `250.00 zł`
- Procent postępu: `75%`

**Wyświetlony komunikat:**

```
┌─────────────────────────────────────┐
│ 🚚 Brakuje 250,00 zł do darmowej dostawy │
│ ███████████████████████░░░░░░░░ 75%     │
│ Dodaj produkty za 250,00 zł netto,       │
│ aby otrzymać darmową dostawę.           │
└─────────────────────────────────────┘
```

**Efekt biznesowy:** Klient dodaje wkrętaki VDE za 280 zł → przekracza próg → otrzymuje darmową dostawę → zwiększa AOV (Average Order Value).

---

### 🎯 Przykład 2: Klient osiąga próg 1000 zł netto

**Stan koszyka:**

- Wartość netto: `1 245.00 zł`
- Próg darmowej dostawy: `1000.00 zł`
- Status: ✅ Darmowa dostawa aktywna

**Wyświetlony komunikat:**

```
┌─────────────────────────────────────┐
│ ✅ Gratulacje!                       │
│ Otrzymujesz darmową dostawę          │
│ dla tego zamówienia.                │
│                                     │
│ 🎁 Zaoszczędzono: ~29,00 zł         │
└─────────────────────────────────────┘
```

**Efekt UX:** Pozytywne wzmocnienie (positive reinforcement) zwiększa satysfakcję klienta i prawdopodobieństwo powrotu.

---

### 🎯 Przykład 3: Testowanie modułu w środowisku dev

```bash
# 1. Włącz tryb debugowania w PrestaShop
# Edytuj: /config/defines.inc.php
define('_PS_MODE_DEV_', true);

# 2. Dodaj testowe produkty do koszyka
# - Wkrętak VDE WIHA 3,0×80 mm: 39,99 zł netto × 10 szt. = 399,90 zł
# - Klucz dynamometryczny TorqueVario-S: 599,00 zł netto × 1 szt. = 599,00 zł
# SUMA: 998,90 zł (brakuje 1,10 zł do progu)

# 3. Sprawdź w koszyku:
# - Czy pasek postępu pokazuje 99.89%?
# - Czy komunikat informuje o brakujących 1,10 zł?

# 4. Dodaj dowolny produkt za >1,10 zł
# - Sprawdź, czy pasek zmienia się na "✅ Darmowa dostawa aktywna!"
# - Sprawdź w podsumowaniu zamówienia: koszt dostawy = 0,00 zł
```

---

## 🔧 Rozwiązywanie problemów

### ❌ Darmowa dostawa nie aktywuje się po przekroczeniu progu

1. **Sprawdź wartość koszyka** — czy liczysz netto czy brutto?
   
   - Moduł używa: `$cart->getOrderTotal(false, Cart::ONLY_PRODUCTS)` → netto
   - Jeśli klient widzi ceny brutto, przelicz: `1000 zł netto = 1230 zł brutto (23% VAT)`

2. **Sprawdź status reguły** — czy `KEDAR_FREESHIP_AUTO` jest aktywna?
   
   - Przejdź: **Zasady cenowe → Reguły koszyka**
   - Wyszukaj: `KEDAR_FREESHIP_AUTO`
   - Upewnij się: ✅ Aktywna, ✅ Darmowa dostawa, ✅ Minimalna kwota: 1000.00

3. **Wyczyść cache PrestaShop**:
   
   - **Zaawansowane → Wydajność → Wyczyść cache**
   - Lub przez CLI: `php bin/console cache:clear --env=prod`

4. **Sprawdź konflikty z innymi regułami**:
   
   - Czy inne reguły koszyka nie mają wyższego priorytetu?
   - Czy nie ma reguł wykluczających darmową dostawę?

### ❌ Pasek postępu nie wyświetla się w koszyku

1. **Sprawdź hooki** — czy moduł jest przypisany do `displayShoppingCartFooter`?
   
   - Przejdź: **Moduły → Menedżer modułów → KEDAR-WIHA Free Shipping → Pozycje hooków**
   - Upewnij się: ✅ `displayShoppingCartFooter`, ✅ `displayCheckoutSummaryTop`

2. **Sprawdź szablon Smarty**:
   
   - Plik: `modules/kedarwiha_freeshipping/views/templates/hook/free_shipping_bar.tpl`
   - Upewnij się, że plik istnieje i ma uprawnienia do odczytu (`644`)

3. **Sprawdź zmienne Smarty**:
   
   - W `renderBar()` przypisujesz: `kw_fs_total_nett`, `kw_fs_threshold`, `kw_fs_remaining`, `kw_fs_percent`
   - W szablonie używasz: `{$kw_fs_remaining}`, `{$kw_fs_percent}` — czy nazwy się zgadzają?

### ❌ Błąd przy instalacji / aktualizacji

1. **Sprawdź uprawnienia do bazy danych** — użytkownik DB musi mieć uprawnienia `INSERT` do tabeli `PREFIX_cart_rule`

2. **Sprawdź logi PrestaShop**:
   
   - `/var/logs/prod.log` lub w Back Office: **Zaawansowane → Logi**

3. **Jeśli reguła już istnieje** — moduł używa `SELECT id_cart_rule ... WHERE code = 'KEDAR_FREESHIP_AUTO'` aby uniknąć duplikatów. Jeśli instalacja się nie powiedzie, ręcznie usuń istniejącą regułę i spróbuj ponownie.

---

## 🧑‍💻 Rozwój i Contributing

### Struktura projektu

```
kedarwiha_freeshipping/
├── kedarwiha_freeshipping.php    # Główny plik modułu (logika + hooki)
├── logo.png                      # Ikona modułu w Back Office
├── views/
│   └── templates/
│       └── hook/
│           └── free_shipping_bar.tpl  # Szablon Smarty dla paska postępu
├── README.md                     # Ten plik
├── LICENSE                       # Licencja
└── .gitignore                    # Reguły ignorowania plików
```

### Zasady kodowania

- ✅ Przestrzegaj [PrestaShop Coding Standards](https://devdocs.prestashop-project.org/9/modules/concepts/coding-standards/)
- ✅ Używaj `pSQL()` do sanitizacji zapytań SQL
- ✅ Unikaj bezpośrednich zapytań — używaj `Db::getInstance()->executeS()`
- ✅ Dokumentuj publiczne metody PHPDoc
- ✅ Testuj na PrestaShop 9.0.3 przed commitowaniem zmian

### Dodawanie nowych hooków

Aby dodać obsługę nowego hooka (np. `displayCartExtraContent`):

1. Otwórz plik `kedarwiha_freeshipping.php`
2. W metodzie `install()` dodaj rejestrację hooka:
   
   ```php
   && $this->registerHook('displayCartExtraContent')
   ```
3. Dodaj metodę obsługującą hook:
   
   ```php
   public function hookDisplayCartExtraContent(array $params): string
   {
       return $this->renderBar();
   }
   ```
4. Zapisz plik i przeinstaluj moduł (lub ręcznie dodaj hook w tabeli `PREFIX_hook_module`)

### Testowanie lokalne

```bash
# 1. Sklonuj repozytorium
git clone https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/prestashop-module_freeshipping.git

# 2. Stwórz symlink do lokalnej instalacji PrestaShop
ln -s /path/to/module kedarwiha_freeshipping /path/to/prestashop/modules/

# 3. Włącz tryb debugowania w PrestaShop
# Edytuj /config/defines.inc.php:
define('_PS_MODE_DEV_', true);

# 4. Testuj zmiany w Back Office → Moduły
```

### Pull Request workflow

1. **Forkuj** repozytorium
2. **Utwórz branch** z opisową nazwą: `feat/add-display-cart-extra-content-hook`
3. **Wprowadź zmiany** zgodnie z coding standards
4. **Przetestuj** na lokalnej instancji PS 9.0.3:
   - Koszyk z wartością < 1000 zł → pasek postępu
   - Koszyk z wartością ≥ 1000 zł → komunikat sukcesu
5. **Commituj** z czytelnym message: `feat: add support for displayCartExtraContent hook`
6. **Otwórz PR** z opisem zmian, screenshotami paska postępu i listą testów

---

## 📜 Licencja

Ten moduł jest własnością **KEDAR-WIHA.pl** i **PB-MEDIA Strony | Sklepy | Marketing**.

```
Copyright © 2025 KEDAR-WIHA.pl
Wszelkie prawa zastrzeżone.

Niniejszy moduł jest przeznaczony wyłącznie do użytku w ramach sklepu 
internetowego KEDAR-WIHA.pl oraz projektów realizowanych przez PB-MEDIA.

Kopiowanie, modyfikowanie lub dystrybucja kodu bez pisemnej zgody 
właściciela jest zabroniona.
```

> ℹ️ W przypadku zainteresowania komercyjnym wykorzystaniem modułu, prosimy o kontakt: **info@kedar-wiha.pl**

---

## 🆘 Wsparcie

### 📞 Kontakt techniczny

| Kanał             | Dane                                                                                                           |
| ----------------- | -------------------------------------------------------------------------------------------------------------- |
| **E-mail**        | [info@kedar-wiha.pl](mailto:info@kedar-wiha.pl)                                                                |
| **Telefon**       | [+48 575 838 766](tel:+48575838766)                                                                            |
| **WhatsApp**      | [wa.me/48575838766](https://wa.me/48575838766)                                                                 |
| **GitHub Issues** | [Otwórz zgłoszenie](https://github.com/PB-MEDIA-Strony-Sklepy-Marketing/prestashop-module_freeshipping/issues) |

### 🔗 Przydatne linki

- 🏪 [Sklep KEDAR-WIHA.pl](https://kedar-wiha.pl)
- 📘 [Dokumentacja PrestaShop 9](https://devdocs.prestashop-project.org/9/)
- 🎨 [Brandbook KEDAR-WIHA.pl](https://github.com/piotroq/DOCS/)
- 🧩 [Repozytorium modułów](https://github.com/piotroq/MODULES/)

### 🐛 Zgłaszanie błędów

Przed zgłoszeniem błędu upewnij się, że:

1. Używasz najnowszej wersji modułu
2. Sprawdziłeś sekcję [Rozwiązywanie problemów](#-rozwiązywanie-problemów)
3. Przetestowałeś na czystej instalacji PrestaShop 9.0.3 (jeśli możliwe)

**W zgłoszeniu podaj:**

- Wersję PrestaShop i PHP
- Krok po kroku opis reprodukcji błędu
- Oczekiwany vs. rzeczywisty rezultat
- Zrzuty ekranu / logi błędów (jeśli dotyczy)

---

## 🔄 Historia wersji

| Wersja  | Data    | Zmiany                                                                                                                                                                                                                                                                                                                                          |
| ------- | ------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `1.0.0` | 2025-03 | 🎉 Pierwsza wersja produkcyjna<br>✅ Automatyczne tworzenie CartRule `KEDAR_FREESHIP_AUTO`<br>✅ Próg 1000 zł netto (konfigurowalny w kodzie)<br>✅ Pasek postępu w koszyku i checkout (`displayShoppingCartFooter`, `displayCheckoutSummaryTop`)<br>✅ Szablon Smarty `free_shipping_bar.tpl`<br>✅ Brak zewnętrznych zależności — czysty PHP + SQL |

---

> **KEDAR-WIHA.pl** — Autoryzowany Dystrybutor Narzędzi WIHA Polska  
> 🛠️ Narzędzia VDE • Klucze dynamometryczne • Zestawy BHP  
> 🌐 [kedar-wiha.pl](https://kedar-wiha.pl) | 📧 info@kedar-wiha.pl | 📞 +48 575 838 766

```
---

✅ **Plik `README.md` jest gotowy do:**
1. Pobrania i zapisania w repozytorium GitHub
2. Zacommitowania:  
   ```bash
   git add README.md
   git commit -m "docs: add comprehensive README.md with usage examples"
   git push origin main
```

3. Wyświetlenia jako główna dokumentacja modułu na GitHub

📌 **Rekomendacja:** Dodaj plik `LICENSE` z treścią licencji, `.gitignore` z regułami dla PrestaShop modules oraz szablon `views/templates/hook/free_shipping_bar.tpl` przed pierwszym pushem.

Darmowa dostawa w sklepie PrestaShop 9.0+ od 1000 zł netto.
