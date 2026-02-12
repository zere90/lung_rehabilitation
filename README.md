# 🫁 Образовательная платформа "Реабилитация лёгких"

Полная платформа для восстановления после лечения рака лёгких с системой уроков, тестированием и выдачей сертификатов.

## 📋 Возможности

- ✅ Регистрация и авторизация пользователей
- ✅ 7 образовательных уроков
- ✅ Отслеживание прогресса обучения
- ✅ Итоговое тестирование (5/7 для прохождения)
- ✅ Автоматическая выдача сертификатов
- ✅ Новостная секция
- ✅ Личный кабинет с профилем

---

## 🚀 ИНСТРУКЦИЯ ПО ЗАПУСКУ

### Шаг 1: Установка зависимостей

```bash
npm install
```

### Шаг 2: Настройка MongoDB Atlas

#### 2.1 Создание кластера

1. Перейдите на https://www.mongodb.com/cloud/atlas/register
2. Зарегистрируйтесь (или войдите)
3. Нажмите "Build a Database"
4. Выберите **FREE tier** (M0)
5. Выберите регион (рекомендуется ближайший к вам)
6. Нажмите "Create Deployment"

#### 2.2 Создание пользователя базы данных

1. После создания кластера появится окно "Security Quickstart"
2. Создайте пользователя:
   - Username: `lungrehab`
   - Password: (сгенерируйте сложный пароль)
   - **ВАЖНО: Сохраните пароль!**
3. Нажмите "Create User"

#### 2.3 Настройка доступа

1. В разделе "Network Access":
   - Нажмите "Add IP Address"
   - Выберите "Allow Access from Anywhere" (0.0.0.0/0)
   - Нажмите "Confirm"

#### 2.4 Получение строки подключения

1. Вернитесь в "Database" → "Connect"
2. Выберите "Connect your application"
3. Скопируйте connection string:
   ```
   mongodb+srv://lungrehab:<password>@cluster0.xxxxx.mongodb.net/?retryWrites=true&w=majority
   ```
4. Замените `<password>` на ваш пароль
5. Добавьте название базы данных после `.net/`:
   ```
   mongodb+srv://lungrehab:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/lung-rehab?retryWrites=true&w=majority
   ```

### Шаг 3: Создание .env файла

Создайте файл `.env` в корне проекта:

```env
MONGODB_URI=mongodb+srv://lungrehab:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/lung-rehab?retryWrites=true&w=majority
SESSION_SECRET=your-super-secret-key-change-me-please
NODE_ENV=development
```

**Генерация SESSION_SECRET:**
```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

### Шаг 4: Запуск локально

```bash
# Development (с автоперезагрузкой)
npm run dev

# Production
npm start
```

Откройте http://localhost:3000

---

## ☁️ ДЕПЛОЙ НА RENDER

### Шаг 1: Подготовка репозитория

1. Создайте репозиторий на GitHub
2. Загрузите проект:

```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

**ВАЖНО:** Не загружайте файл `.env` в GitHub (он в `.gitignore`)

### Шаг 2: Создание Web Service на Render

1. Перейдите на https://render.com
2. Зарегистрируйтесь/войдите
3. Нажмите "New+" → "Web Service"
4. Подключите ваш GitHub репозиторий
5. Настройте сервис:

**Settings:**
- **Name**: `lung-rehab-platform`
- **Environment**: `Node`
- **Build Command**: `npm install`
- **Start Command**: `npm start`
- **Plan**: `Free`

### Шаг 3: Добавление переменных окружения

В разделе "Environment" добавьте:

```
MONGODB_URI=mongodb+srv://lungrehab:YOUR_PASSWORD@cluster0.xxxxx.mongodb.net/lung-rehab?retryWrites=true&w=majority
SESSION_SECRET=your-generated-secret-from-step-3
NODE_ENV=production
```

### Шаг 4: Деплой

1. Нажмите "Create Web Service"
2. Render автоматически задеплоит приложение
3. После деплоя откройте URL: `https://your-app-name.onrender.com`

---

## 🔧 Структура проекта

```
lung-rehab-final/
├── server.js              # Express сервер
├── models.js              # Mongoose модели
├── package.json           # Зависимости
├── .env                   # Переменные окружения (НЕ коммитить!)
├── .env.example           # Пример .env
├── README.md              # Эта инструкция
└── public/                # Frontend файлы
    ├── index.html         # Главная
    ├── auth.html          # Авторизация
    ├── education.html     # Список уроков
    ├── lesson1-7.html     # Уроки
    ├── test.html          # Тест
    ├── certificate.html   # Сертификат
    ├── profile.html       # Профиль
    ├── news.html          # Новости
    ├── news-article.html  # Статья новости
    ├── style.css          # Стили
    ├── auth-check.js      # Проверка авторизации
    ├── menu.js            # Мобильное меню
    ├── news-data.js       # Данные новостей
    └── progress.js        # Прогресс (deprecated)
```

---

## 🛠 API Endpoints

### Публичные
- `POST /api/register` - Регистрация
- `POST /api/login` - Вход
- `GET /health` - Проверка здоровья

### Защищённые (требуют авторизации)
- `GET /api/user` - Текущий пользователь
- `POST /api/logout` - Выход
- `GET /api/progress` - Прогресс уроков
- `POST /api/lesson/complete` - Отметить урок
- `POST /api/test/submit` - Отправить тест
- `GET /api/test/results` - Результаты тестов
- `GET /api/certificate` - Получить сертификат

---

## 🐛 Решение проблем

### MongoDB не подключается

1. Проверьте правильность MONGODB_URI в .env
2. Убедитесь, что IP разрешён в Network Access
3. Проверьте пароль пользователя БД

### Не работают cookies на Render

В `server.js` уже настроено:
```javascript
cookie: {
  secure: process.env.NODE_ENV === 'production',
  sameSite: 'none'
}
```

### Сертификат не генерируется

1. Убедитесь, что прошли все 7 уроков
2. Пройдите тест с результатом ≥5/7
3. Проверьте консоль браузера на ошибки

---

## 📄 Лицензия

ISC

## 👥 Авторы

Rehabilitation Team
