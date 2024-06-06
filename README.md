# Техническое задание №3
**Проектировка приложения доставки еды**

## Описание системы

Приложение для доставки еды предназначено для связи клиентов с ресторанами и курьерами через внешние системы управления. Клиенты могут выбирать блюда из меню различных ресторанов, оформлять заказы и отслеживать их статус в реальном времени.  
Основные функции приложения включают регистрацию и вход пользователей, просмотр меню и добавление блюд в заказ, оформление и остлеживание заказа.  
Курьеры напрямую с системой не взаимодействуют, присутствует курьерская система, являющаяся промежуточным звеном между ними и отправляющая их на выполнение заказа.  
Рестораны могут создавать свои меню, работать с заказами через интерфейс системы.

### Пример работы приложения

**1.** Клиент регистрируется, указывая свое имя, телефон, адрес, почту и пароль.  
*В это время формируется и ID пользователя в приложении.*

**2.** Клиент входит в приложение, указав почту и пароль.

**3.** Пользователь просматривает меню различных ресторанов и добавляет блюда в заказ.

**4.** Клиент оформляет заказ, указывая адрес и переводя деньги за него.  
*Статус заказа: оплачен (paid).*

**5.** Заказ отправляется в систему ресторана, где подтверждается и готовится.  
*Статус заказа: подтвержден (confirmed).*

**6.** В курьерскую систему направляется запрос на доставщика, она назначает одного из свободных (ближайшего по адресу).  
*Статус заказа: отправлен (shipped).*

**7.** Курьер доставляет заказ, меняя его статус через систему курьеров на выполенный.  
*Статус заказа: выполнен (completed).*

## Описание диаграмм UML

### Диаграмма вариантов использования

Здесь приведены некоторые варианты использования системы.

![Use Cases](https://github.com/Kaldazar/tp3/blob/master/PNG-files/useCasesDiagram.png?raw=true)

В качестве акторов выбраны клиент, система ресторана и система курьеров.  
Клиент может зарегистрироваться, просматривать меню ресторанов, сделать заказ, оплатить его и отслеживать его статус.  
Система ресторанов подтверждает заказ и обновляет статус.
Курьерская система назначает курьера на заказ и обновляет статус.

### Диаграмма последовательности

Описание всего процесса заказа еды.

![Sequence](https://github.com/Kaldazar/tp3/blob/master/PNG-files/sequenceDiagram.png?raw=true)

Клиент открывает приложение, которое запрашивает меню у ресторанной системы. По получении приложение отображает клиенту меню.  
Пользователь может добавить блюдо в заказ (необязательно один раз).  
Клиент оформляет заказ, после чего приложение отправляет его в систему ресторана. Ресторан подтверждает заказ. После этого и сборки заказа приложение запрашивает курьера у соответствующей системы, а та назначает подходящего.  
После доставки заказа к клиенту система курьеров посылает в приложение статус "выполнен".  
Все это время с оформления заказа, его статус менялся в зависимости от этапа и отображался клиенту динамически.

### Диаграмма состояний

Показывает статусы заказа.

![State](https://github.com/Kaldazar/tp3/blob/master/PNG-files/stateDiagram.png?raw=true)

После оформления заказ приобретает статус "paid" (оплачен).  
Далее заказ либо получает статус "confirmed" после подтверждения ресторана.  
После приготовления и сборки заказа его статус становится "assembled" (собран).  
Далее назначается курьер, и заказ переходит в состояние "shipped" (отправлен).  
После передачи клиенту, он приобретает статус "completed" (выполнен).  
После этого заказ завершается.  
Если заказ был отменен на одном из этапов, то он сразу завершается.  
*На всех этапах с "paid" по "shipped" заказ может быть отменен покупателем и на этапе "paid" рестораном.*

### Диаграмма деятельности

Обработка заказа системой

![Activity](https://github.com/Kaldazar/tp3/blob/master/PNG-files/activityDiagram.png?raw=true)

По оформлении заказа приложение отправляет запрос на подтверждение у системы ресторана.  
Если ресторан <ins>подтверждает</ins> заказ, то приложение запрашивает курьера у системы. Эта структура назначает доставщика и меняет статус заказа. Потом (уже после доставки) устанавливается финальный статус (выполнен), и заказ завершается.  
Если ресторан <ins>отменяет</ins> заказ, то статус меняется и устанавливается финальный (отменен), после чего заказ считается завершенным.  

### Диаграмма классов

Описание модулей системы и связей между ними.

![Class](https://github.com/Kaldazar/tp3/blob/master/PNG-files/classDiagram.png?raw=true)

Здесь представлены классы системы:
* Client. Класс клиента, описывающий его самого и возможные взаимодействия с ним.
* Order. Класс заказа и возможных методов работы с ним.
* RestaurantSystem. Описывает систему ресторанов и ее возможности в рамках приложения.
* Dish. Класс блюда, его характеристики и возможные действия.
* CourierSystem. Класс системы управления курьерами, осуществляющий управление собственно курьерами.
* Courier. *Вспомогательный* класс, описывающий необходимые для корректной работы системы характеристики и методы взаимодействия с курьерами, например, указание доступности и адреса.
* Status. Перечисление возможных статусов заказа.

## Автор

Гайфутдинов Данияр (НИУ ВШЭ, Бизнес-информатика, 1ый курс, ББИ232)
> Ссылка на Telegram: [@deathrattle94](https://t.me/deathrattle94)  
> Корпоративная почта: drgayfutdinov@edu.hse.ru