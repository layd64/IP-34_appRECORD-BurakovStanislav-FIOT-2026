## Тема, Мета, Місце розташування

**Тема лабораторної:** ВИБІР ПРЕДМЕТНОЇ ОБЛАСТІ. АНАЛІЗ, МОДЕЛЮВАННЯ ТА РОЗРОБЛЕННЯ АДАПТИВНОГО WEB-ЗАСТОСУНКУ.

**Мета лабораторної:** Навчитися формулювати ключові складові опису інформаційної системи: актуальність, мету та завдання, об’єкт і предмет роботи, практичне значення, функціональні та нефункціональні вимоги, Use-case та ER-діаграми.

---

**Назва проєкту:** BookStore Pro

**Тема проєкту:** Веб-застосунок для онлайн-магазину книг

**Мета проєкту:** Прискорення та спрощення процесу купівлі книг, забезпечення зручного доступу до широкого асортименту літератури, створення інтуїтивного інтерфейсу для користувачів, підтримка ефективного управління каталогом і замовленнями для адміністраторів.

---

*Посилання на репозиторій власного WEB-застосунку:* [https://github.com/layd64/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026](https://github.com/layd64/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026)

*Посилання на живу сторінку власного WEB-застосунку:* [layd64.github.io/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026](https://layd64.github.io/IP-34_appWEB-Backend-Burakov-Stanislav-FIOT-2026/)

*Посилання на репозиторій звітного HTML-документа:* [github.com/layd64/IP-34_appRECORD-BurakovStanislav-FIOT-2026](https://github.com/layd64/IP-34_appRECORD-BurakovStanislav-FIOT-2026)

*Посилання на живу сторінку звітного HTML-документа:* [layd64.github.io/IP-34_appRECORD-BurakovStanislav-FIOT-2026](https://layd64.github.io/IP-34_appRECORD-BurakovStanislav-FIOT-2026/)

*Посилання на макет сторінки у Figma:* [figma.com/design/JzXNXmCqtPJ3gEQfRZUPc1/BookStore-Pro](https://www.figma.com/design/JzXNXmCqtPJ3gEQfRZUPc1/BookStore-Pro?node-id=0-1&t=XVJOPUvVyByF5fVK-1)

---

## 1 Аналіз та Моделювання Системи

### 1.1 Актуальність теми

У сучасних умовах цифровізації освіти та зростаючого попиту на онлайн-сервіси доступ до книжкового контенту через інтернет стає все більш затребуваним. Традиційні книжкові магазини обмежені географічно та за асортиментом, тоді як онлайн-платформи дозволяють охопити ширшу аудиторію, пропонувати персоналізований пошук і фільтрацію, а також забезпечувати оперативне оновлення каталогу.

Проблема полягає у відсутності зручного, адаптивного та функціонального веб-застосунку для перегляду, пошуку та придбання книг, орієнтованого на різні категорії користувачів. Існуючі рішення часто є або надто громіздкими, або не адаптованими до мобільних пристроїв, що знижує задоволеність від їх використання.

### 1.2 Мета роботи

Метою роботи є проєктування та розроблення повноцінного веб-застосунку BookStore Pro - платформи цифрового книжкового магазину з клієнтською та серверною частинами. Застосунок забезпечує надійне зберігання даних у реляційній базі даних, серверну обробку запитів через REST API, а також адаптивний інтерфейс, що коректно відображається на пристроях з різною роздільною здатністю.

### 1.3 Завдання роботи

Для досягнення поставленої мети необхідно виконати такі завдання:

1. Визначити предметну область та обґрунтувати актуальність розробки.
2. Сформулювати функціональні та нефункціональні вимоги до системи.
3. Побудувати Use-case діаграму для опису взаємодії акторів із системою.
4. Побудувати ER-діаграму для представлення структури бази даних.
5. Розробити адаптивний HTML/CSS/JS інтерфейс (frontend) з компонентами: header, main, footer.
6. Реалізувати адаптивну верстку з використанням Flexbox/Grid та медіа-запитів.
7. Реалізувати «бургер-меню» для мобільних та планшетних пристроїв.
8. Розробити серверну частину (backend).
9. Спроєктувати та реалізувати реляційну базу даних відповідно до ER-діаграми.
10. Реалізувати систему реєстрації та особистого кабінету.
11. Реалізувати каталог книг із серверним пошуком, фільтрацією, сортуванням та пагінацією.
12. Реалізувати кошик покупок та систему відгуків із збереженням у базі даних.
13. Організувати логічну файлову структуру проєкту та розмістити у репозиторії GitHub.

### 1.4 Об'єкт роботи

Об'єктом роботи є процес проєктування та розроблення повноцінного (full-stack) веб-застосунку для онлайн-продажу та перегляду книг, що включає клієнтську частину, серверний застосунок та реляційну базу даних.

### 1.5 Предмет роботи

Предметом роботи є методи та засоби розроблення full-stack веб-застосунку **BookStore Pro**: адаптивний інтерфейс, серверна частина, система автентифікації, а також реляційна база даних для надійного зберігання і обробки даних.

### 1.6 Бізнес-логіка роботи

Реєстрація та автентифікація:
1. Користувач вводить email і пароль.
2. Система валідує формат email і надійність пароля.
3. Створюється обліковий запис із унікальним ID.
4. Збираються уподобання користувача.

Відкриття та пошук книг:
1. Ввід запиту або застосування фільтрів.
2. Запит до бази даних із параметрами.
3. Ранжування за релевантністю, популярністю та уподобаннями.
4. Відображення з пагінацією та сортуванням.

Процес покупки:
1. Додавання книг до кошика.
2. Розрахунок ціни з податками та доставкою.
3. Оформлення замовлення та оплата через захищений шлюз.
4. Надсилання підтвердження замовлення.
5. Миттєвий доступ до цифрових книг; фізичні - готуються до відправки.

Управління контентом:
1. Подання метаданих книг видавцями/авторами.
2. Перевірка та валідація контенту.
3. Категоризація і тегування для пошуку.
4. Встановлення цін та доступності.
5. Публікація для користувачів.



### 1.7 Функціональні вимоги

Управління користувачами:
- Реєстрація з підтвердженням email
- Вхід/вихід
- Профіль та уподобання
- Ролі: клієнт, адміністратор, видавець

Управління книгами:
- Каталог і пошук
- Категорії та теги
- Доставка цифрових книг

Електронна комерція:
- Кошик
- Рахунки

Контент та спільнота:
- Рецензії та рейтинги
- Списки бажань та улюблені
- Підписка на розсилку



### 1.8 Нефункціональні вимоги

Продуктивність:
- Завантаження сторінки &lt; 3 с
- Відповідь БД &lt; 500 мс

Безпека:
- HTTPS-шифрування всіх передач даних

Зручність використання:
- Адаптивний дизайн
- Інтуїтивний інтерфейс
- Підтримка кількох мов



### 1.9 Use-case діаграма

Нижче представлена Use Case діаграма, яка показує основні взаємодії користувачів з системою.

<div style="background:#fff; padding:1rem; display:inline-block;">

![Use-case діаграма BookStore Pro](/assets/labs/lab-1/UML_FrontEnd.drawio.png)

</div>

### 1.10 ER-діаграма

ER-діаграма описує логічну структуру реляційної бази даних застосунку BookStore Pro, що охоплює всі функціональні вимоги системи.

<div style="background:#fff; padding:1rem; display:inline-block;">

![ER-діаграма BookStore Pro](/assets/labs/lab-1/ER.drawio.png)

</div>


---

## 2 Хід Виконання Роботи

### 2.1 Організація файлової структури проєкту

Проєкт організовано за принципом монорепозиторію з окремими директоріями для frontend та backend частин:

```
root/
├── html/
│   ├── index.html           # Головна сторінка
│   ├── catalog.html         # Каталог книг із фільтрацією
│   ├── book.html            # Деталі книги та відгуки
│   ├── about.html           # Опис застосунку
│   ├── login.html           # Сторінка входу
│   ├── register.html        # Сторінка реєстрації
│   └── profile.html         # Особистий кабінет
├── css/
│   └── styles.css           # Таблиця стилів
├── js/
│   ├── script.js            # Головна логіка та UI
│   ├── auth.js              # Автентифікація
│   ├── book-details.js      # Сторінка книги та відгуки
│   ├── profile.js           # Особистий кабінет
│   ├── login.js             # Форма входу
│   ├── register.js          # Форма реєстрації
│   └── toast.js             # Система сповіщень
└── assets/                  # Зображення та медіафайли
```

### 2.2 Реалізація шапки (header) з навігаційним меню та бургером

Шапка реалізована у кожному HTML-файлі та містить логотип, горизонтальне навігаційне меню, кнопку бургер-меню та окремий блок мобільного меню:

```html
<header>
    <nav>
        <div class="logo">
            <a href="#" aria-label="Go to home">
                <h1>BookStore Pro</h1>
            </a>
        </div>
        <ul class="nav-links">
            <li><a href="#home">Головна</a></li>
            <li><a href="catalog.html#catalog-title">Каталог</a></li>
            <li><a href="#categories">Категорії</a></li>
            <li id="authNavItem">
                <a href="login.html" id="loginLink">Вхід</a>
            </li>
            <li>
                <a href="#" id="cartBtn" class="cart-btn">
                    Кошик <span id="cartCount" class="cart-count">0</span>
                </a>
            </li>
        </ul>
        <div class="burger-menu">
            <div class="burger-icon">
                <span></span>
                <span></span>
                <span></span>
            </div>
        </div>
    </nav>
    <div class="mobile-menu">
        <ul class="mobile-nav-links">
            <li><a href="#home">Головна</a></li>
            <li><a href="catalog.html#catalog-title">Каталог</a></li>
            <li><a href="#categories">Категорії</a></li>
            <li id="mobileAuthNavItem">
                <a href="login.html" id="mobileLoginLink">Вхід</a>
            </li>
            <li><a href="#" id="mobileCartBtn">Кошик <span id="mobileCartCount">(0)</span></a></li>
        </ul>
    </div>
</header>
```

### 2.3 Реалізація основного блоку (main) із сіткою книг і категорій

Основний блок головної сторінки містить три секції: hero-блок привітання, сітку рекомендованих книг та секцію жанрових категорій. Картки книг реалізовані з обкладинкою, назвою, автором, ціною, рейтингом та кнопками «В КОШИК» і ♥ «Обране»:

```html
<main>
    <!-- Hero-секція -->
    <section id="home" class="hero">
        <div class="hero-content">
            <div class="hero-text">
                <h2>Welcome!</h2>
                <h3>Ласкаво просимо до BookStore Pro!</h3>
                <p>Ваш найкращий цифровий досвід читання.</p>
                <div class="button-container">
                    <a href="catalog.html#catalog-title">
                        <button type="button">Перейти до каталогу</button>
                    </a>
                </div>
            </div>
            <div class="hero-image-container">
                <img src="assets/stackOfBooks2.png"
                     alt="Modern Library Interior"
                     class="hero-image">
            </div>
        </div>
    </section>

    <!-- Сітка рекомендованих книг -->
    <section id="books" class="featured-books">
        <div class="section-heading">
            <h2>Рекомендовані книги</h2>
        </div>
        <div class="books-grid">
            <div class="book-card" data-book-id="1">
                <a href="book.html?id=1" class="book-card-link">
                    <img src="assets/kobzar.png" alt="Кобзар" class="book-image">
                    <h3>Кобзар</h3>
                    <p class="book-author">Тарас Шевченко</p>
                    <p class="book-price">300 грн</p>
                    <div class="book-rating-card">
                        <span class="book-rating-stars">⭐⭐⭐⭐⭐</span>
                        <span class="book-rating-value">5.0</span>
                    </div>
                </a>
                <div class="card-actions">
                    <button class="add-to-cart-btn" data-book-id="1">В КОШИК</button>
                    <button class="favorite-btn" data-book-id="1" aria-label="Add to favorites">
                        <svg viewBox="0 0 24 24" class="heart-icon">
                            <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5
                                     2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09
                                     C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42
                                     22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
                        </svg>
                    </button>
                </div>
            </div>
            <!-- ... інші картки -->
        </div>
    </section>

    <!-- Сітка категорій (CSS Grid, auto-fit) -->
    <section id="categories" class="categories">
        <div class="categories-grid">
            <div class="category-card" data-genre="Художня література">
                <div class="category-header">
                    <div class="category-icon">📚</div>
                    <h3>Художня література</h3>
                </div>
                <div class="category-content">
                    <p>Романи та повісті, що занурюють у нові світи.</p>
                    <div class="category-meta">
                        <a class="pill-link"
                           href="catalog.html?category=Художня література#catalog-title">
                            Переглянути
                        </a>
                    </div>
                </div>
            </div>
            <!-- ... інші категорії -->
        </div>
    </section>
</main>
```

### 2.4 Реалізація нижнього колонтитулу (footer)

Footer містить три колонки: бренд-блок, швидкі посилання та контактну інформацію:

```html
<footer>
    <div class="footer-content">
        <div class="footer-section">
            <h3>BookStore Pro</h3>
            <p>Ваша цифрова бібліотека сучасного світу</p>
        </div>
        <div class="footer-section">
            <h4>Швидкі посилання</h4>
            <ul>
                <li><a href="index.html">Головна</a></li>
                <li><a href="catalog.html#catalog-title">Каталог</a></li>
                <li><a href="index.html#categories">Категорії</a></li>
                <li><a href="profile.html">Профіль</a></li>
            </ul>
        </div>
        <div class="footer-section">
            <h4>Контактна інформація</h4>
            <p>Бураков Станіслав ІП-34</p>
            <p>Телефон: +1 (555) 111-1111</p>
        </div>
    </div>
</footer>
```

### 2.5 Стилізація та адаптивна верстка (styles.css)

**Загальні налаштування, шрифти та відносні одиниці:**

```css
/* Базовий розмір шрифту для rem-обчислень */
html {
    scroll-behavior: smooth;
    font-size: 16px;
    height: 100%;
}

body {
    font-family: 'Inter', sans-serif;
    background-color: #FFF6E8;
    color: #000000;
    line-height: 1.6;
    min-height: 100%;
    display: flex;
    flex-direction: column;
}

.container {
    max-width: 75rem;     /* відносні одиниці rem */
    margin: 0 auto;
    padding: 0 1.25rem;
}
```

**Адаптивна сітка книг (Flexbox із `flex-wrap`):**

```css
.books-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: clamp(1.5rem, 5vw, 6.25rem);      /* vw – відносно ширини вікна */
    max-width: min(75rem, 95vw);
    margin: 0 auto;
    padding: 0 clamp(1rem, 3vw, 1.25rem);
}

.book-card {
    flex: 1 1 16.25rem;     /* grow, shrink, base-width */
    max-width: 24rem;
    min-width: 16.25rem;
    background-color: white;
    padding: 1.25rem;
    box-shadow: 0 0.25rem 0.75rem rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
}

.book-card:hover {
    transform: translateY(-0.3125rem);  /* плавний підйом при наведенні */
}
```

**Адаптивна сітка категорій (CSS Grid, `auto-fit`):**

```css
.categories-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(15.625rem, 1fr));
    gap: 1.5rem;
    max-width: 75rem;
    margin: 0 auto;
    padding: 0 1.25rem;
    grid-auto-rows: 1fr;
}

.category-card {
    transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
}

.category-card:hover {
    transform: translateY(-4px);
    box-shadow: 0 0.9rem 2.2rem rgba(0, 0, 0, 0.08);
    border-color: rgba(247, 168, 35, 0.35);
}
```

**Медіа-запити для адаптивності:**

```css
/* Великі екрани */
@media (max-width: 1200px) {
    .nav-links { gap: 1.5rem; }
    .nav-links a { font-size: 1rem; }
}

/* Середні планшети */
@media (max-width: 1024px) {
    .categories-grid {
        grid-template-columns: repeat(auto-fit, minmax(13.75rem, 1fr));
    }
}

/* Планшети – портрет: приховати nav-links, показати бургер */
@media (max-width: 900px) {
    .nav-links { display: none; }
    .burger-menu { display: block; }
}

/* Планшети / більші смартфони */
@media (max-width: 768px) {
    .hero-content {
        flex-direction: column;
        text-align: center;
        gap: 2rem;
    }
    .hero-text h2 { font-size: 3.5rem; }
}

/* Ландшафтна орієнтація планшетів */
@media (max-width: 768px) and (orientation: landscape) {
    .books-grid {
        grid-template-columns: repeat(2, 1fr);
        gap: 2.5rem;
        padding: 0 3.125rem;
    }
}

/* Мобільні сповіщення (toast) */
@media (max-width: 425px) {
    .toast { padding: 0.875rem 1rem; gap: 0.75rem; }
    .toast-message { font-size: 0.875rem; }
}
```

**Стилі бургер-меню та його анімація перетворення в «X»:**

```css
.burger-menu {
    display: none;
    cursor: pointer;
    padding: 0.625rem;
}

.burger-icon span {
    display: block;
    position: absolute;
    height: 0.1875rem;
    width: 100%;
    background: #000000;
    transition: 0.2s ease;
}

.burger-icon span:nth-child(1) { top: 0; }
.burger-icon span:nth-child(2) { top: 0.5rem; }
.burger-icon span:nth-child(3) { top: 1rem; }

/* Перетворення на «X» при активації */
.burger-menu.active .burger-icon span:nth-child(1) {
    top: 0.5rem;
    transform: rotate(45deg);
}
.burger-menu.active .burger-icon span:nth-child(2) {
    opacity: 0;
}
.burger-menu.active .burger-icon span:nth-child(3) {
    top: 0.5rem;
    transform: rotate(-45deg);
}

/* Напівпрозорий блюр-оверлей при відкритому меню */
.menu-overlay {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    backdrop-filter: blur(5px);
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s ease, visibility 0.3s ease;
}
.menu-overlay.active { opacity: 1; visibility: visible; }
```

**CSS-анімації модальних вікон:**

```css
@keyframes fadeIn {
    from { opacity: 0; }
    to   { opacity: 1; }
}

@keyframes slideIn {
    from { transform: translate(-50%, -48%); opacity: 0; }
    to   { transform: translate(-50%, -50%); opacity: 1; }
}

.modal { animation: fadeIn 0.3s ease; }
.modal-content { animation: slideIn 0.3s ease; }

/* Обертання іконки закриття модала при наведенні */
.modal-close:hover {
    background-color: #e6951f;
    transform: rotate(90deg);
}
```

### 2.6 JavaScript: логіка застосунку (script.js, auth.js)

**Ініціалізація застосунку та реєстрація обробників:**

```js
document.addEventListener('DOMContentLoaded', function () {
    initMobileMenu();   // бургер-меню
    updateAuthUI();     // стан авторизації в хедері
    initCart();         // кошик покупок
    initNewsletter();   // форма розсилки

    const path = window.location.pathname;
    if (path.endsWith('index.html') || path === '/' || path.endsWith('/')) {
        renderHomePageCategories();
        initHomePage();
    } else if (path.endsWith('catalog.html')) {
        initCatalog();
    }

    initPasswordToggles(); // показ/приховання паролю
});
```

**Логіка бургер-меню з оверлеєм:**

```js
function initMobileMenu() {
    const burgerMenu = document.querySelector('.burger-menu');
    const mobileMenu = document.querySelector('.mobile-menu');
    let overlay = document.querySelector('.menu-overlay');

    // Динамічне створення оверлею
    if (!overlay) {
        overlay = document.createElement('div');
        overlay.className = 'menu-overlay';
        document.body.appendChild(overlay);
    }

    function closeMenu() {
        mobileMenu.classList.remove('active');
        burgerMenu.classList.remove('active');
        overlay.classList.remove('active');
        document.body.classList.remove('menu-open');
    }

    function toggleMenu() {
        const isActive = mobileMenu.classList.contains('active');
        if (isActive) {
            closeMenu();
        } else {
            mobileMenu.classList.add('active');
            burgerMenu.classList.add('active');
            overlay.classList.add('active');
            document.body.classList.add('menu-open');
        }
    }

    burgerMenu.onclick = (e) => { e.stopPropagation(); toggleMenu(); };
    overlay.addEventListener('click', closeMenu);

    // Автоматично закривати при розширенні вікна
    window.addEventListener('resize', () => {
        if (window.innerWidth > 768) closeMenu();
    });
}
```

**Система автентифікації (клас `AuthSystem` у `auth.js`):**

```js
class AuthSystem {
    constructor() {
        this.usersKey = 'bookstore_users';
        this.currentUserKey = 'bookstore_current_user';
        this.init();
    }

    register(userData) {
        const users = this.getUsers();
        // Перевірка унікальності email
        if (users.find(user => user.email === userData.email)) {
            return { success: false, message: 'Користувач з таким email вже існує' };
        }
        const newUser = {
            id: Date.now().toString(),
            email: userData.email,
            password: userData.password,
            fullName: userData.fullName,
            phone: userData.phone || '',
            address: userData.address || '',
            favorites: [],
            createdAt: new Date().toISOString()
        };
        users.push(newUser);
        localStorage.setItem(this.usersKey, JSON.stringify(users));
        this.login(userData.email, userData.password); // автовхід
        return { success: true, message: 'Реєстрація успішна!', user: newUser };
    }

    login(email, password) {
        const user = this.getUsers().find(u => u.email === email && u.password === password);
        if (!user) return { success: false, message: 'Невірний email або пароль' };
        // Зберегти сесію без пароля
        const userWithoutPassword = { ...user };
        delete userWithoutPassword.password;
        localStorage.setItem(this.currentUserKey, JSON.stringify(userWithoutPassword));
        return { success: true, message: 'Вхід успішний!', user: userWithoutPassword };
    }

    toggleFavorite(bookId) {
        const id = parseInt(bookId);
        const currentUser = this.getCurrentUser();
        if (!currentUser) return { success: false, message: 'Будь ласка, увійдіть' };
        const users = this.getUsers();
        const userIndex = users.findIndex(u => u.id === currentUser.id);
        const favorites = users[userIndex].favorites || [];
        const index = favorites.indexOf(id);
        if (index === -1) { favorites.push(id); }
        else { favorites.splice(index, 1); }
        users[userIndex].favorites = favorites;
        localStorage.setItem(this.usersKey, JSON.stringify(users));
        return { success: true, favorites };
    }
}

const authSystem = new AuthSystem();
```

**Фільтрація та рендеринг каталогу з пагінацією:**

```js
function initCatalog() {
    const BOOKS_PER_PAGE = 15;
    let currentPage = 1;
    let filteredBooks = [];

    function renderBooks() {
        currentPage = 1;
        const genreValue  = genreFilter.value;
        const ratingValue = parseInt(ratingFilter.value) || 0;
        const priceValue  = priceFilter.value;
        const searchValue = searchInput.value.toLowerCase().trim();

        // Збагачення книг поточними рейтингами з localStorage
        const booksWithRatings = window.catalogBooks.map(book => {
            const { ratingValue: liveRating, reviewCount } =
                getBookRatingInfo(book.id, book.rating);
            return { ...book, ratingValue: liveRating, reviewCount };
        });

        // Комбінована фільтрація
        filteredBooks = booksWithRatings.filter(book => {
            if (genreValue && book.genre !== genreValue) return false;
            if (book.ratingValue < ratingValue) return false;
            if (priceValue) {
                const [min, max] = priceValue.split('-')
                    .map(v => v === '+' ? Infinity : parseInt(v));
                if (book.price < min || book.price > max) return false;
            }
            if (searchValue) {
                const match = book.title.toLowerCase().includes(searchValue) ||
                              book.author.toLowerCase().includes(searchValue);
                if (!match) return false;
            }
            return true;
        });

        renderCurrentPage();
        renderPagination();
    }
    // ...
}
```

### 2.7 Тестування адаптивності

Перевірку адаптивності виконано за допомогою інструментів розробника Google Chrome. Протестовано на різних контрольних точках:

_Скріншот 1: Вигляд сайту на десктопі_

![Скрін 1 – десктоп](/assets/labs/lab-1/screen-desktop.png)

_Скріншот 2: Вигляд сайту на планшеті_

![Скрін 2 – планшет](/assets/labs/lab-1/screen-tablet.png)

_Скріншот 3: Вигляд сайту на смартфоні_

![Скрін 3 – смартфон](/assets/labs/lab-1/screen-mobile.png)


## 3 Частина 2: Основи Node.js та Express.js

У другій частині лабораторної роботи було виконано завдання зі створення базового HTTP-сервера за допомогою Node.js та фреймворку Express.js, а також реалізовано REST API для роботи з колекцією студентів.

### 3.1 Створення Node.js проєкту та встановлення Express.js (Завдання 1)

Було ініціалізовано новий Node.js проєкт у директорії серверної частини (app/server) за допомогою команди npm init -y. Після створення файлу package.json, було встановлено бібліотеку Express.js командою npm install express.

### 3.2 Створення базового HTTP-сервера (Завдання 2)

Створено головний файл сервера index.js, в якому підключено Express та налаштовано базовий маршрут для кореневого URL (/), який повертає привітальне повідомлення.

```javascript
const express = require('express');
const app = express();
const PORT = 3000;

// Middleware для парсингу JSON у тілі запитів
app.use(express.json());

// Завдання 2: Головний маршрут — повертає привітальне повідомлення
app.get('/', (req, res) => {
  res.send('Hello from Node.js server');
});
```

_Скріншот 4: Повідомлення від Node.js сервера (Hello from Node.js server)_

![Повідомлення від сервера](/assets/labs/lab-1/hello-node.png)

### 3.3 Створення маршруту для отримання списку студентів (Завдання 3)

Було створено масив об'єктів студентів (як імітацію бази даних) та реалізовано маршрут GET /students, який повертає цей список у форматі JSON.

```javascript
// масив студентів
let students = [
  { id: 1, name: 'Іванов Іван', group: 'IP-34' },
  { id: 2, name: 'Петренко Петро', group: 'IP-34' },
  { id: 3, name: 'Сидоренко Анна', group: 'IP-35' },
];

// Завдання 3: GET /students - отримати список усіх студентів
app.get('/students', (req, res) => {
  res.json(students);
});
```

_Скріншот 5: Відповідь сервера GET /students (список студентів у форматі JSON)_

![Список студентів](/assets/labs/lab-1/get-students.png)

### 3.4 Додавання нового студента (Завдання 4)

Створено маршрут POST /students, який приймає дані у форматі JSON, перевіряє їх на наявність обов'язкових полів (id, name, group), перевіряє чи немає дублювання ідентифікатора, та додає нового студента до масиву.

```javascript
// Завдання 4: POST /students - додати нового студента
app.post('/students', (req, res) => {
  const { id, name, group } = req.body;

  // Валідація: перевіряємо, що всі поля присутні
  if (!id || !name || !group) {
    return res.status(400).json({ error: 'Усі поля (id, name, group) є обовʼязковими' });
  }

  // Перевірка на дублювання id
  const exists = students.find((s) => s.id === id);
  if (exists) {
    return res.status(409).json({ error: `Студент з id=${id} вже існує` });
  }

  const newStudent = { id, name, group };
  students.push(newStudent);
  res.status(201).json(newStudent);
});
```

### 3.5 Оновлення та видалення студента (Завдання 5)

Для повноцінної роботи REST API було додано маршрути PUT /students/:id для оновлення полів існуючого студента та DELETE /students/:id для видалення студента з масиву за його ідентифікатором:

```javascript
// Завдання 5: PUT /students/:id — оновити дані студента
app.put('/students/:id', (req, res) => {
  const studentId = parseInt(req.params.id, 10);
  const index = students.findIndex((s) => s.id === studentId);

  if (index === -1) {
    return res.status(404).json({ error: `Студента з id=${studentId} не знайдено` });
  }

  const { name, group } = req.body;

  if (name) students[index].name = name;
  if (group) students[index].group = group;

  res.json(students[index]);
});

// DELETE /students/:id — видалити студента
app.delete('/students/:id', (req, res) => {
  const studentId = parseInt(req.params.id, 10);
  const index = students.findIndex((s) => s.id === studentId);

  if (index === -1) {
    return res.status(404).json({ error: `Студента з id=${studentId} не знайдено` });
  }

  const deleted = students.splice(index, 1);
  res.json({ message: `Студента з id=${studentId} видалено`, student: deleted[0] });
});
```

_Скріншот 6: Відпрацювання тестувань запитів API (наприклад, через Postman або Thunder Client)_

![Тестування запитів API](/assets/labs/lab-1/api-testing.png)

## Висновки

У ході практичної роботи було обрано предметну область - онлайн-магазин книг BookStore Pro - та обґрунтовано її актуальність. Визначено мету, об'єкт і предмет роботи, описано основні бізнес-процеси системи. Сформовано переліки функціональних вимог і нефункціональних вимог. Побудовано Use-case діаграму, що відображає взаємодію акторів із системою, та ER-діаграму структури даних. На практичному рівні створено головну сторінку веб-застосунку з семантично правильною структурою HTML: реалізовано навігацію, блок рекомендованих книг, секцію категорій із таблицею бестселерів, секцію статистики читання з табличними даними, форму підписки на розсилку та інформативний футер. Відпрацьовано верстку контенту різних типів, організовано логічну файлову структуру проєкту та підготовлено основу для подальшого стилювання й інтерактивності.

У другій частині роботи було успішно реалізовано базовий сервер на Node.js з використанням фреймворку Express.js. Окрім налаштування проєкту та встановлення залежностей, реалізовано повноцінний RESTful API для роботи зі списком студентів, що включає маршрути для отримання, додавання, оновлення та видалення записів. Це дозволило закріпити навички створення серверних застосунків та обробки HTTP-запитів.
