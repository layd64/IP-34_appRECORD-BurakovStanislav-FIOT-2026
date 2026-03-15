## Тема, Мета, Місце розташування

**Тема лабораторної:** СТВОРЕННЯ БАЗИ ДАНИХ У MYSQL. ПІДКЛЮЧЕННЯ NODE.JS ДО MYSQL. РОБОТА З ORM SEQUELIZE.

**Мета лабораторної:** Навчитися підключати реляційну базу даних до Node.js додатку, виконувати базові SQL-запити через пакет mysql2, використовувати ORM Sequelize для опису моделей та реалізовувати зв'язки між таблицями.

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

1. Створити базу даних, яка буде використовуватись веб-додатком (наприклад, web_backend_lab).
2. Створити таблиці у базі даних (у нашому випадку users та orders).
3. Виконати SQL-запити (SELECT, INSERT, UPDATE, DELETE).
4. Підключити Node.js до MySQL через пакет mysql2.
5. Виконати SQL-запити з Node.js.
6. Використати ORM Sequelize.
7. Створити моделі (User та Order).
8. Реалізувати зв’язок One-to-Many між моделями.

---

## 2 Хід Виконання Роботи

### 2.1 Створення бази даних та таблиць у MySQL (Завдання 1-2)

Дії зі створення бази даних та таблиць у MySQL можна реалізувати за допомогою пакету mysql2. 
Спочатку виконується підключення до сервера MySQL за допомогою реквізитів адміністратора (або іншого користувача). Потім виконуються SQL-запити для створення бази даних web_backend_lab, після чого налаштовується використання цієї бази за допомогою команди USE web_backend_lab;.
Одразу після цього створюються таблиці: users для зберігання інформації про користувачів, та orders для зберігання їх замовлень. Між таблицями встановлюється зв'язок за зовнішнім ключем (FOREIGN KEY), щоб кожне замовлення належало певному користувачеві.

```javascript
const mysql = require('mysql2/promise');

async function runTasks() {
    console.log('--- Демонстрація роботи mysql2 (Завдання 1-2) ---');

    // Підключення до MySQL сервера
    const connection = await mysql.createConnection({
        host: 'localhost',
        user: 'root',
        password: 'тут_пароль_бд'
    });

    console.log('1. Створення бази даних web_backend_lab...');
    await connection.query(`CREATE DATABASE IF NOT EXISTS web_backend_lab;`);

    // Переключення на створену базу даних
    await connection.query(`USE web_backend_lab;`);

    console.log('2. Створення таблиць users та orders...');
    await connection.query(`
        CREATE TABLE IF NOT EXISTS users (
            id INT AUTO_INCREMENT PRIMARY KEY,
            username VARCHAR(50) NOT NULL,
            email VARCHAR(100) NOT NULL UNIQUE
        );
    `);

    await connection.query(`
        CREATE TABLE IF NOT EXISTS orders (
            id INT AUTO_INCREMENT PRIMARY KEY,
            user_id INT,
            total_amount DECIMAL(10, 2) NOT NULL,
            status VARCHAR(50) DEFAULT 'pending',
            FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
        );
    `);
```

### 2.2 Виконання SQL-запитів: SELECT, INSERT, UPDATE, DELETE (Завдання 3-5)

Після успішного створення таблиць виконуємо базові SQL-запити для наповнення бази даних даними, їх читання, оновлення та видалення:

* **INSERT** використовується для додавання нового користувача у таблицю users та нового замовлення у таблицю orders.
* **SELECT** дозволяє вибрати дані з бази даних. Використання оператора JOIN у запиті наочно демонструє зв'язок між користувачем та його замовленнями і дозволяє вивести їх спільно.
* **UPDATE** показує зміну вже існуючих даних, у даному випадку відбувається оновлення імені користувача за його ідентифікатором.
* **DELETE** використовується для видалення записів з таблиці. У нашому випадку видалення користувача також призведе до автоматичного видалення його замовлень, оскільки ми використовували ON DELETE CASCADE при створенні таблиці.

```javascript
    console.log('3 & 5. Виконання SQL-запитів з Node.js...');

    // INSERT
    console.log('--> INSERT заявка у таблиці:');
    await connection.query(`INSERT IGNORE INTO users (username, email) VALUES ('Ivan', 'ivan@example.com');`);

    const [[user]] = await connection.query(`SELECT id FROM users WHERE email = 'ivan@example.com' LIMIT 1;`);
    const userId = user.id;

    await connection.query(`INSERT INTO orders (user_id, total_amount, status) VALUES (?, 450.00, 'completed');`, [userId]);
    
    // SELECT
    console.log('--> SELECT (Отримання даних):');
    const [rows] = await connection.query(`SELECT users.username, orders.total_amount, orders.status FROM users JOIN orders ON users.id = orders.user_id;`);
    console.log('Результат:', rows);

    // UPDATE
    console.log('--> UPDATE (Оновлення даних):');
    await connection.query(`UPDATE users SET username = 'Ivan_Updated' WHERE id = ?;`, [userId]);
    
    // DELETE (Очищення даних після тесту)
    console.log('--> DELETE (Видалення):');
    const [deleteResult] = await connection.query(`DELETE FROM users WHERE id = ?;`, [userId]);
    
    console.log('Усі завдання mysql2 виконано успішно!');
    await connection.end();
}

runTasks().catch(console.error);
```

### 2.3 Підключення бази даних до ORM Sequelize (Завдання 6)

Для того, щоб використовувати Sequelize, у файлі db.js налаштовано спільне підключення до бази даних:

```javascript
const { Sequelize } = require('sequelize');

// підключення до бд
const sequelize = new Sequelize('web_backend_lab', 'root', 'тут_пароль_бд', {
    host: 'localhost',
    dialect: 'mysql',
    logging: false, // вимкнути логування SQL-запитів у консоль
});

module.exports = sequelize;
```

### 2.4 Створення моделей (Завдання 7)

Визначення моделі User у файлі models/User.js:

```javascript
const { DataTypes } = require('sequelize');
const sequelize = require('../db');

// Створення моделі User
const User = sequelize.define('User', {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    username: {
        type: DataTypes.STRING,
        allowNull: false
    },
    email: {
        type: DataTypes.STRING,
        allowNull: false,
        unique: true
    }
}, {
    tableName: 'users',
    timestamps: false
});

module.exports = User;
```

Визначення моделі Order (яка грає роль підлеглої сутності замість Post) у файлі models/Order.js:

```javascript
const { DataTypes } = require('sequelize');
const sequelize = require('../db');

// Створення моделі Order
const Order = sequelize.define('Order', {
    id: {
        type: DataTypes.INTEGER,
        primaryKey: true,
        autoIncrement: true
    },
    total_amount: {
        type: DataTypes.DECIMAL(10, 2),
        allowNull: false
    },
    status: {
        type: DataTypes.STRING,
        defaultValue: 'pending'
    }
}, {
    tableName: 'orders',
    timestamps: true 
});

module.exports = Order;
```

### 2.5 Реалізація зв’язку One-to-Many (Завдання 8)

Зв'язок налаштовано за допомогою методів hasMany та belongsTo у файлі ініціалізації моделей models/index.js, він відповідає асоціації один-до-багатьох (в одного користувача може бути багато замовлень).

```javascript
const sequelize = require('../db');
const User = require('./User');
const Order = require('./Order');

// Реалізація зв'язку One-to-Many
User.hasMany(Order, {
    foreignKey: 'user_id',
    as: 'orders',
    onDelete: 'CASCADE'
});

Order.belongsTo(User, {
    foreignKey: 'user_id',
    as: 'user'
});

module.exports = {
    sequelize,
    User,
    Order
};
```

### 2.6 Використання моделей в Express-сервері

У файлі index.js налаштовані маршрути API, які використовують моделі Sequelize для отримання і запису інформації з урахуванням зв'язків:

```javascript
// ...
// Sequelize ORM: Отримати всіх користувачів разом з їхніми замовленнями
app.get('/api/users', async (req, res) => {
  try {
    const users = await User.findAll({ include: 'orders' });
    res.json(users);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

// Додати нового користувача
app.post('/api/users', async (req, res) => {
  try {
    const { username, email } = req.body;
    const newUser = await User.create({ username, email });
    res.status(201).json(newUser);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Додати нове замовлення для користувача
app.post('/api/users/:userId/orders', async (req, res) => {
  try {
    const { userId } = req.params;
    const { total_amount, status } = req.body;
    const newOrder = await Order.create({ total_amount, status, user_id: userId });
    res.status(201).json(newOrder);
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Синхронізація з БД та запуск сервера
sequelize.sync({ alter: true }).then(() => {
  console.log('Підключення до бази даних через Sequelize успішне, моделі синхронізовані.');
  app.listen(PORT, () => {
    console.log(`Сервер запущено на http://localhost:${PORT}`);
  });
}).catch(err => {
  console.error('Помилка підключення до бази даних:', err);
});
```

_Скріншот 1: Виконання скрипту mysql2-demo.js_

![Результат виконання mysql2-demo.js](/assets/labs/lab-2/mysql2-demo.png)

---

## Висновки

У ході лабораторної роботи було створено і налаштовано базу даних у СУБД MySQL. Вивчено та виконано базові SQL-запити (SELECT, INSERT, UPDATE, DELETE) як напряму, так і за допомогою пакету mysql2 у середовищі Node.js. 

Крім того, до серверного застосунку успішно підключено та налаштовано ORM Sequelize, створено відповідні моделі для користувачів (User) і замовлень (Order) — що виступають замість моделі записів (Post) у рамках реалізації магазину. Реалізовано зв'язок один-до-багатьох (One-to-Many), який дозволяє ефективно працювати з пов'язаними даними, зокрема завантажувати користувачів разом із їхніми замовленнями через API роути. Замикаючий етап підтвердив коректну автоматичну синхронізацію таблиць та ефективність використання ORM-підходу у подальшій розробці.
