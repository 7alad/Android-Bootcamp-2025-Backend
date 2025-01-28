# Android-Bootcamp-2025-Backend

## Команда : MindCraft

Разработать программный продукт для помощи в осуществлении волонтерской деятельности в клиент-серверной архитектуре.

Клиент - Android Приложение, реализованное на java или kotlin с адаптивной версткой.
Мобильное приложение должно предоставлять удобный и интуитивно понятный интерфейс для волонтеров, в котором можно зарегистрироваться в базу волонтеров, просмотреть ближайшие волонтерские центры, отредактировать собственный профиль и посмотреть активных волонтеров в конкретном центре. В случае авторизации с правами администратора, пользователь приложения должен иметь возможность просмотреть список не занятых волонтеров и их контактные данные.

Сервер - микросервис на Spring Boot, работающий с In-memory базой данных H2.
Серверное приложение должно предоставлять API необходимый для работы мобильного приложения. Осуществлять все основные манипуляции с данными в СУБД при помощи Spring Data JPA. Данные о всех зарегистрированных волонтерских центрах должны быть предзаполнены в БД при запуске приложения.Создание схемы БД и предзаполнение данными необходимо реализовать с использованием liquibase.

# User Stories 

    Общие:
        1. Как пользователь(Админ), я хочу авторизоваться в системе, чтобы в дальнейшем продолжить работу с приложением

    (Опционально)
        2. 
        (a)  Как пользователь(Админ), я хочу видеть список ближайших волонтерских центров, чтобы найти подходящий и изучить информацию о нём
        (б) Как пользователь(Админ), я хочу видеть ближайшие волонтерские центры на карте, чтобы найти подходящий и изучить информацию о нём

        3. Как пользователь(Админ), я хочу открыть информацию о конкретном центре, чтобы посмотреть список активных волонтёров в нём

    Пользовательские:
        4. Как пользователь, я хочу зарегистрироваться в системе, чтобы попасть в базу волонтёров и начать работу с приложением
        5. Как пользователь, я хочу иметь возможность изменять свои данные профиля, чтобы информация обо мне была актуальной

    Администраторские:
        6. Как админ, я хочу просматривать список свободных на данный момент волонтеров, чтобы изучить информацию о них
        7. Как админ, я хочу иметь возможность удалять/добавлять пользователей в волонтерские центры, чтобы осуществлять модерацию в приложении

    Опционально:
        8. Как пользователь, я хочу присоединиться к волонтерскому центру, чтобы быть причастным к нему

# Use Cases

## Общие:
    1.
    User Story : “Как пользователь(Админ), я хочу авторизоваться в системе чтобы в дальнейшем продолжить работу с приложением”
    Цель : Авторизация пользователя(Админа)
    Акторы : Пользователь(Админ)
    Предусловия : Учетная запись создана (для пользователя)
    
    Основной поток :
    1.Ввод email и пароль(валидных)
    2.Отправка запроса на сервер
    3.Получение ответа, авторизация, переход на главный экран
    
    Альтернативный поток 1: 
    1.Ввод неверных данных учетной записи(но валидных)
    2.Отправка запроса на сервер
    3.Возвращение ошибки, уведомление пользователя об ошибке (данного пользователя не существует)

    Альтернативный поток 2:
    1.Ввод невалидных данных учетной записи
    2.Возвращение ошибки, уведомление пользователя о неверно введенных данных

    2.
    User Story : “Как пользователь(Админ), я хочу видеть список ближайших волонтерских центров в системе, чтобы найти подходящий и изучить информацию о нём”
    Цель : Просмотр волонтерских центров пользователем
    Акторы  : Пользователь(Админ)
    Предусловия : Авторизация пройдена
    
    Основной поток : 
    1.Кнопка отображения волонтерских центров на главном экране
    2.Приложение просит разрешение на использование местоположения пользователя
    3.Приложение отправляет запрос на сервер с данными о местоположении пользователя
    4.Возвращение ответа(показ списка волонтерских центров, отсортированных на сервере по близости к устройству)
    
    Альтернативный поток 1 : 
    1.Пользователь переходит на страницу волонтерских центров
    2.Кнопка отображения волонтерских центров
    3.Приложение просит разрешение на использование местоположения пользователя
    4.Пользователь отклоняет передачу местоположения
    5.уведомление пользователя об ошибке

    3.
    User Story : “Как пользователь(Админ), я хочу открыть информацию о конкретном центре, чтобы посмотреть список активных волонтёров в нём”
    Цель : Получение информации про конкретный центр
    Акторы : Пользователь(Админ)
    Предусловия : Авторизация пройдена, страница просмотра по центрам открыта
    
    Основной поток : 
    1.Кнопка перехода к конкретному центру
    2.Отправка запроса на сервер
    3.Возвращение ответа с сервера об успехе, отображение информации о центре(название, список активных волонтеров, а если их нет - информацию об этом)
    *Опционально: если пользователь состоит в данном центре, он увидит пометку с информацией об этом
    
    Альтернативный поток 1:
    1.Возвращение ошибки с сервера после запроса
    2.Уведомление пользователя о технической ошибке


## Пользовательские:

    4.
    User Story : “Как пользователь, я хочу зарегистрироваться в системе, чтобы попасть в базу волонтёров и начать работу с приложением”
    Цель : Регистрация пользователя
    Акторы : Пользователь
    Предусловия : Учетная запись еще не создана
    
    Основной поток : 
    1.Ввод данных учетной записи(валидных)
    2.Отправка запроса на сервер
    3.Сохранение пользователя, возвращение положительного ответа, переход на главный экран
    
    Альтернативный поток 1 : 
    1.Ввод данных уже существующего пользователя
    2.Отправка запроса на сервер
    3.Возвращение ошибки повторения, уведомление пользователя об ошибке
    
    Альтернативный поток 2 : 
    1.Ввод неверных данных учетной записи(не валидных), 
    2.Ошибка валидации, уведомление пользователя об ошибке

    5.
    User Story : “Как пользователь, я хочу иметь возможность изменять свои данные профиля, чтобы информация обо мне была актуальной”
    Цель : Редактирование профиля
    Акторы : Пользователь
    Предусловия : Пользователь авторизован
    
    Основной поток : 
    1.Переход на страницу профиля
    2.Редактирование профиля
    3.Отправка данных на сервер
    4.Получение ответа об успешном выполнении
    5.Уведомление пользователя о сохранении изменений
    
    Альтернативный поток 1 : 
    1.Возвращение ошибки с сервера
    2.Уведомление пользователя о возникновении технической ошибки

## Администраторские : 
    
    6.
    User Story : “Как админ, я хочу просматривать список свободных на данный момент волонтеров, чтобы изучить информацию о них”
    Цель :  Просмотр свободных волонтеров, зарегистрированных в системе
    Акторы : Админ
    Предусловия : Админ авторизован
    
    Основной поток :
    1.Переход по кнопке к просмотру незанятых волонтеров
    2.Отправка запроса на сервер
    3.Возвращение результата (Отображение списка незанятых волонтёров, а если список пуст - уведомление пользователя об этом)
    
    Альтернативный поток 1:
    1.Возвращение ошибки с сервера
    2.Уведомление пользователя о возникновении технической ошибки


    7.
    User Story : “Как админ, я хочу удалять/добавлять пользователей в волонтерские центры , чтобы осуществлять модерацию в приложении”
    Цель :  Присоединение пользователя к волонтерскому центру
    Акторы  : Админ
    Предусловия : Авторизация админа пройдена, открыт список незанятых волонтеров
    
    Основной поток :
    1.Админ нажимает на кнопку присоединения волонтера, выбирает нужный центр из списка
    2.Отправка запроса на сервер
    3.Получение ответа об успешном присоединении, уведомления пользователя об этом
    
    Альтернативный поток 1: 
    1.Возвращение ошибки с сервера
    2.Уведомление пользователя о возникновении технической ошибки

                
## Опционально : 

    8.
    User Story : “Как админ, я хочу удалять/добавлять пользователей в волонтерские центры , чтобы осуществлять модерацию в приложении”
    Цель :  Удаление пользователя из волонтерского центра
    Акторы : Админ
    Предусловия : Авторизация админа пройдена,  выполнен поиск по волонтерским центрам, выполнен запрос информации конкретного центра, выполнен запрос просмотра участников центра
    
    Основной поток :
    1.Нажатие кнопки удаления пользователя
    2.Отправка запроса на сервер
    3.Обработка возможности удаления
    4.Фидбек(пользователь успешно удалён), окно об успешном удалении
    
    Альтернативный поток 1: 
    1.Нажатие кнопки удаления пользователя
    2.Отправка запроса на сервер
    3.Обработка возможности удаления(ошибка : пользователь не может быть удален, так как не состоит в центре)
    4.Уведомление об ошибке

    9.
    User Story : “Как пользователь, я хочу присоединиться к волонтерскому центру, чтобы быть причастным к нему”
    Цель : Присоединиться к волонтерскому центру
    Акторы : Пользователь
    Предусловия : Авторизация пройдена, поиск по центрам выполнен успешно, открытие информации конкретного центра выполнено(опционально)
    
    Основной поток :	
    1.Кнопка перехода от информации центра к запросу на присоединение
    2.Запрос к серверу на присоединение
    3.Фидбек от сервера, экран с результатом(Успешно или ошибка)
    
    Альтернативный поток 1: 
    1.Кнопка перехода от информации центра к запросу на присоединение
    2.Запрос к серверу на присоединение(Ошибка : Пользователь уже находится  в группе)
    3.Выброс ошибки, вы уже находитесь в волонтерской группе(может быть только 1 группа)

    10.
    User Story: ”Как пользователь(Админ), я хочу видеть ближайшие волонтерские центры на карте, чтобы найти подходящий и изучить информацию о нём”
    Цель : Просмотр волонтерских центров на карте
    Акторы : Пользователь(админ)
    Предусловия : Авторизация пройдена
    
    Основной поток :	
    1.Пользователь нажимает на кнопку перехода к волонтерским центрам
    2.Приложение запрашивает разрешение на доступ к геолокации
    3.Отправка запроса на сервер
    4.Отображение карты с отмеченными ближайшими волонтерскими центрами
    5.При нажатии на центр, отправка запроса на сервер и отображение пользователю информации о конкретном центре
    
    Альтернативный поток 1: 
    1.Возвращение ошибки с сервера
    2.Уведомление пользователя о возникновении технической ошибки


# Задачи для Бэкенда:

## 1.Авторизация(пользователи, админы) 
    1.Реализовать эндпоинт для авторизации пользователя(админа):
    Метод: GET api/auth/login
    2.Добавить обработку ошибок (email еще не зарегистрирован, некорректный ввод) 
    3.Сравнение с данными в БД

## 2.Регистрация(пользователи) 
    1.Реализовать эндпоинт для авторизации пользователя(админа):
    Метод: POST api/auth/register
    2.Добавить обработку ошибок (email уже зарегистрирован, некорректный ввод) 
    3.Хэширование пароля
    4.Занесение данных в БД
## 3.Отображение волонтерских центров(пользователи, админы)
    1.Реализовать эндпоинт для получения списка волонтерских центров:
    Метод: GET api/search/groups/{user_cord}
    
    2.Написать сортировку центров по расстоянию до устройства

## 4.Отображение конкретного волонтерского центра(пользователи, админы)
    1.Реализовать эндпоинт для получения информации о конкретном волонтерском центре:
    Метод: GET api/get/group/{group_id}
    
    2.Обработка ошибок(Такого волонтерского центра не существует)
    также передать список волонтеров, вместе с информацией о центре

## 5.Поиск всех свободных волонтеров(админы)
    1.Реализовать эндпоинт для получения списка всех свободных волонтеров:
    Метод: GET api/get/volunteers_all

    2.Выдаем только волонтеров, которые не принадлежат какому-либо центру

## 6.Присоединение волонтера к центру(админы, пользователи(при опциональных use case)
    1.Реализовать эндпоинт для присоединения волонтера к центру):
    Метод: POST api/join/group/{group_id}/{user_id}

    2.Обработка ошибок(Волонтер уже имеет другой центр итд)

## 7.Изменение профиля(пользователи)
    1.Реализовать эндпоинт для получения данных по профилю данного пользователя
    Метод: GET api/get/profiles/{user_id}

    2.Реализовать эндпоинт для изменения данных по профилю данного пользователя
    Метод: PUT api/edit/profiles/{user_id} 

    3.Обработка ошибок(пользователь может изменять только свой профиль)

## 8.Настроить Spring Security для разграничения доступа

# Сущности в БД
![Screenshot_5](https://github.com/user-attachments/assets/1228c0af-4ddd-474e-a13d-3859067f9c8b)
