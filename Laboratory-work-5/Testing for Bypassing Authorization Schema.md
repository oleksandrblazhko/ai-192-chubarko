# Тестування на обхід схеми авторизації

## Короткий опис
Цей тип перевірки зосереджується на перевірці того, як реалізовано схему авторизації для кожної ролі чи привілею для отримання доступу до зарезервованих функцій і ресурсів.
Для кожної конкретної ролі, яку виконує тестувальник під час оцінювання, і для кожної функції та запиту, які програма виконує на етапі пост-автентифікації, необхідно перевірити:
- Чи можливий доступ до цього ресурсу, навіть якщо користувач не автентифікований?
- Чи можливий доступ до цього ресурсу після виходу з системи?
- Чи можна отримати доступ до функцій і ресурсів, які повинні бути доступні для користувача, який має іншу роль або привілеї?

Спробуйте отримати доступ до програми як адміністратор і відслідковуйте всі адміністративні функції.
- Чи можливий доступ до адміністративних функцій, якщо тестувальник увійшов як користувач без прав адміністратора?
- Чи можна використовувати ці адміністративні функції як користувач з іншою роллю та кому цю дію слід заборонити?

## Цілі тесту
- Оцініть, чи можливий горизонтальний або вертикальний доступ.

## Як тестувати
- Доступ до ресурсів і виконання операцій горизонтально.
- Отримайте доступ до ресурсів і виконуйте операції вертикально.

### Тестування схеми авторизації горизонтального обходу
Для кожної функції, конкретної ролі або запиту, які виконує програма, необхідно перевірити:
- Чи можна отримати доступ до ресурсів, які повинні бути доступні для користувача, який має іншу особу з тією самою роллю чи привілеєм?
- Чи можна керувати функціями на ресурсах, які мають бути доступні для користувача з іншим ідентифікатором?

Для кожної ролі:
1. Зареєструйте або згенеруйте двох користувачів з однаковими привілеями.
2. Встановіть і зберігайте активними два різні сеанси (по одному для кожного користувача).
3. Для кожного запиту змініть відповідні параметри та ідентифікатор сеансу з маркера один на маркер два та діагностуйте відповіді для кожного маркера.
4. Програма вважатиметься вразливою, якщо відповіді однакові, містять однакові особисті дані або вказують на успішну операцію на ресурсі чи даних інших користувачів.

Наприклад, припустімо, що функція ```viewSettings``` є частиною кожного меню облікового запису програми з тією самою роллю, і до неї можна отримати доступ, запитавши таку URL-адресу: ```https://www.example.com/account/viewSettings```. Потім під час виклику функції ```viewSettings``` генерується наступний HTTP-запит:
	
	POST /account/viewSettings HTTP/1.1
	Host: www.example.com
	[other HTTP headers]
	Cookie: SessionID=USER_SESSION
	username=example_user
	
Правильна та законна відповідь:
	
	HTTP1.1 200 OK
	[other HTTP headers]

	{
	  "username": "example_user",
	  "email": "example@email.com",
	  "address": "Example Address"
	}
	
Зловмисник може спробувати виконати цей запит із тим самим параметром ```username```:

	POST /account/viewCCpincode HTTP/1.1
	Host: www.example.com
	[other HTTP headers]
	Cookie: SessionID=ATTACKER_SESSION
	username=example_user

Якщо відповідь зловмисника містить дані ```example_user```, то програма є вразливою для атак з боковим рухом, коли користувач може читати або записувати дані іншого користувача.

### Тестування схеми авторизації вертикального обходу
Вертикальний обхід авторизації є специфічним для випадку, коли зловмисник отримує роль, вищу за свою. Тестування цього обходу зосереджено на перевірці того, як схема вертикальної авторизації реалізована для кожної ролі. Для кожної функції, сторінки, конкретної ролі або запиту, які виконує програма, необхідно перевірити, чи можливо:
- Доступ до ресурсів, які мають бути доступні лише для користувача з вищою роллю.
- Виконуйте функції на ресурсах, якими повинен керувати лише користувач, який має вищу або певну роль.

Для кожної ролі:
1. Зареєструвати користувача.
2. Створіть і підтримуйте два різних сеанси на основі двох різних ролей.
3. Для кожного запиту змініть ідентифікатор сеансу з оригінального на ідентифікатор сеансу іншої ролі та оцініть відповіді для кожного.
4. Програма вважатиметься вразливою, якщо слабший привілейований сеанс містить ті самі дані або вказує на успішні операції з функціями з вищими привілейами.

### Сценарій ролей банківського сайту
У наведеній нижче таблиці показано системні ролі на банківському сайті.Кожна роль пов’язана з певними дозволами для функцій меню подій:

| Роль | Дозвіл | Додаткові дозволи |
|------|--------|-------------------|
| Адміністратор | Повний доступ | Видалення |
| Менеджер | Модифікування, читання, додавання | Додавання |
| Персонал | Читання, модифікування | Модифікування |
| Клієнти | Лишення читання |  |

Програма вважатиметься вразливою, якщо:
1. Клієнт може виконувати функції адміністратора, менеджера або персоналу;
2. Штатний користувач може керувати функціями менеджера або адміністратора;
3. Менеджер міг виконувати функції адміністратора.

Припустімо, що функція ```deleteEvent``` є частиною меню облікового запису адміністратора програми, і до неї можна отримати доступ, запитавши таку URL-адресу: ```https://www.example.com/account/deleteEvent```. Потім під час виклику функції ```deleteEvent``` генерується наступний HTTP-запит:
	
	POST /account/deleteEvent HTTP/1.1
	Host: www.example.com
	[other HTTP headers]
	Cookie: SessionID=ADMINISTRATOR_USER_SESSION
	EventID=1000001

Правильна відповідь:

	HTTP/1.1 200 OK
	[other HTTP headers]
	{"message": "Event was deleted"}

Зловмисник може спробувати виконати такий самий запит:

	POST /account/deleteEvent HTTP/1.1
	Host: www.example.com
	[other HTTP headers]
	Cookie: SessionID=CUSTOMER_USER_SESSION
	EventID=1000002
	
Якщо відповідь на запит зловмисника містить ті самі дані ```{"повідомлення": "Подія видалена"}```, програма є вразливою.

### Доступ до сторінки адміністратора
Припустимо, що меню адміністратора є частиною облікового запису адміністратора.

Програма вважатиметься вразливою, якщо будь-яка особа, крім адміністратора, матиме доступ до меню адміністратора. Іноді розробники виконують перевірку авторизації лише на рівні графічного інтерфейсу користувача та залишають функції без перевірки авторизації, що потенційно може призвести до вразливості.

### Тестування доступу до адміністративних функцій
Наприклад, припустимо, що функція ```addUser``` є частиною адміністративного меню програми, і до неї можна отримати доступ, запитавши таку URL-адресу ```https://www.example.com/admin/addUser```.
Потім під час виклику функції ```addUser``` генерується такий HTTP-запит:

	POST /admin/addUser HTTP/1.1
	Host: www.example.com
	[...]
	userID=fakeuser&role=3&group=grp001

Подальші запитання чи міркування стосуватимуться такого напряму:
- Що станеться, якщо користувач без прав адміністратора спробує виконати цей запит?
- Чи буде створений користувач?
- Якщо так, чи може новий користувач використовувати свої привілеї?

### Перевірка доступу до ресурсів, призначених іншій ролі
Різні програми налаштовують засоби керування ресурсами на основі ролей користувачів. Давайте візьмемо приклад резюме або CV (біографії), завантажених у форму кар’єри в сегмент S3.
Як звичайний користувач, спробуйте отримати доступ до розташування цих файлів. Якщо ви можете отримати їх, змінити або видалити, програма вразлива.
Тестування обробки заголовка спеціального запиту
Деякі програми підтримують нестандартні заголовки, такі як ```X-Original-URL``` або ```X-Rewrite-URL```, щоб дозволити замінити цільову URL-адресу в запитах на URL-адресу, указану в значенні заголовка.
Цю поведінку можна використовувати в ситуації, коли програма стоїть за компонентом, який застосовує обмеження контролю доступу на основі URL-адреси запиту.
Типом обмеження контролю доступу на основі URL-адреси запиту може бути, наприклад, блокування доступу з Інтернету до консолі адміністрування, відкритої в ```/console``` або ```/admin```.Щоб виявити підтримку заголовка ```X-Original-URL``` або ```X-Rewrite-URL```, можна застосувати такі кроки.
1. ### Надішліть звичайний запит без заголовка X-Original-Url або X-Rewrite-Url
		GET / HTTP/1.1
		Хост: www.example.com
		[...]

2. ### Надішліть запит із заголовком X-Original-Url, який вказує на неіснуючий ресурс
		GET / HTTP/1.1
		Хост: www.example.com
		X-Original-URL: /donotexist1
		[...]

3. ### Надішліть запит із заголовком X-Rewrite-Url, який вказує на неіснуючий ресурс
		GET / HTTP/1.1
		Хост: www.example.com
		X-Rewrite-URL: /donotexist2
		[...]

Якщо відповідь на будь-який із запитів містить маркери того, що ресурс не знайдено, це означає, що програма підтримує заголовки спеціальних запитів. Ці маркери можуть містити код статусу відповіді HTTP 404 або повідомлення «ресурс не знайдено» в тілі відповіді.
Після перевірки підтримки заголовка ```X-Original-URL``` або ```X-Rewrite-URL``` можна використати попередній обхід обмеження контролю доступу, надіславши очікуваний запит до програми, але вказавши URL-адресу, «дозволену» на передній панелі -end компонент як основна URL-адреса запиту та вказівка ​​справжньої цільової URL-адреси в заголовку ```X-Original-URL``` або ```X-Rewrite-URL``` залежно від того, який підтримується. Якщо обидва підтримуються, спробуйте один за одним перевірити, для якого заголовка діє обхід.

4. ### Інші заголовки для розгляду
Часто панелі адміністратора або функціональні можливості, пов’язані з адмініструванням, доступні лише клієнтам у локальних мережах, тому для отримання доступу може бути можливим зловживання різними проксі-серверами або заголовками HTTP, пов’язаними з пересиланням. Деякі заголовки та значення для перевірки:
- Заголовки:
	- ```X-Forwarded-For```
	- ```Х-Вперед-За```
	- ```X-Remote-IP```
	- ```X-Originating-IP```
	- ```X-Remote-Addr```
	- ```X-Client-IP```
- Цінності
	- ```127.0.0.1``` (або будь-що в адресному просторі ```127.0.0.0/8``` або ```::1/128```)
	- локальний хост
	- Будь-яка адреса RFC1918:
		- ```10.0.0.0/8```
		- ```172.16.0.0/12```
		- ```192.168.0.0/16```
	- Локальні адреси посилання: ```169.254.0.0/16```

Примітка. Додавання елемента порту разом із адресою чи іменем хоста також може допомогти обійти крайові засоби захисту, такі як брандмауери веб-програм тощо. Наприклад: ```127.0.0.4:80```, ```127.0.0.4:443```, ```127.0.0.4:43982```

### Санація
Застосовуйте принципи найменших привілеїв щодо користувачів, ролей і ресурсів, щоб запобігти несанкціонованому доступу.