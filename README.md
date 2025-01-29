# Android-Bootcamp-2025-Backend
# Команда: For Fun
Разработать программный продукт для помощи в осуществлении волонтерской деятельности в клиент-серверной архитектуре.

Клиент - Android Приложение, реализованное на java или kotlin с адаптивной версткой.
Мобильное приложение должно предоставлять удобный и интуитивно понятный интерфейс для волонтеров, в котором можно зарегистрироваться в базу волонтеров, просмотреть ближайшие волонтерские центры, отредактировать собственный профиль и посмотреть активных волонтеров в конкретном центре. В случае авторизации с правами администратора, пользователь приложения должен иметь возможность просмотреть список не занятых волонтеров и их контактные данные.

Сервер - микросервис на Spring Boot, работающий с In-memory базой данных H2.
Серверное приложение должно предоставлять API необходимый для работы мобильного приложения. Осуществлять все основные манипуляции с данными в СУБД при помощи Spring Data JPA. Данные о всех зарегистрированных волонтерских центрах должны быть предзаполнены в БД при запуске приложения.Создание схемы БД и предзаполнение данными необходимо реализовать с использованием liquibase.

# User Stories & Use Cases
1. Регистрация пользователя (Use Case 1)
Цель: Дать возможность волонтеру зарегистрироваться в системе для участия в волонтерской деятельности.
Участники:
Пользователь
Система
Предусловие:
Пользователь должен иметь доступ к приложению.
Основной поток:
Пользователь запускает приложение.
Пользователь выбирает кнопку "Зарегистрироваться".
Пользователь вводит свои данные (имя, фамилия, контактные данные, email и т.д.).
Система проверяет корректность данных.
После успешной регистрации система сохраняет данные пользователя в базе.
Пользователь получает сообщение о успешной регистрации.
Альтернативный поток:
Если данные введены некорректно, система возвращает ошибку и просит исправить данные.
2. Просмотр волонтерских центров (Use Case 2)
Цель: Дать волонтеру возможность ознакомиться с ближайшими волонтерскими центрами.
Участники:
Пользователь
Предусловие:
Пользователь должен быть не авторизован или авторизован (не обязательно).
Основной поток:
Пользователь открывает приложение.
Пользователь видит карту или список волонтерских центров, расположенных поблизости.
Пользователь выбирает интересующий его центр.
Приложение отображает информацию о выбранном центре (контактные данные, адрес и т.д.).
Альтернативный поток:
Если нет волонтерских центров поблизости, система уведомляет об этом.
Либо если ошибка сервера, то показывает плашку
3. Редактирование профиля (Use Case 3)
Цель: Дать волонтеру возможность редактировать свои контактные данные и информацию о себе.
Участники:
Пользователь
Предусловие:
Пользователь должен быть авторизован в системе.
Основной поток:
Пользователь входит в приложение.
Пользователь переходит в раздел "Профиль".
Пользователь редактирует свои данные (контактные данные, интересы и т.д.).
Система сохраняет изменения в базе данных.
Альтернативный поток:
Если данные введены некорректно, система сообщает о необходимости исправить ошибки.
4. Просмотр активных волонтеров (Use Case 4)
Цель: Дать волонтеру возможность увидеть активных волонтеров в выбранном центре.
Участники:
Пользователь (волонтер)
Предусловие:
Пользователь должен выбрать волонтерский центр.
Основной поток:
Пользователь выбирает волонтерский центр.
Система отображает список активных волонтеров в этом центре.
Пользователь может увидеть имя, контактные данные и другую информацию о волонтерах.
Альтернативный поток:
Если в центре нет активных волонтеров, система уведомляет пользователя.
5. Авторизация с правами администратора (Use Case 5)
Цель: Дать администратору возможность авторизоваться и управлять данными волонтеров.
Участники:
Администратор
Предусловие:
Администратор должен иметь учетные данные для входа.
Основной поток:
Администратор вводит логин и пароль.
Система проверяет правильность введенных данных.
При успешной авторизации администратор получает доступ к функциям для управления данными волонтеров.
Альтернативный поток:
Если данные введены некорректно, система сообщает о неверном логине или пароле.
6. Просмотр не занятых волонтеров (Use Case 6)
Цель: Дать администратору возможность видеть список свободных волонтеров для назначения на проекты.
Участники:
Администратор
Предусловие:
Администратор должен быть авторизован.
Основной поток:
Администратор входит в систему.
Администратор выбирает вкладку "Свободные волонтеры".
Система отображает список волонтеров, не занятых в текущих проектах.
Альтернативный поток:
Если нет свободных волонтеров, система уведомляет об этом.
7. Защита данных пользователя (Use Case 7)
Цель: Обеспечить безопасность данных пользователя через двухфакторную аутентификацию.
Участники:
Пользователь (волонтер)
Предусловие:
Пользователь должен иметь действующий телефон или email.
Основной поток:
Пользователь вводит логин и пароль.
Система отправляет код на телефон или email пользователя.
Пользователь вводит код для подтверждения входа.
Альтернативный поток:
Если код введен неверно, система сообщает об ошибке и предлагает повторить попытку.

# Задачи для разработчика Backend
1. API для получения списка волонтерских центров — разработка маршрутов и логики получения данных о волонтерских центрах из базы данных.
2. Система авторизации с правами администратора и волонтера — создание API для регистрации, логина и обработки сессий, а также назначения прав.
3. Функциональность редактирования профиля пользователя — разработка API для изменения данных профиля пользователя, включая проверку прав доступа.
4. Функциональность отображения активных волонтеров в центре — создание маршрутов для получения списка волонтеров, их статуса, привязки к центрам.
5. Система отображения свободных волонтеров для администратора — логика для фильтрации и получения списка свободных волонтеров.
6. Двухфакторная аутентификация (частично) — создание API для генерации и проверки кодов аутентификации.
7. Интеграция с системой для автозаполнения БД — настройка бэкенд-логики для получения данных и автозаполнения таблиц/моделей в базе данных

# Схема БД 
https://imgur.com/a/vFtof1V