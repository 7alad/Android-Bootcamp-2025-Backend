# Android-Bootcamp-2025-Backend
## Команда: inverse 

Разработать программный продукт для помощи в осуществлении волонтерской деятельности в клиент-серверной архитектуре.

Клиент - Android Приложение, реализованное на java или kotlin с адаптивной версткой.
Мобильное приложение должно предоставлять удобный и интуитивно понятный интерфейс для волонтеров, в котором можно зарегистрироваться в базу волонтеров, просмотреть ближайшие волонтерские центры, отредактировать собственный профиль и посмотреть активных волонтеров в конкретном центре. В случае авторизации с правами администратора, пользователь приложения должен иметь возможность просмотреть список не занятых волонтеров и их контактные данные.

Сервер - микросервис на Spring Boot, работающий с In-memory базой данных H2.
Серверное приложение должно предоставлять API необходимый для работы мобильного приложения. Осуществлять все основные манипуляции с данными в СУБД при помощи Spring Data JPA. Данные о всех зарегистрированных волонтерских центрах должны быть предзаполнены в БД при запуске приложения.Создание схемы БД и предзаполнение данными необходимо реализовать с использованием liquibase.

# **User stories**

* ## **Регистрация / авторизация**

  * Как волонтер, я хочу иметь возможность зарегистрироваться, чтобы иметь доступ к приложению  
  * Как волонтер, я хочу иметь возможность войти в аккаунт при первом запуске, если я уже был зарегистрирован, чтобы продолжить пользоваться своим аккаунтом  
  * Как волонтер, я не хочу вводить данные повторно после авторизации, чтобы не тратить время на ввод данных

* ## **Просмотр центров**

  * Как волонтер, я хочу видеть список ближайших доступных центров, чтобы понять, где рядом может потребоваться моя помощь  
  * Как волонтер, я хочу видеть список волонтеров, принимающих участие в конкретном центре, чтобы узнать, есть ли там мои друзья

* ## **Редактирование профиля**

  * Как волонтер, я хочу редактировать свой профиль, чтобы другие волонтеры могли легко меня узнавать  
  * Как администратор, я хочу иметь возможность модерировать профили волонтеров, чтобы избежать неприемлемого контента на платформе

* ## **Администрирование центров**

  * Как администратор, я хочу иметь возможность добавления центра, чтобы волонтеры могли принять участие в моих мероприятиях  
  * Как администратор, я хочу иметь возможность редактировать информацию о центре, чтобы волонтеры имели актуальную информацию о нем

* ## **Администрирование волонтеров**

  * Как администратор, я хочу видеть список незанятых волонтеров, а также их контактные данные, чтобы связаться с ними по поводу участия в мероприятиях  
  * Как администратор, я хочу иметь возможность изменять статус занятости волонтера, а также прикреплять его к определенному центру и откреплять от него, чтобы в центрах был актуальный список волонтеров и сами волонтеры могли видеть прикрепленный к ним центр

## 

* ## **Безопасность**

  * Как волонтер, я хочу быть уверенным в конфиденциальности вводимых мною при регистрации данных, чтобы избежать утечки персональных данных  
  * Как администратор, я хочу быть уверенным в настройках безопасности приложения, чтобы избежать неавторизованного доступа

--

# **Use Cases**

Прим.: *Здесь и далее пользователь – администратор / волонтер*

# **Запуск приложения / сервера**

## **AutoCompleteDatabaseUseCase**

* Цель: автозаполнение данных в базе на основе внешнего источника или шаблонов  
* Акторы: система (сервер)  
* Предусловие: источник данных доступен, таблица существует  
* Триггер: запрос на автозаполнение данных  
* Основной сценарий: получен запрос с указанием источника данных, таблицы и критериев, система проверяет данные на корректность (существование таблицы, доступность источника), если все данные верны, система заполняет таблицу, система отправляет подтверждение об успешном завершении автозаполнения  
* Альтернативный сценарий:  
* Ошибка данных: некорректные данные (ошибка в таблице или источнике) — система возвращает ошибку  
* Ошибка сервера: проблемы на сервере — система сообщает об ошибке на сервере

## **LaunchApiUseCase**

* Цель: инициализировать необходимые репозитории, а также API  
* Акторы: клиент (приложение)  
* Предусловие: приложение установлено корректно  
* Триггер: запуск приложения  
* Основной сценарий: отправка запроса на сервер на получение репозиториев, инициализация API и основных сущностей, запуск приложения  
* Альтернативный сценарий:   
  * приложение было запущено корректно, остановка приложения  
  * отсутствует подключение к интернету, сообщаем пользователю, ожидание подключения

# **Регистрация / Авторизация**

## **RegisterNewAccountUseCase** {#registernewaccountusecase}

* Цель: новый аккаунт добавлен в БД на сервере  
* Акторы: пользователь  
* Предусловие: аккаунт не был зарегистрирован / предыдущая сессия была завершена  
* Триггер: открытие приложения  
* Основной сценарий: пользователь указал данные для регистрации (почта / логин / номер телефона и пароль), сервер подтвердил регистрацию, запуск [AuthorizeUseCase](#authorizeaccountusecase)  
* Альтернативный сценарий:  
  * пользователь ввел некорректные данные, сервер ответил ошибкой, ошибка отображается на экране, пользователь не допущен к использованию приложения  
  * пользователь выбрал опцию войти, запуск [LoginAccountUseCase](#loginaccountusecase)

## **LoginAccountUseCase** {#loginaccountusecase}

* Цель: получить доступ к приложению, используя существующую учетную запись  
* Акторы: пользователь  
* Предусловие: учетная запись была ранее создана  
* Триггер: нажатие кнопки войти в [RegisterNewAccountUseCase](#registernewaccountusecase) / неудачный вход при попытке использования сохраненных данных  
* Основной сценарий: пользователь указал данные для входа (почта / логин / номер телефона и пароль), сервер подтвердил вход, пользователь авторизован  
* Альтернативный сценарий:   
  * пользователь ввел некорректные данные, сервер ответил ошибку, ошибка отображается на экране, пользователь не авторизован  
  * пользователь выбрал опцию войти, запуск [RegisterNewAccountUseCase](#registernewaccountusecase)

## 

## **AuthorizeAccountUseCase** {#authorizeaccountusecase}

* Цель: пользователь авторизован по сохраненным данным для входа  
* Акторы: пользователь  
* Предусловие: нет  
* Триггер: при каждом запуске приложения  
* Основной сценарий: сохраненные данные отправлены на сервер, сервер подтвердил вход, пользователь авторизован  
* Альтернативный сценарий:   
  * сохраненные данные более невалидны (аккаунт не существует, пароль был изменен), запуск [LoginAccountUseCase](#loginaccountusecase), с отправкой сообщения пользователю о том, что данные более не валидны  
  * Нет доступа к интернету, отображаем ошибку

# **Просмотр центров**

## **GetNearestAvailableCentersUseCase**

* Цель: просмотреть ближайшие активные центры  
* Акторы: пользователь  
* Предусловия: пользователь авторизован  
* Триггер: пользователь открыл экран с отображением ближайших доступных центров  
* Основной сценарий: клиент (приложение) отправило запрос на получение ближайших доступных центров, сервер ответил успешно, данные отобразились на экране  
* Альтернативный сценарий: произошла ошибка во время отправки / получения данных, данные не отобразились на экране, на экране вывелась ошибка

## **GetVolunteersListInCenterUseCase**

* Цель: просмотреть активных волонтеров в выбранном центре  
* Акторы: пользователь  
* Предусловия: пользователь должен быть авторизован  
* Триггер: нажатие на кнопку центра в списке ближайших доступных центров  
* Основной сценарий: клиент (приложение) отправило запрос на получение активных волонтеров в конкретном центре, сервер ответил успешно, данные отобразились на экране  
* Альтернативный сценарий: произошла ошибка во время отправки / получения данных, данные не отобразились на экране, на экране вывелась ошибка

# 

# **Администрирование волонтеров**

## **GetListOfUnemployedVolunteersUseCase**

* Цель: получить список незанятых волонтеров и их контактных данных  
  Акторы: администратор  
* Предусловия: пользователь должен быть авторизован с правами администратора  
* Триггер: пользователь (администратор) выбрал соответствующую опцию в приложении  
* Основной сценарий: пользователь (администратор) запрашивает список незанятых волонтеров и их контактные данные, сервер отвечает успешно, отображаем информацию  
* Альтернативный сценарий: во время отправки / получения данных произошла ошибка, сервер ответил ошибкой, не отображаем информацию, отображаем сообщение об ошибке

## **SetVolunteerCenterUseCase**

* Цель: прикрепить волонтера к центру или открепить от него  
  Акторы: администратор, волонтер  
* Предусловия: пользователь должен быть авторизован с правами администратора  
* Триггер: пользователь (администратор) выбрал соответствующую опцию в приложении  
* Основной сценарий: пользователь (администратор) после согласования с волонтером прикрепляет его к выбранному центру, отправляет запрос на сервер, сервер ответил успешно, волонтер видит у себя прикрепленный центр, а другие пользователи видят его в списке активных волонтеров выбранного центра

# Альтернативный сценарий: во время отправки / получения данных произошла ошибка, сервер ответил ошибкой, не изменяем статус занятости, отображаем сообщение об ошибке

# **Редактирование профиля**

## **UpdateAccountProfileUseCase**

* Цель: отредактировать профиль пользователя  
* Акторы: пользователь  
* Предусловия: пользователь должен быть авторизован  
* Триггер: пользователь нажал кнопку редактировать профиль на экране просмотра профиля  
* Основной сценарий: пользователь ввел корректные данные, данные отправлены на сервер для дальнейшей модерации, отображаем сообщение об успешной отправке данных, а также о статусе заявки

* ## Альтернативный сценарий: пользователь ввел невалидные данные, данные не отправляются на сервер, отображаем сообщение об ошибке {#альтернативный-сценарий:-пользователь-ввел-невалидные-данные,-данные-не-отправляются-на-сервер,-отображаем-сообщение-об-ошибке}

##  **FetchUpdatedProfilesUseCase**

* Цель: получить все заявки на изменение профиля  
* Акторы: администратор  
* Предусловия: пользователь должен быть авторизован с правами администратора  
* Триггер: переход на экран модерации  
* Основной сценарий: пользователь (администратор) запрашивает список аккаунтов с измененными профилями, сервер отвечает успешно, пользователь получает возможность модерировать и фиксировать изменения  
* Альтернативный сценарий: во время отправки / получения данных произошла ошибка, не отображаем данные, вводим ошибку на экран

## 

## **CommitOrDenyChangedProfileUseCase**

* Цель: одобрить или отклонить заявку на изменение профиля  
  Акторы: администратор, волонтер  
* Предусловия: пользователь должен быть авторизован с правами администратора, аккаунт с измененным профилем должен быть среди аккаунтов из [FetchUpdatedProfilesUseCase](#альтернативный-сценарий:-пользователь-ввел-невалидные-данные,-данные-не-отправляются-на-сервер,-отображаем-сообщение-об-ошибке)  
* Триггер: пользователь (администратор) после проверки решил, что данный профиль НЕ удовлетворяет регламенту платформу и не принимает изменения  
* Основной сценарий: пользователь (администратор) отправляет запрос на сервер на изменение профиля данного аккаунта , сервер отвечает успешно, профиль данного аккаунта изменен, меняем статус заявки на *“accepted”*  
* Альтернативный сценарий:   
  * пользователь (администратор) отправляет запрос на сервер на отказ для изменений профиля данного аккаунта, сервер отвечает успешно, профиль данного аккаунта изменен, меняем статус заявки на *“denied”*  
  * Альтернативный сценарий: во время выполнения отправки / подтверждения запросы произошла ошибка, не изменяем профиль, выводим сообщение об ошибке

# 

# **Администрирование центров**

## **AddCenterUseCase**

* Цель: добавить новый центр  
  Акторы: администратор  
* Предусловия: пользователь должен быть авторизован с правами администратора  
* Триггер: пользователь (администратор) выбрал соответствующий пункт в меню  
* Основной сценарий: пользователь (администратор) вводит корректные данные для регистрации нового центра, сервер отвечает успешно, центр добавлен и отображается у волонтеров  
* Альтернативный сценарий: пользователь (администратор) вводит некорректные данные для регистрации нового центра, сервер отвечает ошибкой, выводим на экран ошибку, центр не создается и не отображается у волонтеров

## **UpdateCenterInfoUseCase**

* Цель: обновить информацию о центре  
  Акторы: администратор  
* Предусловия: пользователь должен быть авторизован с правами администратора  
* Триггер: пользователь (администратор) выбрал опцию редактировать в меню информации о центре  
* Основной сценарий: пользователь (администратор) вводит корректные данные, сервер отвечает успешно, данные обновлены, обновленный центр виден всем волонтерам  
* Альтернативный сценарий: пользователь (администратор) вводит некорректные данные, сервер отвечает ошибкой, данные не будут обновлены, волонтеры продолжат видеть старые данные, выводим пользователю (администратору) сообщение об ошибке

--

# **Backend Tasks**

## **RegisterNewAccountUseCase** {#registernewaccountusecase}

- [ ] Реализовать API для регистрации нового пользователя  
- [ ] Валидация данных пользователя (проверка корректности email, номера телефона, пароля)  
- [ ] Реализовать сохранение данных в БД (таблица пользователей).  
- [ ] Реализовать отправку подтверждения на сервер после успешной регистрации  
- [ ] Обработать ошибки (например, если email или телефон уже зарегистрированы)  
- [ ] Обеспечить запуск процесса авторизации после успешной регистрации (вызов [AuthorizeUseCase](#authorizeaccountusecase))

### **API путь:**

## **POST** /api/users/register

### **Параметры запроса:**

## {

##   "email": "admin@igramagadev.ru",

##   "login": "igram",

##   "phone": "+1234567890",

##   "password": "sdhfy334"

## }

### **Ответ:**

* **200 OK** — Если регистрация успешна  
* **400 Bad Request** — Ошибка валидации данных  
* **500 Internal Server Error** — Ошибка на сервере

## 

## **LoginAccountUseCase**

- [ ] Реализовать API для входа в систему  
- [ ] Валидация введенных данных (проверка корректности и существования аккаунта)  
- [ ] Обработка ошибок в случае некорректных данных (неверный логин / пароль)  
- [ ] Реализовать генерацию и возврат токенов для авторизации (Basic)  
- [ ] Обеспечить поддержку процесса регистрации при неудачном входе (связь с [RegisterNewAccountUseCase](#registernewaccountusecase))

### **API путь:**

## **POST** /api/users/login

### **Параметры запроса:**

## {

##   "email": "admin@igramagadev.ru",

##   "password": "2134cjhfdl"

## }

### **Ответ:**

* **200 OK** — Токен авторизации:

  ## {

  ##   "userId": "1",

  ##   "email": "admin@igramagadev.ru",

  ##   "name": "Igra Maga"

  ## }

* **401 Unauthorized** — Неверные данные для входа  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **AuthorizeAccountUseCase** {#authorizeaccountusecase}

- [ ] Реализовать механизм авторизации при каждом запуске приложения (проверка наличия токенов в запросе)  
- [ ] Реализовать API для проверки валидности сохраненных данных пользователя (например, проверка пароля или данных аккаунта на сервере)  
- [ ] Обработать возможные ошибки (например, если данные больше невалидны)  
- [ ] Реализовать уведомление пользователя о том, что данные невалидны или отсутствует интернет-соединение

### **API путь:**

## **GET** /api/users/authorize

### **Ответ:**

* **200 OK** — Информация о пользователе:

  ## {

  ##   "userId": "1",

  ##   "email": "admin@igramagadev.ru",

  ##   "name": "Igra Maga"

  ## }

* **401 Unauthorized** — Неавторизованный доступ  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **GetNearestAvailableCentersUseCase**

- [ ] Реализовать API для получения ближайших доступных центров  
- [ ] Обеспечить корректную обработку данных и возвращение списка ближайших центров  
- [ ] Обработать ошибки при запросах на сервер и вернуть соответствующие сообщения об ошибках

### **API путь:**

## **GET** /api/centers/nearest

### **Параметры запроса:**

## latitude=55\&longitude=36

### **Ответ:**

* **200 OK** — Список ближайших центров:

  ## \[

  ##   {

  ##     "centerId": "1",

  ##     "name": "Центр 1",

  ##     "address": "Ул. Мира 32"

  ##   },

  ##   {

  ##     "centerId": "2",

  ##     "name": "Центр 2",

  ##     "address": "Ул. Пушкина 214"

  ##   }

  ## \]

* **400 Bad Request** — Некорректные координаты  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **UpdateAccountProfileUseCase**

- [ ] Реализовать API для редактирования профиля пользователя  
- [ ] Валидация введенных данных (например, правильность формата email или телефона)  
- [ ] Реализовать модерацию данных (сохранение изменений в базе данных только после проверки администратором)  
- [ ] Обработать ошибки в случае некорректных данных

### **API путь:**

## **PUT** /api/users/profile

### **Параметры запроса:**

## {

##   "name": "Иванов2",

##   "email": "admin@taichkar.ru",

##   "phone": "+123456"

## }

### **Ответ:**

* **200 OK** — Успешное обновление профиля  
* **400 Bad Request** — Ошибка валидации данных  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **GetVolunteersListInCenterUseCase**

- [ ] Реализовать API для получения списка волонтеров в центре  
- [ ] Обеспечить корректное возвращение списка активных волонтеров в ответе  
- [ ] Обработать ошибки при запросах на сервер и корректно вернуть сообщения об ошибках

### **API путь:**

## **GET** /api/centers/{centerId}/volunteers

### **Параметры запроса:**

## centerId=1

### **Ответ:**

* **200 OK** — Список волонтеров в центре:

  ## \[

  ##   {

  ##     "volunteerId": "1",

  ##     "name": "Иван Иванов",

  ##     "status": "active"

  ##   },

  ##   {

  ##     "volunteerId": "2",

  ##     "name": "Петр Петров",

  ##     "status": "inactive"

  ##   }

  ## \]

* **400 Bad Request** — Некорректный ID центра  
* **404 Not Found** — Центр не найден  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **FetchUpdatedProfilesUseCase**

- [ ] Реализовать API для получения списка профилей, ожидающих модерации  
- [ ] Обработать ошибки, если данные не могут быть получены (например, из\-за проблем с соединением)  
- [ ] Обеспечить правильную фильтрацию и сортировку измененных профилей

### **API путь:**

## **GET** /api/admin/profiles/updated

### **Ответ:**

* **200 OK** — Список профилей на модерации

  ## \[

  ##   {

  ##     "profileId": "1",

  ##     "userName": "Иван Иванов",

  ##     "status": "pending"

  ##   },

  ##   {

  ##     "profileId": "2",

  ##     "userName": "Михаил Петров",

  ##     "status": "pending"

  ##   }

  ## \]

* **500 Internal Server Error** — Ошибка на сервере

### 

## **CommitOrDenyChangedProfileUseCase**

- [ ] Реализовать API для принятия или отклонения изменений в профиле  
- [ ] Обработать логику изменения профиля (обновить профиль в базе данных)  
- [ ] Обеспечить обновление статуса заявки в базе данных  
- [ ] Обработать ошибки в случае сбоя отправки/подтверждения запроса

### **API путь:**

## **POST** /api/admin/profiles/{profileId}/update

### **Параметры запроса:**

## {

##   "status": "accepted" // или "denied"

## }

### **Ответ:**

* **200 OK** — Статус профиля обновлен:

  ## {

  ##   "message": "Статус профиля успешно обновлен"

  ## }

* **400 Bad Request** — Некорректный статус или профиль  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **AddCenterUseCase**

- [ ] Реализовать API для добавления нового центра  
- [ ] Валидация данных для добавления центра  
- [ ] Реализовать добавление центра в базу данных  
- [ ] Обработать ошибки, если данные некорректны

### **API путь:**

## **POST** /api/admin/centers

### **Параметры запроса:**

## {

##   "name": "Центер 2",

##   "address": "Гагарина 120",

##   "contact\_info": "+89551007575"

## }

### **Ответ:**

* **200 OK** — Центр успешно добавлен:

  ## {

  ##   "message": "Успешно добавлен центр."

  ## }

* **400 Bad Request** — Ошибка валидации данных  
* **500 Internal Server Error** — Ошибка на сервере

### 

## **UpdateCenterInfoUseCase**

- [ ] Реализовать API для обновления информации о центре  
- [ ] Обработать валидацию и сохранение новых данных в базе данных  
- [ ] Обработать ошибки в случае некорректных данных

### **API путь:**

## **PUT** /api/admin/centers/{centerId}

### **Параметры запроса:**

## {

##   "name": "Новое название центра 2",

##   "address": "Адрес новый",

##   "contact\_info": "+12345679"

## }

### **Ответ:**

* **200 OK** — Центр обновлен  
* **400 Bad Request** — Некорректные данные  
* **500 Internal Server Error** — Ошибка на сервере

## 

## **GetListOfUnemployedVolunteersUseCase**

- [ ] Реализовать API для получения списка незанятых волонтеров  
- [ ] Реализовать обработку ошибок при запросах

### **API путь:**

## **GET** /api/admin/volunteers/unemployed

### **Ответ:**

* **200 OK** — Список незанятых волонтеров:

  ## \[

  ##   {

  ##     "volunteerId": "1",

  ##     "name": “Иван иванов”,

  ##     "status": "unemployed"

  ##   },

  ##   {

  ##     "volunteerId": "2",

  ##     "name": "Петр Петров",

  ##     "status": "unemployed"

  ##   }

  ## \]

* **500 Internal Server Error** — Ошибка на сервере


## 

## **AutoCompleteDatabaseUseCase**

- [ ] Реализовать API для автозаполнения БД (Базы данных) на основе внешнего источника или шаблонов

### **API путь:**

## **GET** /api/database/autofill

### **Параметры запроса:**

## {

##   "source": "External",

##   "table": "users",

##   "criteria": "some"

## }

### **Ответ:**

* **200 OK** — Автозаполнение успешно завершено

  ##   {

  ##     "message": "Автозаполнение данных завершено.",

  ##   }

* **400 Bad Request** — Ошибка валидации или данных источника

  ##   {

  ##     "message": "Ошибка автозаполнения.",

  ##   }

* **500 Internal Server Error** — Ошибка на сервере

  ##   {

  ##     "message": "Ошибка на сервере.",

  ##   }

## 

## **SetVolunteersCenterUseCase**

- [ ] Реализовать сеттер для добавления или открепления пользователя от центра

### **API путь:**

## **PUT** /api/users/profile

### **Параметры запроса:**

{  
  "centerId": "None" //Или айди центра  
}

### **Ответ:**

* **200 OK** — Центр успешно установлен

  ##   {

  ##     "message": "Центр успешно установлен.",

  ##   }

* **400 Bad Request** — Ошибка валидации или данных источника

  ##   {

  ##     "message": "Ошибка установки центра.",

  ##   }

* **500 Internal Server Error** — Ошибка на сервере

  ##   {

  ##     "message": "Ошибка на сервере.",

  ##   }