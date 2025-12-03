📱 **Electronics Store** - Система управления интернет-магазином электроники

**Полнофункциональная база данных** для управления интернет-магазином электроники с поддержкой иерархических категорий, управления запасами, обработки заказов и автоматизации бизнес-процессов.

📋 **Содержание**

**Возможности
Структура базы данных
Установка и настройка
Основные таблицы
Запросы и отчеты
Триггеры и автоматизация
Хранимые процедуры
Примеры использования
Производительность
Лицензия**


✨ **Возможности**

🎯 **Иерархические категории товаров** - поддержка многоуровневой структуры категорий
📦 **Управление запасами** - автоматическое обновление остатков при оформлении заказов
🛒 **Полный цикл заказа** - от создания до доставки с различными статусами
👥 **Управление клиентами** - регистрация, история заказов, аналитика
📊 **Аналитика продаж** - отчеты по популярным товарам, выручке, клиентам
⚡ **Автоматизация** - триггеры для обновления данных в реальном времени
🔒 **Целостность данных** - внешние ключи, проверки, уникальные ограничения


🗄️ **Структура базы данных**

**Диаграмма схемы:**


┌─────────────┐       ┌─────────────┐       ┌─────────────┐
│  Categories │◄──────┤   Products  │◄──────┤ Order_items │
└─────────────┘       └─────────────┘       └─────────────┘
      ▲                      ▲                      │
      │                      │                      │
      │                ┌─────────────┐              │
      └────────────────┤   Orders    │◄─────────────┘
                       └─────────────┘
                              │
                              │
                       ┌─────────────┐
                       │  Customers  │
                       └─────────────┘

🚀 **Установка и настройка**

**Требования**

MySQL Server 8.0+ (или 5.7 с ограничениями)
MySQL Workbench или другой клиент
Права на создание базы данных


📊 Основные таблицы

1. Categories - Категории товаров
```
CREATE TABLE Categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,           -- Название категории
    parent_category_id INT NULL,          -- Родительская категория
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   
);
```

2. Products - Товары
```
CREATE TABLE Products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255) NOT NULL,           -- Название товара
    description TEXT,                     -- Описание
    price DECIMAL(10, 2) NOT NULL,        -- Цена
    category_id INT NOT NULL,             -- Категория
    stock_quantity INT NOT NULL DEFAULT 0, -- Количество на складе
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

);

```
3. Customers - Клиенты
```
CREATE TABLE Customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL UNIQUE,   -- Email (уникальный)
    phone VARCHAR(20) NOT NULL UNIQUE,    -- Телефон (уникальный)
    registration_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

```
4. Orders - Заказы
```
CREATE TABLE Orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    customer_id INT NOT NULL,             -- ID клиента
    order_date DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    status ENUM('new', 'processing', 'shipped', 'delivered', 'cancelled'),
    total_amount DECIMAL(10, 2) DEFAULT 0.00, -- Общая сумма заказа
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

```
5. Order_items - Позиции заказа
```
CREATE TABLE Order_items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,                -- ID заказа
    product_id INT NOT NULL,              -- ID товара
    quantity INT NOT NULL,                -- Количество
    unit_price DECIMAL(10, 2) NOT NULL,   -- Цена за единицу на момент заказа
    subtotal DECIMAL(10, 2) GENERATED ALWAYS AS (quantity * unit_price) STORED,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
                      
