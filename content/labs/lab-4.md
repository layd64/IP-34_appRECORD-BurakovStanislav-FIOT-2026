## Тема, Мета, Місце розташування

**Тема лабораторної:** Розширені можливості Node.js-додатків: логування, завантаження файлів, моніторинг продуктивності.

**Мета лабораторної:** Ознайомитися з розширеними можливостями серверних застосунків на базі Node.js.

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

Згідно з інструкцією, необхідно виконати наступні практичні завдання:

1. **Ініціалізація проєкту:** Запустити серверний застосунок на базі Node.js із використанням Express та переконатися, що сервер відповідає на GET-запит /.
2. **Логування HTTP-запитів:** Підключити Morgan та налаштувати логування всіх HTTP-запитів у консоль.
3. **Файлове логування подій:** Інтегрувати Winston і реалізувати запис логів у файл app.log з підтримкою рівнів info та error.
4. **Обробка помилок:** Реалізувати middleware для обробки помилок, логування їх через Winston та повернення користувачу JSON-відповіді.
5. **Завантаження одного файлу:** Реалізувати endpoint /upload, який дозволяє завантажити один файл за допомогою Multer.
6. **Завантаження кількох файлів:** Розширити функціонал для підтримки завантаження кількох файлів і збереження їх у папку uploads.
7. **Валідація файлів:** Додайте перевірки на дозволені типи файлів (jpg, png, pdf) та обмеження їх розміру.
8. **Моніторинг стану сервера:** Створити endpoint /status, який повертає uptime сервера та використання пам’яті.
9. **Вимірювання часу відповіді:** Реалізувати middleware для вимірювання часу обробки кожного запиту та запису результату у лог.
10. **Інтеграція менеджера процесів:** Запустити застосунок через PM2, перевірити автоматичний рестарт та логування.

---

## Хід Виконання Роботи

### 1. Ініціалізація проєкту

Проєкт побудовано на базі фреймворку Express. Створено точку входу index.js, де ініціалізується сервер, підключаються всі необхідні middleware та встановлюється прослуховування заданого порту (3000 за замовчуванням). Також налаштовано кореневий GET-маршрут /, що підтверджує працездатність серверного застосунку та повертає тестову сторінку.

```javascript
// index.js
require('dotenv').config();
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.json());

// Кореневий запит для перевірки роботи
const { renderTestPage } = require('./controllers/studentController');
app.get('/', renderTestPage);

// ... (інші підключення маршрутів та middleware)

app.listen(PORT, () => {
    console.log(`Сервер запущено на http://localhost:${PORT}`);
});
```

_Скріншот: Успішний запуск сервера_

![Ініціалізація сервера](/assets/labs/lab-4/init-server.png)

### 2. Логування HTTP-запитів

Для забезпечення прозорості вхідного трафіку було підключено бібліотеку Morgan. Вона налаштована на використання формату dev, що ідеально підходить для розробки. В результаті в консоль автоматично виводяться всі HTTP-запити із зазначенням методу, URL, статусу відповіді та часу виконання запиту.

```javascript
// index.js
const morgan = require('morgan');

// логування HTTP-запитів
app.use(morgan('dev'));
```

_Скріншот: Логування запитів у консоль за допомогою Morgan_

![Morgan Логування](/assets/labs/lab-4/morgan-log.png)

### 3. Файлове логування подій

Система логування була значно вдосконалена завдяки інтеграції бібліотеки Winston. Замість звичайного виводу у консоль було створено логер, що зберігає події з рівнями info та error у локальний файл app.log. Усі логи автоматично отримують timestamp та форматуються у форматі JSON, що робить їх зручними для подальшого аналізу.

```javascript
// utils/logger.js
const winston = require('winston');
const path = require('path');

const logDir = path.join(__dirname, '../logs');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json()
  ),
  transports: [
    new winston.transports.File({ filename: path.join(logDir, 'app.log') })
  ]
});

module.exports = logger;
```

_Скріншот: Вміст файлу app.log з логами подій_

![Winston Логування](/assets/labs/lab-4/winston-log.png)

### 4. Обробка помилок

Для централізованої обробки помилок всього додатку створено спеціальний middleware errorLogger. У разі виникнення винятку у будь-якому маршруті, цей middleware перехоплює помилку, детально її логує через створений раніше Winston logger і повертає клієнту безпечну та стандартизовану JSON-відповідь.

```javascript
// middlewares/errorLogger.js
const logger = require('../utils/logger');

function errorLogger(err, req, res, next) {
    logger.error({
        message: err.message || 'Внутрішня помилка сервера',
        stack: err.stack,
        method: req.method,
        url: req.url,
        status: err.status || 500
    });

    res.status(err.status || 500).json({ error: err.message || 'Внутрішня помилка сервера', code: err.status || 500 });
}

module.exports = errorLogger;
```

_Скріншот: Повернення JSON-помилки клієнту та запис у лог_

![Обробка помилок](/assets/labs/lab-4/error-handler.png)

### 5. Завантаження одного файлу

Обробка мультимедійних даних реалізована за допомогою Multer. В маршрутизаторі uploadRoutes.js визначено endpoint /upload. Використовується метод upload.single('file')`, який перехоплює один файл із форми та зберігає його на сервері, повертаючи користувачу об'єкт із деталями збереженого файлу.

```javascript
// routes/uploadRoutes.js
const express = require('express');
const router = express.Router();
const upload = require('../middlewares/fileUpload');

// Task 5: Single file upload
router.post('/upload', upload.single('file'), (req, res) => {
    if (!req.file) {
        return res.status(400).json({ error: 'Файл не завантажено або недопустимий тип/розмір' });
    }
    res.json({ message: 'Файл успішно завантажено', file: req.file });
});
```

_Скріншот: Тестування завантаження одного файлу в Postman_

![Один файл](/assets/labs/lab-4/upload-single.png)

### 6. Завантаження кількох файлів

Функціонал було розширено для підтримки масового завантаження файлів (до 10 штук одночасно) через метод upload.array('files', 10). Ендпоінт /upload-multiple забезпечує коректне оброблення масиву об'єктів файлів і поміщає їх у цільову директорію uploads/.

```javascript
// routes/uploadRoutes.js
// Task 6: Multiple files upload
router.post('/upload-multiple', upload.array('files', 10), (req, res) => {
    if (!req.files || req.files.length === 0) {
        return res.status(400).json({ error: 'Файли не завантажено або недопустимий тип/розмір' });
    }
    res.json({ message: 'Файли успішно завантажено', files: req.files });
});
```

_Скріншот: Результат завантаження кількох файлів одночасно_

![Кілька файлів](/assets/labs/lab-4/upload-multiple.png)

### 7. Валідація файлів

Щоб запобігти завантаженню шкідливих або надмірно великих файлів, конфігурація Multer була посилена системою фільтрів. У middlewares/fileUpload.js додано перевірку MIME-типу, що обмежує дозволені формати тільки як `jpg`, `png` та `pdf`. Крім того, додано суворий ліміт розміру файлу у 5 Мегабайт. При порушенні правил Multer відхиляє завантаження.

```javascript
// middlewares/fileUpload.js
const fileFilter = (req, file, cb) => {
    const allowedTypes = ['image/jpeg', 'image/png', 'application/pdf'];
    if (allowedTypes.includes(file.mimetype)) {
        cb(null, true);
    } else {
        cb(new Error('Недопустимий тип файлу. Дозволено: jpg, png, pdf'), false);
    }
};

const upload = multer({ 
    storage: storage,
    limits: {
        fileSize: 5 * 1024 * 1024 // 5 MB limit
    },
    fileFilter: fileFilter
});
```

_Скріншот: Помилка валідації при спробі завантаження файлу недопустимого формату_

![Валідація файлів](/assets/labs/lab-4/file-validation.png)

### 8. Моніторинг стану сервера

Для постійного контролю за працездатністю застосунку створено спеціальний маршрут /status. Він взаємодіє з глобальним об'єктом process у Node.js і повертає час безперервної роботи (uptime) та докладну статистику споживання оперативної пам'яті (RSS, heapTotal, heapUsed), а також актуальну мітку часу.

```javascript
// routes/statusRoutes.js
const express = require('express');
const router = express.Router();

router.get('/status', (req, res) => {
    res.json({
        uptime: process.uptime(),
        memoryUsage: process.memoryUsage(),
        timestamp: new Date().toISOString()
    });
});

module.exports = router;
```

_Скріншот: Відповідь від endpoint /status із статистикою_

![Статус сервера](/assets/labs/lab-4/server-status.png)

### 9. Вимірювання часу відповіді

Щоб виявляти проблемні місця в оптимізації маршрутів, розроблено проміжний обробник responseTimeLogger. Використовуючи високоточний метод process.hrtime(), middleware розпочинає відлік у момент надходження запиту, а під час події finish від клієнта - фіксує кінець і вираховує тривалість обробки в мілісекундах. Отриманий результат відразу записується у лог через Winston.

```javascript
// middlewares/responseTimeLogger.js
const logger = require('../utils/logger');

function responseTimeLogger(req, res, next) {
    const start = process.hrtime();

    res.on('finish', () => {
        const diff = process.hrtime(start);
        const timeInMs = (diff[0] * 1e3 + diff[1] * 1e-6).toFixed(3);
        
        logger.info({
            message: 'Request processed',
            method: req.method,
            url: req.url,
            responseTimeMs: timeInMs,
            status: res.statusCode
        });
    });

    next();
}

module.exports = responseTimeLogger;
```

_Скріншот: Логи з виміряним часом відповіді (responseTimeMs)_

![Час відповіді](/assets/labs/lab-4/response-time.png)

### 10. Інтеграція менеджера процесів

Для управління життєвим циклом Node-додатку в production-режимі в проєкт інтегровано PM2. Були створені спеціальні скрипти у package.json, які дозволяють легко запускати сервер (з автоматичним рестартом при змінах за допомогою прапорця --watch), зупиняти його та переглядати агреговані PM2 логи.

```json
// package.json (фрагмент)
  "scripts": {
    "start": "node index.js",
    "start:pm2": "pm2 start index.js --name appWEB-backend --watch",
    "stop:pm2": "pm2 stop appWEB-backend",
    "logs:pm2": "pm2 logs appWEB-backend"
  }
```

_Скріншот: Управління процесом через консоль PM2 та перезапуск сервера при змінах в коді_

![PM2 Статус](/assets/labs/lab-4/pm2-status.png)

---

## Висновки

В ході виконання четвертої лабораторної роботи було значно розширено можливості бекенду. Завдяки імплементації пакетів Morgan та Winston, сервер отримав багаторівневу та надійну систему логування з веденням файлу аудит-журналу. Підключення Multer дозволило безпечно завантажувати поодинокі та множинні файли зі строгими лімітами та валідацією MIME-типів.

Окремим досягненням стала реалізація middleware-рівня моніторингу, зокрема інструменту для трекінгу часу обробки запитів, та спеціального маршруту моніторингу станів пам'яті й uptime-у сервера. Інтеграція сучасного менеджера процесів PM2 забезпечила додатку стійкість до збоїв і безперебійну роботу.
