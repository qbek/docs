# Python Playwright 

<!-- vscode-markdown-toc -->
* 1. [Setup środowiska](#Setuprodowiska)
	* 1.1. [Dokumentacja](#Dokumentacja)
	* 1.2. [Instalacja narzędzi](#Instalacjanarzdzi)
	* 1.3. [Konfiguracja](#Konfiguracja)
	* 1.4. [Hello world](#Helloworld)
* 2. [pyTest fixtures](#pyTestfixtures)
	* 2.1. [Hooks Before... After...](#HooksBefore...After...)
	* 2.2. [Wstrzykiwanie zależności](#Wstrzykiwaniezalenoci)
	* 2.3. [Scope](#Scope)
* 3. [Selectory](#Selectory)
	* 3.1. [Selectory wbudowane](#Selectorywbudowane)
	* 3.2. [ Locatory CSS](#LocatoryCSS)
* 4. [Akcje](#Akcje)
* 5. [Asercje](#Asercje)
* 6. [Kontekst przeglądarki](#Kontekstprzegldarki)
	* 6.1. [Tworzenie kontekstów](#Tworzeniekontekstw)
	* 6.2. [Sesje](#Sesje)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->


##  1. <a name='Setuprodowiska'></a>Setup środowiska

###  1.1. <a name='Dokumentacja'></a>Dokumentacja

* [pytest](https://docs.pytest.org/en/stable/contents.html)
* [Playwright](https://playwright.dev/docs/intro)
* [CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)
* [HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)

###  1.2. <a name='Instalacjanarzdzi'></a>Instalacja narzędzi
Wymagane bilbioteki

```bash
pip install pytest
pip install playwright
pip install pytest-playwright

```

Inicjalizacja PlayWright (instalacja przeglądarek)

```bash
playwright install
```

###  1.3. <a name='Konfiguracja'></a>Konfiguracja

Plik ```pytest.ini```

```ini
[pytest]
python_files = *Test.py
addopts = -s --headed --browser firefox
```

###  1.4. <a name='Helloworld'></a>Hello world

```python
import pytest
from playwright.sync_api import Page

def test_helloWorld(page: Page):
     page.goto('https://google.com')
```

##  2. <a name='pyTestfixtures'></a>pyTest fixtures

Mechanizm *Dependency Injection*

###  2.1. <a name='HooksBefore...After...'></a>Hooks Before... After...

```python
@pytest.fixture
def befor_and_after():
     # blok kodu przed testem

     yield # przekazanie sterowania do testu

     # blok kodu po teście
```

###  2.2. <a name='Wstrzykiwaniezalenoci'></a>Wstrzykiwanie zależności
```python
@pytest.fixture
def dependency_injection():
     #  przygotowanie danych

     return data

@pytest.fixture
def befor_and_after_with_injecttio():
     # blok kodu przed testem
     # przygotowanie dancyh

     yield data

     # blok kodu po teście
     # posprzatanie danych
```

###  2.3. <a name='Scope'></a>Scope

Długość życia obiektów stworznych przez *fixture*, jak i moment odpalenia haków *Before* i *After* możemy kontrolować przy pomocy parametru scope:

```python

@pytest.fixture(scope="function") #domyślny scope - jeden test
@pytest.fixture(scope="class")
@pytest.fixture(scope="module")
@pytest.fixture(scope="package")
@pytest.fixture(scope="session") # wszystkie odpalone testy
```


##  3. <a name='Selectory'></a>Selectory

###  3.1. <a name='Selectorywbudowane'></a>Selectory wbudowane
Wbudowane locatory w Playwright:
[Dokuemntacja](https://playwright.dev/python/docs/locators)

- stworzone dla niskiego punktu wejścia
- wyposażone w mechanizm filtracji wyników
- można też dodawać własne

###  3.2. <a name='LocatoryCSS'></a> Locatory CSS

Rowziązanie uniwersalne we wszystkich bibliotekach
- selectory CSS
- selectory XPAPH (meh...)

**Playwright locator -> css**


```python
 page.locator('<selctor>')
```

Podstawowe selektory:
```css
/* Podowolnym atrybucie */
[attribute="value"]

/* po tagu */
html_tag

/* Pseudo klasy */
:nth-child(n)
:focus
```

Operacje logiczne
```css
/* Operacja AND: selector_1selector_2 */
.class1.class2

/* jest potomkiem: przodek potomek */
#form_id input[type="password"]

/* jest dzieckiem: rodzic dziecko */
#div.modal > h2

```
Skrótowce
```css

/* po atrybucie id */
#id_elementu

/* po atrybucie class */
.class_elementu
```



##  4. <a name='Akcje'></a>Akcje

[Interakcja z elementami](https://playwright.dev/python/docs/input)


Pobieranie informacji ze strony:
* [tekst](https://playwright.dev/python/docs/api/class-locator#locator-text-content)
* [value](https://playwright.dev/python/docs/api/class-locator#locator-input-value)
* [atrybut/property](https://playwright.dev/python/docs/api/class-locator#locator-get-attribute)



##  5. <a name='Asercje'></a>Asercje

Wbudowane asercje w Playwright
[Dokumentacja](https://playwright.dev/python/docs/test-assertions)

- Zawierają podstawowe asercje dotyczące zazwyczaj jednego elementu
- Nie są zbyt efektywne gdy chcemy sprawdzić więcej elementów na stronie (strategia fast fail)


##  6. <a name='Kontekstprzegldarki'></a>Kontekst przeglądarki

Każdy konteks to jedna **wyizolowana, czysta** przeglądarka


###  6.1. <a name='Tworzeniekontekstw'></a>Tworzenie kontekstów
Jeżeli potrzebujesz jednej przeglądarki w teście - użyj fixture page

```python
from playwright.sync_api import Page

# tworzy domyślny, pojedyńczy kontekst i wstrzykuje gotowy do testu
def test_singleBrowser(page: Page):
     page.goto("https://google.com")
```

Jeżeli w teście potrzebujesz dwóch i więcej, konteksty musisz tworzyć sam

```python
from playwright.sync_api import Playwright

def test_multi_browser(playwright: Playwright):
      chromium = playwright.chromium
      browser = chromium.launch(headless=False)

      google = browser.new_context().new_page()
      gazeta = browser.new_context().new_page()

      google.goto("https://www.google.com")
      gazeta.goto("https://gazeta.pl")
```

Możesz też w kontekście pojedyńczej przeglądarki otwierać wiele zakładek

```python
from playwright.sync_api import Playwright

def test_multi_page(playwright: Playwright):
      chromium = playwright.chromium
      browser = chromium.launch(headless=False)

      ctx = browser.new_context()

      google = ctx.new_page()
      gazeta = ctx.new_page()

      google.goto("https://www.google.com")
      gazeta.goto("https://gazeta.pl")
```

###  6.2. <a name='Sesje'></a>Sesje

Można również zapisywać sesje (np. po zalogowaniu) i jej reużyć w kolejnym teście.

**Zapis sesji do pliku**
```python
     context.storage_state(path='saved_session.json')
```


**Zasilenie kontekstu zapisanymi danymi sesji**
```python
     page = context.browser
          .new_context(storage_state='saved_session.json').new_page()
```




## Zasady dobrych testów automatycznych

1. Czas wykonania wszystkich testów jest ważny (im mniejszy tym lepszy)
1. Jeden test jedna fukcjonalność aby uzystkać szczegółowy raport i przetestować całą aplikację (koniczene przy strategii fast fail w asercjach)
