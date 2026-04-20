## Тема, Мета, Місце розташування

**Тема лабораторної:** РОЗРОБКА ФУНКЦІОНАЛЬНОГО REST API. РЕЄСТРАЦІЯ ТА АВТОРИЗАЦІЯ КОРИСТУВАЧІВ. ВАЛІДАЦІЯ ДАНИХ І ОБРОБКА ПОМИЛОК.

**Мета лабораторної:** вивчення принципів побудови REST API; набуття практичних навичок розробки серверного застосунку з використанням платформи Node.js і фреймворку Express; реалізувати механізми реєстрації та авторизації користувачів; забезпечити валідацію вхідних даних; забезпечити обробку помилок; організувати захищений доступ до ресурсів із використанням JWT-токенів і системи ролей користувачів.

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

Для реалізації безпечної автентифікації та розширення функціоналу бекенду було встановлено набір спеціалізованих бібліотек, що відповідають сучасним стандартам безпеки веб-застосунків. Вони визначені у `package.json`:
* `bcrypt` - використовується для надійного криптографічного хешування паролів перед їх збереженням у базу даних щоб уникнути зберігання паролів у відкритому вигляді.
* `jsonwebtoken` - застосовується для генерації токенів доступу Access та Refresh, що дозволяє реалізувати Stateless-сесії без збереження стану на сервері.
* `express-validator` - забезпечує зручний та потужний механізм валідації даних, що надходять від клієнта, наприклад, перевірка формату email чи довжини пароля.
* `nodemailer` - інструментарій для налаштування SMTP-підключення, до прикладу, через Gmail, для відправки транзакційних листів, таких як підтвердження реєстрації та скидання пароля.
* `passport` та `passport-google-oauth20` - стандартизовані middleware для інтеграції OAuth2, що дозволяють легко підключити авторизацію через акаунт Google.
* `express-rate-limit` - засіб захисту від брутфорс-атак, який блокує надмірну кількість запитів із однієї IP-адреси за короткий проміжок часу.

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

Для надійного збереження даних користувачів була спроєктована модель User за допомогою ORM-системи Sequelize. Модель охоплює усі необхідні поля для роботи сучасної системи авторизації: окрім базових username, email та password, передбачено зберігання refreshToken для подовження сесій, полів для підтвердження електронної пошти (confirmationToken, isEmailConfirmed), даних для відновлення забутого пароля (resetPasswordToken, resetPasswordExpires), а також googleId для зв'язку з профілем Google при OAuth авторизації. Це дозволяє гнучко розширювати функціонал користувача в майбутньому.

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

З метою впровадження системи контролю доступу у базу даних було додано поле role, яке за замовчуванням набуває значення user. Акаунти з роллю admin мають підвищені привілеї та доступ до специфічних ендпоінтів, наприклад, для видалення інших користувачів. Щоб забезпечити захист маршрутів на рівні ролей, було розроблено middleware `authorizeRole`. Він перевіряє роль авторизованого користувача, і якщо вона не збігається з дозволеною, негайно блокує запит, повертаючи помилку 403 Forbidden.

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

Автентифікація є ядром безпеки платформи. Реалізований метод register перевіряє унікальність email, після чого виконує незворотнє хешування пароля користувача (раунд 10 в bcrypt), генерує унікальний токен для підтвердження електронної пошти та зберігає профіль у системі.

Функція login замість класичних сесій використовує JWT. Вона звіряє введений пароль із розшифрованим хешем у базі. Якщо перевірка успішна і пошта підтверджена, зчитується id, email та role для створення підписаного Access Token, а також генерується Refresh Token, який зберігається як в базі, так і на клієнті.

З метою завершення сесії, метод logout знаходить користувача і обнулює його refreshToken у базі даних. Це робить неможливим поновлення доступу при зламі поточного токена.

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

_Скріншот: Успішна реєстрація на сайті_

![Реєстрація та авторизація](/assets/labs/lab-3/auth.png)

### 2.5 Додати валідацію даних та підтвердження пароля при реєстрації (Завдання 3 та 7)

Обробка невідкоректованих або шкідливих даних на ранньому етапі реалізована за допомогою пакету express-validator. Створено ланцюжки перевірок middlewares, що контролюють наявність необхідних полів, перевіряють, чи дійсно рядок є email-адресою, і встановлюють вимоги до мінімальної довжини пароля. 

Крім того, за допомогою кастомного валідатора реалізовано обов'язкове підтвердження пароля (завдання 7): система порівнює поля password та confirmPassword, генеруючи помилку в разі невідповідності. Якщо хоча б одна умова не виконується, користувачу повертається інформативна відповідь зі статусом 400 Bad Request.

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

_Скріншот: Спрацювання валідації паролів_

![Валідація даних](/assets/labs/lab-3/validation.png)

### 2.6 Додати логування помилок (Завдання 13)

Для полегшення аудиту та виявлення прихованих проблем на робочому сервері була реалізована система локального логування. Будь-яка необроблена помилка в застосунку обов'язково потрапляє в централізований errorLogger. Цей middleware формує мітку часу, зберігає метод та URL запиту, текст помилки, а також детальне трасування стеку. 

Зібрана інформація не лише виводиться у консоль, але й надійно дописується у фізичний файл error.log через модуль fs. При цьому клієнту безпечно віддається лише загальне повідомлення "Внутрішня помилка сервера" без розкриття подробиць архітектури інфраструктури.

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

_Скріншот: Логування помилки у файл_

![Логування помилок](/assets/labs/lab-3/error-log.png)

### 2.7 Додати middleware для перевірки токена та захищений маршрут (Завдання 4 та 15)

Захист закритих ресурсів базується на імплементації прошарку authenticateToken. Даний middleware перехоплює HTTP-запит і отримує токен доступу зі стандартизованого заголовка `Authorization: Bearer <token>`. Якщо токен відсутній, доступ перманентно заборонено (401 Unauthorized).

За допомогою методу jwt.verify і секретного ключа перевіряється справжність токена. У разі позитивної верифікації розшифровані дані (наприклад, ID та роль) записуються в об'єкт req.user, і управління передається наступному контролеру (один із захищених маршрутів - /api/users/profile).

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

_Скріншот: Блокування доступу без токена адміністратора (Захищений маршрут)_

![Захищений маршрут](/assets/labs/lab-3/protected-route.png)

### 2.8 Реалізувати refresh token (Завдання 12)

Через те, що accessToken має короткий час життя (зазвичай 15 хвилин), було введено механізм оновлення сесій за допомогою refreshToken який діє 7 днів. Концепція така: як тільки клієнт отримує помилку про "прострочений" токен доступу, він відправляє свій збережений refreshToken на маршрут /api/auth/refresh.

Сервер розшифровує токен, знаходить користувача в БД, і головне - перевіряє, чи відповідає переданий токен тому, що зараз зафіксований в базі. Якщо так, генерується та повертається нова пара токенів. Це захищає користувача від викрадення довгострокового токена (компрометований refreshToken можна анулювати примусовим логаутом).

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

З метою запобігання атакам типу Brute Force (підбору паролів) і уникнення виснаження ресурсів сервера (DDoS), використано пакет express-rate-limit. Ендпоінт авторизації /login огорнуто цим обмеженням: воно запам'ятовує спроби підключень із конкретної IP-адреси. Якщо користувач робить 5 безуспішних спроб запровадити пароль протягом 15 хвилин, всі подальші запити цього IP тимчасово блокуються із відповіддю "Забагато спроб входу".

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

Маршрути управління профілем дозволяють клієнтам ефективно працювати зі своїми даними. Для оновлення інформації передбачено підтримку часткових змін - сервіс перезаписує лише ті поля (телефон, адреса, ім'я), які користувач передав у тілі запиту. Це позбавляє необхідності передавати повний об'єкт наново.

Зміна пароля включає критично важливий етап безпеки: вона вимагає підтвердження старого пароля через bcrypt.compare. У разі успішної верифікації новий пароль надійно хешується і перезаписується. Обидві функції працюють лише за наявності валідного токена, ідентифікуючи запитувача через ідентифікатор в токені.

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

_Скріншот: Результат оновлення профілю_

![Оновлення профілю](/assets/labs/lab-3/update-profile.png)

### 2.11 Реалізувати видалення користувача (Завдання 17)

Акаунт завжди можна видалити - це реалізовано двома шляхами: самостійне видалення авторизованим клієнтом через метод deleteAccount, де ідентифікатор береться напряму з токена req.user.id або адміністративне примусове видалення з панелі керування (маршрут із перевіркою roleMiddleware('admin') та передачею цільового ID у параметрах). З використанням Sequelize достатньо визвати метод destroy(), що гарантує безболісне видалення як користувача, так і його дочірніх даних завдяки каскадному видаленню.

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

_Скріншот: Видалення акаунту через панель адміністратора_

![Видалення користувача](/assets/labs/lab-3/delete-user.png)

### 2.12 Додати підтвердження email та відновлення пароля (Завдання 19 та 18)

Процеси верифікації пошти та скидання забутого пароля архітектурно подібні. Під час створення облікового запису генерується випадкова 32-байтова строка (confirmationToken або resetPasswordToken). Цей унікальний ідентифікатор зберігається в БД поряд із даними користувача.

Завдяки сервісу nodemailer, система автоматично компонує та надсилає електронного листа через Gmail SMTP на пошту клієнта. Лист містить унікальне посилання, і як тільки клієнт переходить за ним, контролер верифікує токен, переводить статус акаунту isEmailConfirmed в true або дозволяє перезаписати новий хеш пароля, і анулює використаний токен з міркувань безпеки, унеможливлюючи повторне застосування.

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

_Скріншот 1: Повідомлення про підтвердження пошти_

![Повідомлення про підтвердження пошти](/assets/labs/lab-3/email-sent.png)

_Скріншот 2: Отриманий лист для підтвердження на пошту_

![Отриманий лист для підтвердження на пошту](/assets/labs/lab-3/email-confirmed-page.png)

_Скріншот 3: Сторінка успішного підтвердження після переходу_

![Сторінка успішного підтвердження після переходу](/assets/labs/lab-3/email-reset.png)

### 2.13 Реалізувати OAuth (Google login) (Завдання 20)

Щоб покращити користувацький досвід і дозволити клієнтам входити у застосунок менш ніж за секунду (через клік на кнопку "Login with Google"), здійснена інтеграція Google OAuth 2.0 за стандартами фреймворку Passport.js. Контролер /google ініціює відкриття панелі вибору облікового запису Google. Присвоївши системі успішний дозвіл, користувач спрямовується до callback ресурсу.

Отримавши Google-профіль, система перевіряє, чи привязаний вже цей акаунт до БД. Якщо користувача не знайдено, система миттєво створює для нього обліковий запис у "тихому режимі". Фінальним кроком є миттєва генерація пари JWT-токенів. Через те, що Google OAuth оперує в новому вікні, токени безпечно делегуються на фронтенд-аплікацію через API браузера window.opener.postMessage(). Це безшовно завершує процес авторизації.

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

API було протестовано за допомогою Postman. Користувачі були створені та авторизовані. Токени отримані та використані у Bearer Token для доступу до приватних маршрутів (наприклад, профілю). Після 15 хвилин Access Token втрачав валідність, а Refresh Token успішно оновлював сесію. При 5 невдалих спробах входу спрацьовував Rate Limiter та блокував наступні спроби. 

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
