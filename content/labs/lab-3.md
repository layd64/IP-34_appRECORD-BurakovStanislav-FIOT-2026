## Тема, Мета, Місце розташування

**Тема лабораторної:** АВТОРИЗАЦІЯ ТА АУТЕНТИФІКАЦІЯ У NODE.JS ДОДАТКАХ. РОБОТА З JWT, OAUTH ТА БЕЗПЕКОЮ.

**Мета лабораторної:** Навчитися реалізовувати безпечну реєстрацію та авторизацію користувачів, використовувати JWT для захисту маршрутів, інтегрувати OAuth-провайдерів та застосовувати кращі практики безпеки веб-додатків.

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

## 1 Завдання до лабораторної роботи

Згідно з інструкцією, необхідно виконати наступні завдання:

1. Встановити необхідні бібліотеки
2. Реалізувати реєстрацію та авторизацію користувача
3. Додати валідацію даних, обробку помилок
4. Реалізувати захищений маршрут
5. Протестувати API через Postman або curl
6. Проаналізувати отримані результати
7. Додати підтвердження пароля при реєстрації
8. Додати роль користувача (admin/user)
9. Реалізувати logout
10. Додати оновлення профілю
11. Зберігати користувачів у базі
12. Реалізувати refresh token
13. Додати логування помилок
14. Обмежити кількість спроб входу
15. Додати middleware для перевірки токена
16. Реалізувати зміну пароля
17. Реалізувати видалення користувача
18. Реалізувати відновлення пароля
19. Додати підтвердження email
20. Реалізувати OAuth (Google login)

---

## 2 Хід Виконання Роботи

### 2.1 Встановити необхідні бібліотеки (Завдання 1)

Були встановлені наступні пакети, визначені у `package.json`:
* `bcrypt` для хешування паролів.
* `jsonwebtoken` для генерації та перевірки токенів доступу.
* `express-validator` для валідації вхідних даних при реєстрації та вході.
* `nodemailer` для відправки листів з підтвердженням email та відновленням пароля.
* `passport` та `passport-google-oauth20` для реалізації входу через Google OAuth.
* `express-rate-limit` для обмеження кількості спроб входу.

```json
  "dependencies": {
    "bcrypt": "^6.0.0",
    "cors": "^2.8.6",
    "dotenv": "^17.4.1",
    "express": "^5.2.1",
    "express-rate-limit": "^8.3.2",
    "express-session": "^1.19.0",
    "express-validator": "^7.3.2",
    "jsonwebtoken": "^9.0.3",
    "mysql2": "^3.19.1",
    "nodemailer": "^8.0.5",
    "passport": "^0.7.0",
    "passport-google-oauth20": "^2.0.0",
    "sequelize": "^6.37.8"
  }
```

### 2.2 Зберігати користувачів у базі (Завдання 11)

Була створена та оновлена модель `User` за допомогою Sequelize для зберігання інформації про користувачів.

```javascript
// models/User.js
const User = sequelize.define('User', {
    id: { type: DataTypes.INTEGER, primaryKey: true, autoIncrement: true },
    username: { type: DataTypes.STRING, allowNull: false },
    email: { type: DataTypes.STRING, allowNull: false, unique: true },
    password: { type: DataTypes.STRING, allowNull: true },
    phone: { type: DataTypes.STRING, allowNull: true },
    address: { type: DataTypes.STRING, allowNull: true },
    role: { type: DataTypes.STRING, defaultValue: 'user' },
    refreshToken: { type: DataTypes.STRING, allowNull: true },
    isEmailConfirmed: { type: DataTypes.BOOLEAN, defaultValue: false },
    confirmationToken: { type: DataTypes.STRING, allowNull: true },
    resetPasswordToken: { type: DataTypes.STRING, allowNull: true },
    resetPasswordExpires: { type: DataTypes.DATE, allowNull: true },
    googleId: { type: DataTypes.STRING, allowNull: true, unique: true }
}, {
    tableName: 'users',
    timestamps: true
});
```

### 2.3 Додати роль користувача (admin/user) (Завдання 8)

Роль користувача реалізована полем `role` у моделі `User` (зі значеннями 'user' та 'admin'). Також було написано відповідний middleware:

```javascript
// middlewares/roleMiddleware.js
const authorizeRole = (...roles) => {
    return (req, res, next) => {
        if (!req.user || !roles.includes(req.user.role)) {
            return res.status(403).json({ error: 'Немає доступу. Потрібна роль: ' + roles.join(', ') });
        }
        next();
    };
};
module.exports = authorizeRole;
```

### 2.4 Реалізувати реєстрацію, авторизацію та logout (Завдання 2 та 9)

Код у контролері відповідає за створення нового облікового запису з хешуванням пароля та за авторизацію через перевірку пароля та повернення токенів доступу:

```javascript
// controllers/authController.js
const register = async (req, res, next) => {
    try {
        const { username, email, password } = req.body;
        const existingUser = await User.findOne({ where: { email } });
        if (existingUser) return res.status(400).json({ error: 'Email вже існує' });

        const hashedPassword = await bcrypt.hash(password, 10);
        const newUser = await User.create({ username, email, password: hashedPassword });
        res.status(201).json({ message: 'Реєстрація успішна!' });
    } catch (err) {
        next(err);
    }
};

const login = async (req, res, next) => {
    try {
        const { email, password } = req.body;
        const user = await User.findOne({ where: { email } });
        if (!user || !(await bcrypt.compare(password, user.password))) {
            return res.status(400).json({ error: 'Невірний email або пароль' });
        }
        const tokens = generateTokens(user);
        user.refreshToken = tokens.refreshToken;
        await user.save();
        res.json(tokens);
    } catch (err) {
        next(err);
    }
};

const logout = async (req, res, next) => {
    try {
        const user = await User.findByPk(req.user.id);
        if (user) {
            user.refreshToken = null;
            await user.save();
        }
        res.json({ message: 'Успішний вихід' });
    } catch (err) {
        next(err);
    }
};
```

_Скріншот: Тестування авторизації та роботи сесії у Postman_

![Реєстрація та авторизація](/assets/labs/lab-3/auth.png)

### 2.5 Додати валідацію даних та підтвердження пароля при реєстрації (Завдання 3 та 7)

Валідація контролюється за допомогою `express-validator` на рівні маршрутів:

```javascript
// middlewares/validators.js
const { body, validationResult } = require('express-validator');

const validateRegister = [
    body('username').notEmpty().withMessage('Ім\\'я обов\\'язкове').isLength({ min: 3 }),
    body('email').isEmail().withMessage('Некоректний формат email'),
    body('password').isLength({ min: 6 }).withMessage('Мінімум 6 символів'),
    body('confirmPassword').custom((value, { req }) => {
        if (value !== req.body.password) throw new Error('Паролі не співпадають');
        return true;
    }),
    (req, res, next) => {
        const errors = validationResult(req);
        if (!errors.isEmpty()) return res.status(400).json({ errors: errors.array() });
        next();
    }
];
```

_Скріншот: Спрацювання валідації паролів у Postman_

![Валідація даних](/assets/labs/lab-3/validation.png)

### 2.6 Додати логування помилок (Завдання 13)

Централізований middleware для обробки та логування будь-яких помилок, що виникають в системі:

```javascript
// middlewares/errorLogger.js
const fs = require('fs');
const path = require('path');

const errorLogger = (err, req, res, next) => {
    const errorMsg = `[${new Date().toISOString()}] ${req.method} ${req.url} - ${err.message}\n${err.stack}\n\n`;
    fs.appendFileSync(path.join(__dirname, '../logs/error.log'), errorMsg);
    
    console.error(err.stack);
    res.status(500).json({ error: 'Внутрішня помилка сервера' });
};

module.exports = errorLogger;
```

_Скріншот: Логування помилки у файл (або вивід у консоль)_

![Логування помилок](/assets/labs/lab-3/error-log.png)

### 2.7 Додати middleware для перевірки токена та захищений маршрут (Завдання 4 та 15)

Захист маршрутів виконується через перевірку наявності та валідності JWT у заголовку `Authorization`:

```javascript
// middlewares/authMiddleware.js
const jwt = require('jsonwebtoken');

const authenticateToken = (req, res, next) => {
    const authHeader = req.headers['authorization'];
    const token = authHeader && authHeader.split(' ')[1];

    if (!token) return res.status(401).json({ error: 'Потрібна авторизація (Token відсутній)' });

    jwt.verify(token, process.env.JWT_SECRET || 'secret_key', (err, user) => {
        if (err) return res.status(403).json({ error: 'Недійсний токен доступу' });
        req.user = user;
        next();
    });
};

module.exports = authenticateToken;
```
Використання у маршрутизаторі:
```javascript
// routes/userRoutes.js
router.get('/profile', authenticateToken, getProfile);
```

_Скріншот: Блокування доступу без токена (Захищений маршрут)_

![Захищений маршрут](/assets/labs/lab-3/protected-route.png)

### 2.8 Реалізувати refresh token (Завдання 12)

Як тільки дія accessToken проходить, клієнт може відновити сесію через refreshToken:

```javascript
// controllers/authController.js
const refreshToken = async (req, res, next) => {
    try {
        const { token } = req.body;
        if (!token) return res.status(401).json({ error: 'Refresh Token обовʼязковий' });

        jwt.verify(token, process.env.JWT_REFRESH_SECRET || 'refresh_secret_key', async (err, decoded) => {
            if (err) return res.status(403).json({ error: 'Недійсний Refresh Token' });

            const user = await User.findByPk(decoded.id);
            if (!user || user.refreshToken !== token) return res.status(403).json({ error: 'Недійсний Refresh Token' });

            const tokens = generateTokens(user);
            user.refreshToken = tokens.refreshToken;
            await user.save();
            res.json(tokens);
        });
    } catch (err) {
        next(err);
    }
};
```

### 2.9 Обмежити кількість спроб входу (Завдання 14)

Використовуємо пакет `express-rate-limit` для обмеження запитів до ендпоінту `/login`:

```javascript
// middlewares/rateLimiter.js
const rateLimit = require('express-rate-limit');

const loginLimiter = rateLimit({
    windowMs: 15 * 60 * 1000, 
    max: 5, 
    message: { error: 'Забагато спроб входу, будь ласка, спробуйте через 15 хвилин' },
    standardHeaders: true,
    legacyHeaders: false,
});

module.exports = { loginLimiter };
```

_Скріншот: Повідомлення про перевищення ліміту спроб входу_

![Rate limiting](/assets/labs/lab-3/rate-limiting.png)

### 2.10 Додати оновлення профілю та зміну пароля (Завдання 10 та 16)

Оновлення особистих даних дозволено тільки аутентифікованим користувачам:

```javascript
// controllers/userController.js
const updateProfile = async (req, res, next) => {
    try {
        const { username, phone, address } = req.body;
        const user = await User.findByPk(req.user.id);
        if (!user) return res.status(404).json({ error: 'Користувача не знайдено' });

        if (username) user.username = username;
        if (phone !== undefined) user.phone = phone;
        if (address !== undefined) user.address = address;
        await user.save();
        res.json({ message: 'Профіль оновлено', user });
    } catch (err) {
        next(err);
    }
};

const changePassword = async (req, res, next) => {
    try {
        const { oldPassword, newPassword } = req.body;
        const user = await User.findByPk(req.user.id);

        if (user.password) {
            const isMatch = await bcrypt.compare(oldPassword, user.password);
            if (!isMatch) return res.status(400).json({ error: 'Старий пароль невірний' });
        }
        user.password = await bcrypt.hash(newPassword, 10);
        await user.save();
        res.json({ message: 'Пароль успішно змінено' });
    } catch (err) {
        next(err);
    }
};
```

_Скріншот: Результат оновлення профілю у Postman_

![Оновлення профілю](/assets/labs/lab-3/update-profile.png)

### 2.11 Реалізувати видалення користувача (Завдання 17)

```javascript
// controllers/userController.js
const deleteAccount = async (req, res, next) => {
    try {
        const user = await User.findByPk(req.user.id);
        if (!user) return res.status(404).json({ error: 'Користувача не знайдено' });

        await user.destroy();
        res.json({ message: 'Акаунт успішно видалено' });
    } catch (err) {
        next(err);
    }
};
```

_Скріншот: Успішне видалення акаунту через API_

![Видалення користувача](/assets/labs/lab-3/delete-user.png)

### 2.12 Додати підтвердження email та відновлення пароля (Завдання 19 та 18)

При реєстрації генерується токен та надсилається на пошту (через nodemailer). Користувач переходить за лінком для активації акаунта. Те ж саме для відновлення пароля.

```javascript
// controllers/authController.js
const forgotPassword = async (req, res, next) => {
    try {
        const { email } = req.body;
        const user = await User.findOne({ where: { email } });
        if (!user) return res.status(404).json({ error: 'Користувача не знайдено' });

        const resetToken = crypto.randomBytes(32).toString('hex');
        user.resetPasswordToken = resetToken;
        user.resetPasswordExpires = Date.now() + 3600000; // 1 година
        await user.save();

        const resetUrl = `http://localhost:3000/api/auth/reset/${resetToken}`;
        await sendMail(email, 'Відновлення пароля', `<p>Для відновлення: <a href="${resetUrl}">${resetUrl}</a></p>`);

        res.json({ message: 'Лист відправлено' });
    } catch (err) {
        next(err);
    }
};
```

_Скріншот 1: Отриманий лист для підтвердження на пошту_

![Лист для підтвердження](/assets/labs/lab-3/email-sent.png)

_Скріншот 2: Сторінка успішного підтвердження після переходу_

![Сторінка успішного підтвердження](/assets/labs/lab-3/email-confirmed-page.png)

_Скріншот 3: Отриманий лист для відновлення пароля_

![Лист відновлення](/assets/labs/lab-3/email-reset.png)

### 2.13 Реалізувати OAuth (Google login) (Завдання 20)

Доступний швидкий вхід через акаунт Google за допомогою `passport-google-oauth20`:

```javascript
// routes/authRoutes.js
router.get('/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

router.get('/google/callback', passport.authenticate('google', { session: false, failureRedirect: '/login' }), async (req, res) => {
    try {
        const user = req.user;
        const tokens = generateTokens(user);
        user.refreshToken = tokens.refreshToken;
        await user.save();
        
        // Передача токена на фронтенд
        res.send(`<script>window.opener.postMessage({ type: 'GOOGLE_LOGIN_SUCCESS', token: '${tokens.accessToken}' }, '*'); window.close();</script>`);
    } catch (error) {
        res.status(500).json({ error: 'Internal Server Error' });
    }
});
```

_Скріншот: Вікно авторизації через Google_

![Авторизація через Google](/assets/labs/lab-3/google-oauth.png)

### 2.14 Протестувати API та проаналізувати результати (Завдання 5 та 6)

API було протестовано за допомогою Postman. Користувачі були створені та авторизовані. Токени отримані та використані у `Bearer Token` для доступу до приватних маршрутів (наприклад, профілю). Після 15 хвилин Access Token втрачав валідність, а Refresh Token успішно оновлював сесію. При 5 невдалих спробах входу спрацьовував Rate Limiter та блокував наступні спроби. 

_Скріншот 1: Тестування реєстрації нового користувача у Postman_

![Результат реєстрації у Postman](/assets/labs/lab-3/register.png)

_Скріншот 2: Тестування авторизації та отримання JWT токенів_

![Результат отримання токенів](/assets/labs/lab-3/login.png)

_Скріншот 3: Доступ до захищеного профілю з Bearer Token_

![Захищений маршрут](/assets/labs/lab-3/protected.png)

_Скріншот 4: Спрацювання Rate Limiter при частих спробах входу_

![Обмеження запитів](/assets/labs/lab-3/rate-limit.png)

_Скріншот 5: Успішне підтвердження email у браузері_

![Підтвердження email](/assets/labs/lab-3/email-confirm.png)

Результати показали, що архітектура повністю відповідає вимогам безпеки. Аутентифікація через JWT є зручною системою для REST API, а Google OAuth значно спрощує досвід користувачів.

---

## Висновки

У ході проведення лабораторної роботи було створено імплементацію автентифікації та авторизації з використанням популярного підходу - JWT-токенів. Були реалізовані реєстрація, вхід, відновлення пароля та підтвердження поштової скриньки за допомогою Nodemailer. 
Також була налаштована інтеграція Google OAuth для швидкого входу, реалізовано рольовий доступ, валідацію вхідних даних, ліміт помилкових запитів (rate limiting) та логування процесів. Завдяки розділенню токенів на Access та Refresh забезпечено максимальний баланс між безпекою та зручністю використання сервісу. Всі модулі було успішно протестовано за допомогою Postman, і вони показали надійні та коректні результати при будь-яких сценаріях вводу від користувача.
