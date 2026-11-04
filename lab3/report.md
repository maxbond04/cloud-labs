# Звіт з лабораторної роботи №3

**Студент:** Бондаревський Максим

## Мета

Отримати практичні навички створення бессерверних обчислень (FaaS) за допомогою Azure Functions, створення REST API шлюзу через Azure API Management (APIM), а також налаштування політик трансформації запитів та керування доступом у хмарному середовищі Microsoft Azure.

Хід виконання роботи

Завдання 1: Створення та тестування Azure Function App (FaaS)

Створено групу ресурсів Calculator-FaaS-RG та бессерверний додаток Azure Function App у регіоні West US 3 на базі середовища виконання Node.js 22 LTS (Serverless Consumption Plan).
<img width="788" height="615" alt="Знімок екрана 2026-11-03 о 23 55 51" src="https://github.com/user-attachments/assets/46fd907b-cda7-4162-ac63-37ccf2d6e0c5" />

<img width="860" height="581" alt="Знімок екрана 2026-11-03 о 23 57 49" src="https://github.com/user-attachments/assets/49cbd52b-d42f-43d9-991a-c9b0eb552c9b" />

<img width="1417" height="724" alt="Знімок екрана 2026-11-03 о 23 59 00" src="https://github.com/user-attachments/assets/e9c70627-b8ad-4ee5-8b82-35eb7963185f" />

Налаштовано бессерверну функцію з HTTP-тригером Calculator.
<img width="802" height="254" alt="Знімок екрана 2026-11-03 о 23 59 28" src="https://github.com/user-attachments/assets/419d493a-f478-4c71-98f7-39c0baca4bbf" />

<img width="1418" height="295" alt="Знімок екрана 2026-11-03 о 23 59 51" src="https://github.com/user-attachments/assets/51656df9-4ad6-4a95-9e99-4ee3a158b085" />

Реалізовано обробник обчислень на Node.js, який приймає JSON-об'єкт із операндами (a, b) та оператором (op), після чого повертає результат математичної операції у форматі JSON.
<img width="1411" height="590" alt="Знімок екрана 2026-11-04 о 00 07 21" src="https://github.com/user-attachments/assets/1d3651ff-7883-44ef-8c3c-2647b9f64df3" />

Успішно проведено локальне тестування функції в Azure Portal (інструмент Test/Run) із методом POST. Отримано статус 200 OK та коректний результат обчислення (2 + 5 = 7).
<img width="1437" height="800" alt="Знімок екрана 2026-11-04 о 00 07 38" src="https://github.com/user-attachments/assets/088c2d61-fa0f-48b8-aaa6-ca7bcb381aff" />

<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 10 29" src="https://github.com/user-attachments/assets/d9492536-663f-4ed3-aa3a-f34e6fa12246" />


Завдання 2: Створення та налаштування Azure API Management (APIM)

Розгорнуто екземпляр шлюзу Azure API Management my-calculator-apim-021 у тарифі Consumption в регіоні West US 3.
<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 18 25" src="https://github.com/user-attachments/assets/af299a5b-1561-42aa-a325-e4e81b644359" />

<img width="1136" height="494" alt="Знімок екрана 2026-11-04 о 00 25 08" src="https://github.com/user-attachments/assets/7fa0fefa-4e27-4aaa-a693-eac074846384" />

Створено новий REST API під назвою LambdaCalc із суфіксом lambdacalc.
<img width="1440" height="811" alt="Знімок екрана 2026-11-04 о 00 27 27" src="https://github.com/user-attachments/assets/5c3aa7a3-9a0b-4b23-aa39-1ef39fad5091" />

Створено HTTP-операцію GET /calculator та прив'язано її до Backend-сервісу створеної бессерверної функції.
<img width="1440" height="810" alt="Знімок екрана 2026-11-04 о 00 30 02" src="https://github.com/user-attachments/assets/34fa98a4-516a-4116-bed5-5f4304290677" />

<img width="1440" height="809" alt="Знімок екрана 2026-11-04 о 00 37 07" src="https://github.com/user-attachments/assets/7a4219e2-c16a-4718-a11e-56050673b689" />

У розділі Inbound processing за допомогою XML-політики налаштовано автоматичну трансформацію GET-запитів:

Перетворення URL-параметрів operand1, operand2 та operator у JSON-тіло запиту (POST).

Додавання заголовка Content-Type: application/json.

Динамічна підстановка секретного ключа авторизації функції (code=...) для захищеного виклику бекенду.

Вимкнено обов'язковість підписки (Subscription required) для забезпечення відкритого виклику API через браузер.

<img width="1440" height="809" alt="Знімок екрана 2026-11-04 о 00 41 03" src="https://github.com/user-attachments/assets/b739b43f-94d9-4802-aa9f-bf8da6c22187" />

<img width="1440" height="809" alt="Знімок екрана 2026-11-04 о 00 44 40" src="https://github.com/user-attachments/assets/3fa43868-ff61-4ca5-a913-c432585321cd" />

<img width="1440" height="810" alt="Знімок екрана 2026-11-04 о 00 44 48" src="https://github.com/user-attachments/assets/f09dcd8e-3fa7-48c6-a83e-d1eb93386c44" />

<img width="943" height="640" alt="Знімок екрана 2026-11-04 о 00 47 10" src="https://github.com/user-attachments/assets/b1c845ed-e234-4764-a9c2-3d18c9a750a4" />



Завдання 3: Тестування фінального API через браузер

Сформовано підсумковий URL виклику API через шлюз APIM:
https://my-calculator-apim-021.azure-api.net/lambdacalc/calculator?operand1=4&operand2=6&operator=add

Виконано запит безпосередньо з адресного рядка веб-браузера. Отримано успішну JSON-відповідь із виконаним додаванням (c: 10).

<img width="1435" height="146" alt="Знімок екрана 2026-11-04 о 00 50 52" src="https://github.com/user-attachments/assets/232670d4-f02e-49b5-a90c-4722768061ee" />


Висновок

Під час виконання лабораторної роботи було успішно побудовано безсерверну архітектуру (Serverless) в хмарі Microsoft Azure. Було опановано принципи створення FaaS-компонентів (Azure Functions) та проксіювання запитів через шлюз API Management. Налаштована політика трансформації дозволила безпечно приховати внутрішню структуру функції та її секретні ключі, перетворивши вхідний HTTP GET-запит із URL-параметрами у валідний POST JSON-запит до бекенду. Після завершення тестування всі хмарні ресурси та групи ресурсів було видалено для оптимізації витрат.
