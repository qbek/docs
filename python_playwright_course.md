# Python Playwright 

<!-- vscode-markdown-toc -->
* 1. [Setup środowiska](#Setuprodowiska)
	* 1.1. [Instalacja narzędzi](#Instalacjanarzdzi)
	* 1.2. [Konfiguracja](#Konfiguracja)
	* 1.3. [Hello world](#Helloworld)
* 2. [pyTest fixtures](#pyTestfixtures)
* 3. [Selectory](#Selectory)
	* 3.1. [Selectory wbudowane](#Selectorywbudowane)
	* 3.2. [ Locatory](#Locatory)
* 4. [Akcje](#Akcje)
* 5. [Asercje](#Asercje)
* 6. [Kontekst przeglądarki](#Kontekstprzegldarki)
* 7. [Sesje](#Sesje)

<!-- vscode-markdown-toc-config
	numbering=true
	autoSave=true
	/vscode-markdown-toc-config -->
<!-- /vscode-markdown-toc -->


##  1. <a name='Setuprodowiska'></a>Setup środowiska

###  1.1. <a name='Instalacjanarzdzi'></a>Instalacja narzędzi
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

###  1.2. <a name='Konfiguracja'></a>Konfiguracja

Plik ```pytest.ini```

```ini
[pytest]
python_files = *Test.py
addopts = -s --headed --browser firefox
```

###  1.3. <a name='Helloworld'></a>Hello world

```python
import pytest
from playwright.sync_api import Page

def test_helloWorld(page: Page)
     page.goto('https://google.com)

```


##  2. <a name='pyTestfixtures'></a>pyTest fixtures

Mechanizm *Dependency Injection*

### Hooks Before... After...

### Wstrzykiwanie zależności

##  3. <a name='Selectory'></a>Selectory

###  3.1. <a name='Selectorywbudowane'></a>Selectory wbudowane
Wbudowane locatory w Playwright:
[Dokuemntacja](https://playwright.dev/python/docs/locators)

- stworzone dla niskiego punktu wejścia
- wyposażone w mechanizm filtracji wyników
- można też dodawać własne

###  3.2. <a name='Locatory'></a> Locatory

Rowziązanie uniwersalne we wszystkich bibliotekach
- selectory CSS
- selectory XPAPH (meh...)

**Playwright locator -> css**

[get_by_role](https://playwright.dev/python/docs/locators#locate-by-role)
```python
     #page.get_by_role("heading", name="Sign up")
     page.locator('h3')

     #page.get_by_role("checkbox", name="Subscribe")
     page.locator('input')
     page.locator('[type="checkbox"]')

     #page.get_by_role("button", name=re.compile("submit", re.IGNORECASE))
     page.locator('button')
```

[get_by_label](https://playwright.dev/python/docs/locators#locate-by-label)
```python
     #page.get_by_label("Password")
     page.locator('input')
     page.locator('[type="checkbox"]')
```

##  4. <a name='Akcje'></a>Akcje





##  5. <a name='Asercje'></a>Asercje

Wbudowane asercje w Playwright
[Dokumentacja](https://playwright.dev/python/docs/test-assertions)

- Zawierają podstawowe asercje dotyczące zazwyczaj jednego elementu
- Nie są zbyt efektywne gdy chcemy sprawdzić więcej elementów na stronie (strategia fast fail)

Przykłady:
- test na wiele todo (gdzie możemy )




##  6. <a name='Kontekstprzegldarki'></a>Kontekst przeglądarki

Każdy konteks to jedna **wyizolowana, czysta** przeglądarka

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


##  7. <a name='Sesje'></a>Sesje



# Skrypt


```python
def test_many(userSteps: UserSteps):
     userSteps.create_few_todos()
     userSteps.check_all_todos()
     time.sleep(5)
```