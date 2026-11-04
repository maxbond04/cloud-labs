# Звіт з лабораторної роботи №3

**Студент:** Бондаревський Максим

## Мета

Отримати практичні навички створення бессерверних обчислень (FaaS) за допомогою Azure Functions, створення REST API шлюзу через Azure API Management (APIM), а також налаштування політик трансформації запитів та керування доступом у хмарному середовищі Microsoft Azure.

Хід виконання роботи

Завдання 1: Створення та тестування Azure Function App (FaaS)

Створено групу ресурсів Calculator-FaaS-RG та бессерверний додаток Azure Function App у регіоні West US 3 на базі середовища виконання Node.js 22 LTS (Serverless Consumption Plan).
<img width="860" height="581" alt="Знімок екрана 2026-11-03 о 23 57 49" src="https://github.com/user-attachments/assets/49cbd52b-d42f-43d9-991a-c9b0eb552c9b" />
<img width="788" height="615" alt="Знімок екрана 2026-11-03 о 23 55 51" src="https://github.com/user-attachments/assets/46fd907b-cda7-4162-ac63-37ccf2d6e0c5" />
<img width="1417" height="724" alt="Знімок екрана 2026-11-03 о 23 59 00" src="https://github.com/user-attachments/assets/e9c70627-b8ad-4ee5-8b82-35eb7963185f" />

Налаштовано бессерверну функцію з HTTP-тригером Calculator.
<img width="1418" height="295" alt="Знімок екрана 2026-11-03 о 23 59 51" src="https://github.com/user-attachments/assets/51656df9-4ad6-4a95-9e99-4ee3a158b085" />
<img width="802" height="254" alt="Знімок екрана 2026-11-03 о 23 59 28" src="https://github.com/user-attachments/assets/419d493a-f478-4c71-98f7-39c0baca4bbf" />

Реалізовано обробник обчислень на Node.js, який приймає JSON-об'єкт із операндами (a, b) та оператором (op), після чого повертає результат математичної операції у форматі JSON.
<img width="1411" height="590" alt="Знімок екрана 2026-11-04 о 00 07 21" src="https://github.com/user-attachments/assets/1d3651ff-7883-44ef-8c3c-2647b9f64df3" />

Успішно проведено локальне тестування функції в Azure Portal (інструмент Test/Run) із методом POST. Отримано статус 200 OK та коректний результат обчислення (2 + 5 = 7).
<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 10 29" src="https://github.com/user-attachments/assets/d9492536-663f-4ed3-aa3a-f34e6fa12246" />
<img width="1437" height="800" alt="Знімок екрана 2026-11-04 о 00 07 38" src="https://github.com/user-attachments/assets/088c2d61-fa0f-48b8-aaa6-ca7bcb381aff" />


Завдання 2: Створення та налаштування Azure API Management (APIM)

Розгорнуто екземпляр шлюзу Azure API Management my-calculator-apim-021 у тарифі Consumption в регіоні West US 3.
<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 27 27" src="https://github.com/user-attachments/assets/54ff49b6-2a0a-485c-acaa-8cd74f25df6a" />
<img width="1136" height="494" alt="Знімок екрана 2026-11-04 о 00 25 08" src="https://github.com/user-attachments/assets/1419f98e-530c-408b-8d20-371003e5308b" />

Створено новий REST API під назвою LambdaCalc із суфіксом lambdacalc.
<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 27 27" src="https://github.com/user-attachments/assets/5c3aa7a3-9a0b-4b23-aa39-1ef39fad5091" />

Створено HTTP-операцію GET /calculator та прив'язано її до Backend-сервісу створеної бессерверної функції.
<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 27 27" src="https://github.com/user-attachments/assets/d4eae746-efc4-4e3b-8595-a97f35f12424" />

У розділі Inbound processing за допомогою XML-політики налаштовано автоматичну трансформацію GET-запитів:

Перетворення URL-параметрів operand1, operand2 та operator у JSON-тіло запиту (POST).

Додавання заголовка Content-Type: application/json.

Динамічна підстановка секретного ключа авторизації функції (code=...) для захищеного виклику бекенду.

Вимкнено обов'язковість підписки (Subscription required) для забезпечення відкритого виклику API через браузер.

Скриншот 2: Конфігурація Inbound Policy в Azure API Management

![APIM Inbound Policy](images/step2_apim_policy.png)

Завдання 3: Тестування фінального API через браузер

Сформовано підсумковий URL виклику API через шлюз APIM:
https://my-calculator-apim-021.azure-api.net/lambdacalc/calculator?operand1=4&operand2=6&operator=add

Виконано запит безпосередньо з адресного рядка веб-браузера. Отримано успішну JSON-відповідь із виконаним додаванням (c: 10).

Скриншот 3: Результат виконання запиту до API Management у браузері

![Browser API Test](images/step3_browser_test.png)

Висновок

Під час виконання лабораторної роботи було успішно побудовано бессерверну архітектуру (Serverless) в хмарі Microsoft Azure. Було опановано принципи створення FaaS-компонентів (Azure Functions) та проксіювання запитів через шлюз API Management. Налаштована політика трансформації дозволила безпечно приховати внутрішню структуру функції та її секретні ключі, перетворивши вхідний HTTP GET-запит із URL-параметрами у валідний POST JSON-запит до бекенду. Після завершення тестування всі хмарні ресурси та групи ресурсів було видалено для оптимізації витрат.
