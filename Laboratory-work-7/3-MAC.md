1. Створіть у БД структури даних, необхідні для роботи повноважного керування доступом.

2. Додайте до таблиці з даними стовпчик, який буде зберігати мітки конфіденційності. Визначте для кожного рядка таблиці мітки конфіденційності, які будуть різнитися (для кожного рядка своя мітка).

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/blob/laboratory-work-7/Laboratory-work-7/img/Screenshot_26.png)

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/blob/laboratory-work-7/Laboratory-work-7/img/Screenshot_27.png)

3. Визначте для користувача його рівень доступу.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/blob/laboratory-work-7/Laboratory-work-7/img/Screenshot_28.png)

4. Створіть нову схему даних, назва якої співпадає з назвою користувача.

5. Створіть віртуальну таблицю, назва якої співпадає з назвою реальної таблиці та яка забезпечує SELECT-правила повноважного керування доступом для користувача.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/blob/laboratory-work-7/Laboratory-work-7/img/Screenshot_29.png)

6. Створіть INSERT/UPDATE/DELETE-правила повноважного керування доступом для користувача.

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/blob/laboratory-work-7/Laboratory-work-7/img/Screenshot_30.png)

7. Встановіть з’єднання з СКБД від імені нового користувача.

8. Від імені нового користувача перевірте роботу механізму повноважного керування, виконавши операції SELECT, INSERT, UPDATE, DELETE

![image](https://github.com/oleksandrblazhko/ai-192-chubarko/blob/laboratory-work-7/Laboratory-work-7/img/Screenshot_31.png)
