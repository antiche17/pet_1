Пет‑проект: UI‑автотесты на Python (Selenium / Playwright)
Ниже — структура, стек, тесты, CI, отчёты и всё, что нужно, чтобы проект выглядел как у настоящего автоматизатора.

🧰 1. Технологический стек (Python)
Основной стек:
Python 3.10+

pytest — тестовый фреймворк

Playwright

pytest-playwright

Allure — отчёты

Python-dotenv — конфиги

GitHub Actions — CI

📁 2. Структура проекта
Код
ui-tests/
│
├── tests/
│   ├── test_login.py
│   ├── test_cart.py
│   ├── test_checkout.py
│   └── test_catalog.py
│
├── pages/
│   ├── base_page.py
│   ├── login_page.py
│   ├── catalog_page.py
│   ├── product_page.py
│   ├── cart_page.py
│   └── checkout_page.py
│
├── data/
│   ├── users.json
│   └── test_data.json
│
├── utils/
│   ├── config.py
│   ├── helpers.py
│   └── logger.py
│
├── requirements.txt
├── pytest.ini
├── .gitignore
└── README.md
Эта структура выглядит профессионально и показывает, что ты понимаешь архитектуру автотестов.

🧪 3. Что тестируем (функционал)
Берём простой демо‑магазин, например:
https://www.saucedemo.com/ — идеальный сайт для UI‑тестов.

Тесты:
🔐 Авторизация
успешный логин

неверный пароль

пустые поля

заблокированный пользователь

🛒 Каталог
отображение списка товаров

сортировка

переход на карточку товара

📦 Корзина
добавление товара

удаление товара

изменение количества

проверка итоговой суммы

🧾 Оформление заказа
успешное оформление

ошибки валидации

Всего: 10–15 тестов — идеальный объём.

🧩 4. Page Object Model (POM)
Пример base_page.py:

python
class BasePage:
    def __init__(self, page):
        self.page = page

    def click(self, locator):
        self.page.locator(locator).click()

    def fill(self, locator, text):
        self.page.locator(locator).fill(text)

    def get_text(self, locator):
        return self.page.locator(locator).inner_text()
Пример login_page.py:

python
from pages.base_page import BasePage

class LoginPage(BasePage):
    username = "#user-name"
    password = "#password"
    login_btn = "#login-button"
    error_msg = "h3[data-test='error']"

    def login(self, user, pwd):
        self.fill(self.username, user)
        self.fill(self.password, pwd)
        self.click(self.login_btn)

    def get_error(self):
        return self.get_text(self.error_msg)
🧪 5. Пример теста (pytest + Playwright)
python
def test_success_login(page):
    login_page = LoginPage(page)
    page.goto("https://www.saucedemo.com/")
    login_page.login("standard_user", "secret_sauce")
    assert page.url == "https://www.saucedemo.com/inventory.html"
Минимум кода — максимум пользы.

📊 6. Allure‑отчёты
Добавляешь в pytest.ini:

Код
[pytest]
addopts = --alluredir=allure-results
Запуск:

Код
pytest --alluredir=allure-results
allure serve allure-results
На собеседовании это звучит очень солидно.

🔄 7. CI (GitHub Actions)
Минимальный пайплайн:
Код
name: UI Tests
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Install Playwright browsers
        run: playwright install
      - name: Run tests
        run: pytest --alluredir=allure-results
      - name: Upload Allure results
        uses: actions/upload-artifact@v3
        with:
          name: allure-results
          path: allure-results
Теперь твой проект выглядит как у настоящего QA Automation Engineer.

📘 8. README.md (очень важно!)
В README ты описываешь:
цель проекта
стек
как запускать
примеры тестов
CI
отчёты
Это то, что интервьюеры читают в первую очередь.

🎤 9. Как рассказывать о проекте на собеседовании
«Я сделал пет‑проект на Python с UI‑автотестами для демо‑интернет‑магазина.
Использовал Playwright + pytest, реализовал Page Object Model, вынес тестовые данные, добавил Allure‑отчёты и настроил CI в GitHub Actions.
Тесты стабильные, работают в headless‑режиме, отчёты собираются автоматически.
Проект показывает мои навыки автоматизации и понимание процессов CI/CD».
