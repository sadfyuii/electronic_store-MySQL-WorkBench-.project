📱 **Electronics Store** - Система управления интернет-магазином электроники

**Полнофункциональная база данных** для управления интернет-магазином электроники с поддержкой иерархических категорий, управления запасами, обработки заказов и автоматизации бизнес-процессов.

📋 **Содержание**

**Возможности
Структура базы данных
Установка и настройка
Основные таблицы
Триггеры и автоматизация
Хранимые процедуры
Примеры использования
Производительность
Лицензия**

<img width="768" height="325" alt="image" src="https://github.com/user-attachments/assets/08920363-980a-42db-a11b-113679f603dd" />

🗄️ **Структура базы данных**

**Диаграмма схемы:**

<img width="483" height="304" alt="image" src="https://github.com/user-attachments/assets/39271ef2-4c2f-41c8-a8fe-654b920f9a80" />

🚀 **Установка и настройка**

**Требования**

MySQL Server 8.0+ (или 5.7 с ограничениями)
MySQL Workbench или другой клиент
Права на создание базы данных


📊 Основные таблицы

1. **Categories - Категории товаров**
```
CREATE TABLE Categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,           -- Название категории
    parent_category_id INT NULL,          -- Родительская категория
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   
);
```

2. **Products - Товары**
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
3. **Customers - Клиенты**
```
CREATE TABLE Customers (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(255) NOT NULL UNIQUE,   -- Email (уникальный)
    phone VARCHAR(20) NOT NULL UNIQUE,    -- Телефон (уникальный)
    registration_date DATE NOT NULL DEFAULT (CURRENT_DATE),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP

);

```
4. **Orders - Заказы**
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
5. **Order_items - Позиции заказа**
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

                    
⚡ **Триггеры и автоматизация**

Основные триггеры:
after_order_item_insert - **уменьшает остатки при добавлении товара в заказ**
after_order_item_update - **корректирует остатки при изменении заказа**
after_order_item_delete - **возвращает товары при удалении из заказа**
after_order_cancelled - **возвращает все товары при отмене заказа**

Пример работы триггера:
```
-- При добавлении товара в заказ автоматически:
-- 1. Проверяется доступное количество
-- 2. Уменьшается stock_quantity
-- 3. Обновляется total_amount заказа
```

📞 **Поддержка**

Для вопросов и поддержки:

Напишите на pmdworking@yandex.ru

📄 **Лицензия**
Этот проект распространяется под лицензией MIT. См. файл LICENSE для получения дополнительной информации.
