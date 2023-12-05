## Крок 1

### Встановіть СКБД PostgreSQL

PostgreSQL вже було встановлено.

## Крок 2

### Створіть термінальну консоль psql через утиліту командного рядка вашої ОС та встановіть з’єднання з БД postgres від імені користувача-адміністратора postgres

Було встановлено з’єднання з БД postgres від імені користувача-адміністратора postgres

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/Screenshot_1.png)

## Крок 3

### Зареєструйте нового користувача в СКБД PostgreSQL, назва якого співпадає з вашим прізвищем, наприклад blazhko, і довільним паролем.

Було створено користувача chubarko з паролем '2468' командою CREATE USER chubarko WITH PASSWORD '2468'. Командою \du перевірено наявність створеного користувача

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 4

### Створіть роль в СКБД PostgreSQL (назва співпадає з вашим прізвищем латинськими літерами) і надайте новому користувачеві можливість наслідувати цю роль.

Роль chubarko було створено автоматично після створення користувача

## Крок 5

### Створіть реляційну таблицю з урахуванням варіанту з таблиці 2.1 від імені користувача-адміністратора.

Було створено таблицю employer командою Create table employer (e_id integer, name varchar, salary integer); . За допомогою \dt було перевірено наявність таблиці

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)


## Крок 6

### Внесіть один рядок в таблицю, використовуючи команду insert into ..., відповідно до варіанту.

Було внесено один рядок до створеної таблиці за допомогою команди Insert into employer values (1, 'Ivanov', 200); Та за допомогою команди SELECT * FROM employer перевірено правильність введення

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 7

### Додатково створіть ще одну термінальну консоль psql та встановіть з’єднання з БД postgres від імені нового користувача.

Було створено ще одну термінальну консоль psql та  встановлено з’єднання з БД postgres від імені користувача chubarko

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 8

### Від імені нового користувача виконайте запит на отримання даних з таблиці (select * from таблиця). Запротоколюйте результат виконання команди.

Командою SELECT * FROM employer; виконано запит на отримання даних з таблиці. Як бачимо, користувач chubarko не має доступу до таблиці.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 9

### Встановіть повноваження на читання таблиці новому користувачеві.

Командою GRANT SELECT ON employer TO chubarko; було надано повноваження.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 10

### Повторіть крок 8.

Оскільки тепер користувач chubarko має доступ, то командою SELECT * FROM employer; було отримано дані з таблиці employer

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 11

### Зніміть повноваження на читання таблиці для нового користувача.

Командою REVOKE SELECT ON employer FROM chubarko; для користувача chubarko знято повноваження на читання таблиці employer.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 12

### Повторіть крок 8.

Як бачимо, доступу до читання таблиці немає

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 13

### Створіть команду оновлення даних таблиці (UPDATE) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

Спробуємо оновити дані в таблиці командою UPDATE employer SET name = 'Bobok' WHERE e_id = 1;. Як бачимо, прав на оновлення у користувача chubarko немає.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 14

### Встановіть повноваження на оновлення таблиці новому користувачу.

Командою GRANT UPDATE ON employer TO chubarko для користувача chubarko надано повноваження на оновлення таблиці employer.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 15

### Повторіть крок 13.


![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 16

### Створіть команду видалення запису таблиці (DELETE) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

Повноваження для видалення даних з таблиці відсутні.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 17

### Встановіть повноваження на видалення таблиці новому користувачеві.

Командою GRANT DELETE ON employer TO chubarko для користувача chubarko надано повноваження на видалення рядків з таблиці employer.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 18

### Повторіть крок 16.

Успішне видалення

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 19

### Зніміть всі повноваження з таблиці для нового користувача.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 20

### Створіть команду внесення запису в таблицю (INSERT) і виконайте її від імені нового користувача. Проаналізуйте результат виконання команди.

Повноваження для додавання даних у таблицю відсутні.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 21

### Встановіть повноваження на внесення даних до таблиці для ролі.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

## Крок 22

### Повторіть крок 20.

Успішне додавання даних

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)

Перевірка за допомогою SELECT * FROM employer

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/laboratory-work-7/Laboratory-work-7/img/####)
