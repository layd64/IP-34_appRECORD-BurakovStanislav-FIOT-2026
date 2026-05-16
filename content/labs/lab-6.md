## Тема, Мета, Місце розташування

**Тема лабораторної:** Документування API за допомогою Swagger Деплой Node.js-додатку. Підсумковий проєкт: REST API з MySQL

**Мета лабораторної:** Ознайомитися з інструментами автоматичної генерації документації API за допомогою Swagger та отримати практичні навички розгортання Node.js-застосунків на хмарних платформах.

---

**Назва проєкту:** BookStore Pro

**Тема проєкту:** Веб-застосунок для онлайн-магазину книг

**Мета проєкту:** Прискорення та спрощення процесу купівлі книг, забезпечення зручного доступу до широкого асортименту літератури, створення інтуїтивного інтерфейсу для користувачів, підтримка ефективного управління каталогом і замовленнями для адміністраторів.

---

*Посилання на репозиторій власного WEB-застосунку:* [https://github.com/layd64/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026](https://github.com/layd64/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026)

*Посилання на живу сторінку власного WEB-застосунку:* [https://ip-34-appweb-backend-burakov-stanislav.onrender.com](https://ip-34-appweb-backend-burakov-stanislav.onrender.com) *(замініть на ваше посилання на Render)*

*Посилання на репозиторій звітного HTML-документа:* [github.com/layd64/IP-34_appRECORD-BurakovStanislav-FIOT-2026](https://github.com/layd64/IP-34_appRECORD-BurakovStanislav-FIOT-2026)

*Посилання на живу сторінку звітного HTML-документа:* [layd64.github.io/IP-34_appRECORD-BurakovStanislav-FIOT-2026](https://layd64.github.io/IP-34_appRECORD-BurakovStanislav-FIOT-2026/)

---

## Завдання до лабораторної роботи

Необхідно виконати наступні практичні завдання:

1. **Інтегрувати Swagger UI у проєкт:** Підключити та налаштувати бібліотеки для автоматичної генерації документації.
2. **Задокументувати всі endpoint-и:** Описати маршрути, параметри, тіло запитів та можливі відповіді за допомогою JSDoc коментарів.
3. **Виконати тестування API через Swagger UI:** Перевірити працездатність маршрутів безпосередньо через інтерактивний інтерфейс.
4. **Зробити деплой на Render:** Розгорнути Node.js-застосунок на хмарній платформі Render.

---

## Хід Виконання Роботи

### 1. Інтеграція Swagger UI у проєкт

Для створення інтерактивної документації API було використано бібліотеки `swagger-jsdoc` та `swagger-ui-express`. Було створено файл конфігурації `swagger.js`, який містить загальну інформацію про API та налаштування безпеки (JWT авторизація).

```javascript
// server/swagger.js
const swaggerJSDoc = require('swagger-jsdoc');

const swaggerDefinition = {
  openapi: '3.0.0',
  info: {
    title: 'Bookstore Backend API',
    version: '1.0.0',
    description: 'API for Bookstore application',
  },
  servers: [
    {
      url: process.env.APP_URL || 'http://localhost:3000',
      description: 'API server',
    },
  ],
  components: {
    securitySchemes: {
      bearerAuth: {
        type: 'http',
        scheme: 'bearer',
        bearerFormat: 'JWT',
      },
    },
  },
  security: [{ bearerAuth: [] }],
};

const options = {
  swaggerDefinition,
  apis: ['./routes/*.js'], // Шлях до файлів маршрутів
};

module.exports = swaggerJSDoc(options);
```

В головному файлі `index.js` цей модуль було підключено до маршруту `/api-docs`.

```javascript
// server/index.js (фрагмент)
const swaggerUi = require('swagger-ui-express');
const swaggerSpec = require('./swagger');

// ...
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```

### 2. Документування endpoint-ів

Всі існуючі маршрути були покриті JSDoc-коментарями, які зчитуються Swagger. Наприклад, в `bookRoutes.js` були додані описи параметрів, тіла запиту та можливих статусів відповідей.

```javascript
// server/routes/bookRoutes.js (фрагмент)
/**
 * @swagger
 * /api/books:
 *   get:
 *     summary: Отримати список книг (з Redis-кешуванням)
 *     tags: [Books]
 *     parameters:
 *       - in: query
 *         name: genre
 *         schema:
 *           type: string
 *         description: Фільтр за жанром
 *       - in: query
 *         name: page
 *         schema:
 *           type: integer
 *           default: 1
 *     responses:
 *       200:
 *         description: Список книг
 *         content:
 *           application/json:
 *             schema:
 *               type: object
 */
router.get('/', getBooks);
```

### 3. Тестування API через Swagger UI

Після запуску застосунку, документація доступна за адресою `/api-docs`. Інтерфейс Swagger дозволяє переглядати доступні endpoint-и, структуру запитів та здійснювати тестові виклики (Try it out). Для захищених маршрутів передбачено можливість введення JWT-токена (кнопка Authorize).

_Скріншот: Загальний вигляд сторінки Swagger UI_

![Swagger UI](/assets/labs/lab-6/swagger-ui.png)

_Скріншот: Успішне тестування одного з endpoint-ів (наприклад, GET /api/books) через Swagger_

![Swagger Тестування](/assets/labs/lab-6/swagger-test.png)

### 4. Деплой на Render та налаштування баз даних

Для повноцінної роботи проєкту в хмарному середовищі було виконано такі кроки:
1. **База даних MySQL на Aiven**: Створено хмарний екземпляр бази даних MySQL за допомогою сервісу Aiven. Отриманий рядок підключення було використано для конфігурації Sequelize.
2. **Кешування (Redis)**: На платформі Render додано окремий сервіс Redis. Його URL підключення також збережено для використання у проєкті.
3. **Розгортання Web-сервісу**:
   - Створено новий Web Service на Render, підключений до GitHub репозиторію.
   - Вказано директорію (`server`), команду встановлення залежностей (`npm install`) та команду запуску (`npm start`).
   - Налаштовано всі необхідні змінні середовища (URL до бази Aiven, Redis URL, секретні ключі JWT, налаштування пошти, тощо).
   - У файлі `index.js` налаштовано `app.set('trust proxy', 1);` для коректної роботи за проксі-серверами хмарної платформи.

_Скріншот: Успішний деплой на Render_

![Render Деплой](/assets/labs/lab-6/render-deploy.png)

_Скріншот: Працюючий застосунок на задеплоєному сервері_

![Render Production](/assets/labs/lab-6/swagger-prod.png)

---

## Висновки

В ході виконання шостої лабораторної роботи було успішно інтегровано інструменти автоматичної генерації документації Swagger у Node.js застосунок. Задокументовано всі ключові API маршрути, що значно полегшує розуміння роботи бекенду для фронтенд-розробників та тестувальників. Також було проведено тестування запитів безпосередньо через Swagger UI.

Окрім цього, серверний застосунок був успішно розгорнутий на хмарній платформі Render. Це дозволило зробити API доступним в Інтернеті та забезпечило безперервну інтеграцію (CI/CD) при подальших змінах у репозиторії.

**Підсумки проєкту:**
Розробку застосунку "BookStore Pro" було повністю завершено. Було успішно досягнуто всіх початкових функціональних вимог: створено надійний бекенд на Node.js та Express, підключено базу даних з ORM Sequelize, реалізовано кешування за допомогою Redis, забезпечено захист API через Helmet, налаштовано авторизацію, каталог з книгами, адміністрування та багато іншого. Цей проєкт є повноцінною і завершеною системою онлайн-магазину книг, готовою до подальшої підтримки та повноцінного використання.

_Скріншот: Авторизація_

![Авторизація](/assets/labs/lab-6/login.png)

_Скріншот: Каталог книг_

![Каталог книг](/assets/labs/lab-6/catalog.png)

_Скріншот: Функціонал кошика та оформлення замовлення_

![Кошик та замовлення](/assets/labs/lab-6/order.png)

_Скріншот: Панель адміністратора_

![Панель адміністратора](/assets/labs/lab-6/admin.png)
