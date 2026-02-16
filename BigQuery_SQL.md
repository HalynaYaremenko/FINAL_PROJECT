## Завдання 1: Знайомство з користувачами

### Виведіть список усіх користувачів (таблиця users) із країни 'Brasil' (Бразилія), які зареєструвалися у 2023 році. Виведіть їхнє ім'я (first_name), прізвище (last_name) та електронну пошту.

```
SELECT  first_name, 
        last_name, 
        email
FROM `bigquery-public-data.thelook_ecommerce.users` 
WHERE country = 'Brasil' 
      AND EXTRACT(YEAR FROM created_at) = 2023;
``` 

![alt text](Screens/SQL_1.png)



## Завдання 2: Топ-категорії товарів

### Порахуйте, скільки всього товарів представлено в кожній категорії (таблиця products). Відсортуйте результат за кількістю товарів — від найбільшої до найменшої.

```
SELECT category,
       COUNT(*) AS count_products
FROM `bigquery-public-data.thelook_ecommerce.products` 
GROUP BY category
ORDER BY count_products DESC;
```
![alt text](Screens/SQL_2.png)



## Завдання 3: Аналіз замовлень (JOIN)

### Об'єднайте таблиці замовлень (orders) та користувачів (users). Виведіть order_id (ID замовлення), first_name та last_name клієнта, а також статус замовлення для всіх замовлень, які мають статус 'Shipped' (Відправлено).
```
SELECT  orders.order_id, 
        users.first_name, 
        users.last_name, 
        orders.status
FROM `bigquery-public-data.thelook_ecommerce.users` AS users
JOIN `bigquery-public-data.thelook_ecommerce.orders` AS orders ON users.id = orders.user_id
WHERE orders.status = 'Shipped'
ORDER BY orders.order_id;
```
![alt text](Screens/SQL_3.png)



## Завдання 4: Фінанси — найдорожчі замовлення

### Знайдіть 10 найдорожчих замовлень (таблиця order_items). Виведіть order_id, user_id та ціну продажу (sale_price). Відсортуйте за ціною у спадному порядку.
```
SELECT  order_id, 
        user_id, 
        ROUND(SUM(sale_price),2) AS total_order_value     
FROM `bigquery-public-data.thelook_ecommerce.order_items` 
GROUP BY order_id, user_id
ORDER BY total_order_value DESC
LIMIT 10;
```
![alt text](Screens/SQL_4.png)



## Завдання 5: Географія покупців

## Порахуйте кількість унікальних користувачів (таблиця users) у кожній країні. Виведіть тільки ті країни, де кількість користувачів перевищує 500.
```
SELECT  country,
        COUNT(DISTINCT id) AS unique_users
FROM `bigquery-public-data.thelook_ecommerce.users` 
GROUP BY country
HAVING COUNT(DISTINCT id) > 500
ORDER BY unique_users DESC;
```
![alt text](Screens/SQL_5.png)