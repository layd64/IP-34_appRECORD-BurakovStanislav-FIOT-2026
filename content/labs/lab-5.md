## Тема, Мета, Місце розташування

**Тема лабораторної:** Безпека та продуктивність серверних додатків Безпека Node.js-додатків Оптимізація запитів і кешування Тестування API»

**Мета лабораторної:** Ознайомитися з методами підвищення продуктивності та безпеки серверних застосунків, а також розгортанням за допомогою Docker.

---

**Назва проєкту:** BookStore Pro

**Тема проєкту:** Веб-застосунок для онлайн-магазину книг

**Мета проєкту:** Прискорення та спрощення процесу купівлі книг, забезпечення зручного доступу до широкого асортименту літератури, створення інтуїтивного інтерфейсу для користувачів, підтримка ефективного управління каталогом і замовленнями для адміністраторів.

---

*Посилання на репозиторій власного WEB-застосунку:* [https://github.com/layd64/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026](https://github.com/layd64/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026)

*Посилання на живу сторінку власного WEB-застосунку:* [layd64.github.io/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026](https://layd64.github.io/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026/)

*Посилання на репозиторій звітного HTML-документа:* [github.com/layd64/IP-34_appRECORD-BurakovStanislav-FIOT-2026](https://github.com/layd64/IP-34_appRECORD-BurakovStanislav-FIOT-2026)

*Посилання на живу сторінку звітного HTML-документа:* [layd64.github.io/IP-34_appRECORD-BurakovStanislav-FIOT-2026](https://layd64.github.io/IP-34_appRECORD-BurakovStanislav-FIOT-2026/)

---

## Завдання до лабораторної роботи

Необхідно виконати наступні практичні завдання:

1. **Реалізувати захист API:** Захистити застосунок за допомогою заголовків безпеки, підключивши Helmet.
2. **Swagger документація:** Згенерувати та налаштувати Swagger UI для інтерактивної документації API.
3. **Реалізувати кешування відповідей:** Підключити Redis-кешування для збереження результатів запитів і зменшення навантаження на базу даних.
4. **Оптимізувати один із маршрутів API:** Додати пагінацію, фільтрацію та пошук, а також використати кешування.
5. **Провести тестування API:** Написати тести за допомогою Jest та Supertest для перевірки функціоналу та заголовків безпеки.
6. **Docker-контейнеризація:** Налаштувати Dockerfile та docker-compose.yml для підняття застосунку разом із залежними сервісами (MySQL, Redis).
7. **Проаналізувати продуктивність до та після оптимізації:** Провести навантажувальне тестування за допомогою Artillery та власного бенчмарку.

---

## Хід Виконання Роботи

### 1. Реалізація захисту API (Helmet)

Для підвищення безпеки застосунку було використано middleware helmet. Це допомагає захистити сервер від ряду відомих веб-вразливостей шляхом встановлення відповідних HTTP-заголовків (наприклад, Content-Security-Policy, X-Frame-Options, X-DNS-Prefetch-Control).

```javascript
// index.js
const helmet = require('helmet');

const app = express();

app.use(cors());
// підключення захисних заголовків
app.use(helmet());
```

_Скріншот: Тестування безпеки заголовків Helmet (відповідь сервера із заголовками безпеки)_

![Helmet Захист. Деякі заголовки з Helmet.](/assets/labs/lab-5/helmet.png)

### 2. Swagger документація

Щоб забезпечити зручне тестування та інтеграцію API, було підключено `swagger-jsdoc` та `swagger-ui-express`. Документація генерується автоматично з коментарів у файлах маршрутів та доступна за адресою `/api-docs`.

```javascript
// swagger.js
const swaggerJSDoc = require('swagger-jsdoc');

const swaggerDefinition = {
  openapi: '3.0.0',
  info: {
    title: 'Bookstore Backend API',
    version: '1.0.0',
    description: 'API for Bookstore application',
  },
  // ...
};

const options = {
  swaggerDefinition,
  apis: ['./routes/*.js'],
};

module.exports = swaggerJSDoc(options);

// index.js
const swaggerUi = require('swagger-ui-express');
const swaggerSpec = require('./swagger');
app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```

_Скріншот: Інтерфейс Swagger UI для тестування API_

![Swagger](/assets/labs/lab-5/swagger.png)

### 3. Кешування відповідей (Redis)

Для пришвидшення відповідей сервера та зниження навантаження на базу даних було імплементовано кешування на базі **Redis**. Створено Redis-клієнт, який перехоплює запити на отримання книг і повертає кешовані дані. При зміні або видаленні даних (POST, PUT, DELETE) кеш автоматично інвалідується.

```javascript
// controllers/bookController.js
const { createClient } = require('redis');
const CACHE_KEY = 'books:all';
const CACHE_TTL = 60; // секунд

let redisClient;
(async () => {
    redisClient = createClient({
        url: process.env.REDIS_URL || 'redis://localhost:6379'
    });
    await redisClient.connect();
})();

async function invalidateCache() {
    if (redisClient?.isOpen) {
        await redisClient.del(CACHE_KEY);
    }
}
```

_Скріншот: Успішне підключення Redis та повернення даних з кешу_

![Redis Кеш](/assets/labs/lab-5/redis.png)

### 4. Оптимізація маршруту API

Головний маршрут `GET /api/books` було значно оптимізовано. Додана підтримка пагінації (`limit`, `page`), пошуку (`search`) за назвою чи автором, а також фільтрації за жанром (`genre`). Чистий запит без фільтрів відтепер використовує Redis-кешування.

```javascript
// controllers/bookController.js (фрагмент)
const getBooks = async (req, res, next) => {
    try {
        const { genre, search, page = 1, limit = 20 } = req.query;
        const isDefaultPagination = parseInt(page) === 1 && parseInt(limit) === 20;
        const useCache = !genre && !search && isDefaultPagination;

        if (useCache && redisClient?.isOpen) {
            const cached = await redisClient.get(CACHE_KEY);
            if (cached) return res.json({ source: 'cache', data: JSON.parse(cached) });
        }

        // ... логіка пагінації та пошуку з Sequelize ...
        
        if (useCache && redisClient?.isOpen) {
            await redisClient.setEx(CACHE_KEY, CACHE_TTL, JSON.stringify(result));
        }
        res.json({ source: 'database', data: result });
    } catch (err) {
        next(err);
    }
};
```

_Скріншот: Результат GET-запиту з оптимізованого маршруту з пагінацією_

![Оптимізація](/assets/labs/lab-5/route-optimization.png)

### 5. Тестування API

За допомогою фреймворку `Jest` та бібліотеки `Supertest` написано набір автоматизованих тестів, що покривають ключові маршрути книг, систему аутентифікації та перевіряють наявність заголовків безпеки Helmet.

```javascript
// server.test.js
describe('Bookstore API — Books', () => {
    test('GET /api/books — кешування (поле source)', async () => {
        const first = await request(API_URL).get('/api/books');
        expect(['database', 'cache']).toContain(first.body.source);

        const second = await request(API_URL).get('/api/books');
        expect(second.body.source).toBe('cache');
    });
});

describe('Bookstore API — Security headers (Helmet)', () => {
    test('Заголовки безпеки присутні', async () => {
        const response = await request(API_URL).get('/api/books');
        expect(response.headers['x-dns-prefetch-control']).toBeDefined();
        expect(response.headers['x-frame-options']).toBeDefined();
    });
});
```

_Скріншот: Результати успішного виконання юніт-тестів через Jest_

![Jest Тести](/assets/labs/lab-5/jest-tests.png)

### 6. Docker-контейнеризація

Для зручності розгортання проєкт було обгорнуто в Docker-контейнери. `docker-compose.yml` конфігурує і піднімає базу даних (MySQL), систему кешування (Redis) та безпосередньо сам Node.js застосунок, автоматично налаштовуючи зв'язки між ними.

```yaml
# docker-compose.yml (фрагмент)
services:
  db:
    image: mysql:8.0
    # ...
  redis:
    image: redis:alpine
    ports:
      - "6379:6379"
  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DB_HOST=db
      - REDIS_URL=redis://redis:6379
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
```

_Скріншот: Запуск контейнерів за допомогою Docker Compose_

![Docker](/assets/labs/lab-5/docker.png)

### 7. Аналіз продуктивності

Для оцінки ефективності кешування створено власний скрипт `benchmark.js` та проведено навантажувальне тестування за допомогою `Artillery` (`npx artillery quick --count 50 --num 20 http://localhost:3000/api/books`). Результати показали значне прискорення відповідей (зменшення часу до декількох мілісекунд), оскільки запити більше не завантажують базу даних.

```javascript
// benchmark.js (фрагмент виводу)
console.log('='.repeat(55));
console.log('  Benchmark: GET /api/books (без кешу vs з кешем)');
// ...
const improvement = Math.round((1 - afterStats.avg / beforeResult.duration) * 100);
console.log(`\n  прискорення: ~${improvement}% швидше з кешем`);
```

_Скріншот: Звіт навантажувального тестування Artillery та скрипта benchmark.js_

![Продуктивність](/assets/labs/lab-5/performance.png)

---

## Висновки

Під час виконання п'ятої лабораторної роботи було значно підвищено продуктивність та безпеку розробленого API. Використання пакету Helmet забезпечило базовий захист від поширених веб-вразливостей, а підключення Redis дозволило реалізувати ефективне кешування запитів, що суттєво зменшило час відповіді та навантаження на базу даних. 

Маршрут отримання книг було оптимізовано за допомогою пагінації та фільтрації, що дозволяє працювати з великими обсягами даних. Також була інтегрована Swagger-документація для зручної роботи з API, написані автоматизовані тести з використанням Jest і Supertest, та налаштована Docker-контейнеризація для легкого розгортання проєкту. Аналіз продуктивності підтвердив ефективність впроваджених архітектурних рішень.
