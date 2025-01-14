[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/TwoHearts-AI/system-design/main.svg)](https://results.pre-commit.ci/latest/github/TwoHearts-AI/system-design/main)

# Дизайн документа ML-системы - TwoHearts_AI MVP 1.0

*Основано на шаблоне ML System Design Doc от телеграм-канала [Reliable ML](https://t.me/reliable_ml)*

---

## 1. Цели и предпосылки

### 1.1 Цели разработки продукта

- **Бизнес-цель:**

    - Создать мобильное приложение **TwoHearts_AI** для улучшения психоэмоционального состояния пользователей посредством анализа их переписки с использованием больших языковых моделей (LLM) и методов обработки естественного языка (NLP).
    - Достичь 5000 ежемесячных активных пользователей к концу декабря 2024 года.
    - Получить грантовую поддержку и финансирование для дальнейшего развития.

- **Преимущества использования ML:**

    - Автоматический анализ длинных переписок с помощью LLM и NLP позволит выявлять паттерны коммуникации и предоставлять персонализированные рекомендации.
    - Сделает психологическую помощь более доступной и оперативной, снизив стоимость услуг по сравнению с традиционными методами.
    - Пользователи смогут улучшить свои отношения без необходимости обращения к специалистам.

- **Критерии успеха с точки зрения бизнеса:**

    - Разработка MVP приложения с функционалом анализа переписки и генерации отчёта.
    - Получение положительной обратной связи от не менее 70% пользователей бета-тестирования.
    - Привлечение первых платных пользователей и достижение ключевых показателей вовлечённости.

### 1.2 Бизнес-требования и ограничения

- **Бизнес-требования:**

    - **Функциональные:**
        - Реализовать приложение, позволяющее пользователям загружать переписку из Telegram.
        - Автоматически создавать визуальный отчёт с результатами анализа чата.
        - Предоставлять персонализированные рекомендации по улучшению отношений.
        - Обеспечить безопасность и конфиденциальность данных пользователей.

    - **Нефункциональные:**
        - Обеспечить высокую производительность и надёжность системы.
        - Создать удобный и интуитивно понятный пользовательский интерфейс.
        - Соответствовать законодательству по обработке персональных данных.

- **Бизнес-ограничения:**

    - Ограниченный бюджет на разработку и инфраструктуру.
    - Поддержка только русского языка на этапе пилотного запуска.
    - Локальная обработка данных без подключения сторонних сервисов.
    - Ограничения в технической экспертизе команды.

### 1.3 Область охвата проекта

- **Входит в скоуп:**

    - Разработка Telegram-бота для взаимодействия с пользователями.
    - Реализация функционала выгрузки и обработки переписки из Telegram.
    - Настройка RAG-пайплайна и интеграция с локальной LLM.
    - Генерация визуального отчёта с анализом коммуникации и конфликтности партнёров.
    - Обеспечение безопасности данных и соответствие законодательству.

- **Не входит в скоуп:**

    - Поддержка выгрузки переписки из WhatsApp и других мессенджеров.
    - Анализ медиафайлов и мультимодальных данных.
    - Реализация анализа по типологиям личности Майерс-Бриггс (MBTI).
    - Тонкая настройка модели на пользовательских данных.
    - Поддержка других языков на этапе пилотного запуска.

### 1.4 Предпосылки и обоснование решения

- **Использование локальных LLM:**

    - Для обеспечения конфиденциальности данных и соответствия законодательству обработка происходит на собственных серверах без передачи данных третьим лицам.
    - Используются модели с открытым исходным кодом, дообученные на русском языке.

- **Применение RAG (Retrieval Augmented Generation):**

    - Техника RAG помогает преодолеть ограничения окна контекста LLM.
    - Позволяет извлекать релевантные фрагменты из длинных переписок для анализа.

- **Фокус на русском языке:**

    - Основная целевая аудитория — русскоговорящие пользователи.
    - Модель и инструменты должны эффективно работать с особенностями русского языка.

- **Безопасность и конфиденциальность:**

    - Строгое соблюдение законодательства по обработке персональных данных (GDPR, ФЗ-152).
    - Шифрование данных при хранении и передаче.
    - Данные переписки не сохраняются после обработки.

---

## 2. Методология

### 2.1 Постановка задачи

- **Тип задачи:**

    - Анализ текста с использованием NLP для выявления паттернов коммуникации.
    - Генерация персонализированных рекомендаций по улучшению отношений.

- **Техническая цель:**

    - Разработать систему автоматизированного анализа текстовой переписки пользователей с использованием LLM и RAG для предоставления рекомендаций по улучшению коммуникации и отношений между партнёрами.

### 2.2 Схема решения

1. **Пользовательский интерфейс (Telegram-бот):**

     - Регистрация и авторизация пользователя.
     - Предоставление инструкций по выгрузке переписки.
     - Получение согласия на обработку данных.

2. **Выгрузка и предобработка переписки:**

     - Получение переписки через Telegram API с согласия пользователя.
     - Очистка данных, токенизация, нормализация текста.

3. **Индексация и создание эмбеддингов:**

     - Разбивка переписки на логические фрагменты.
     - Создание эмбеддингов с помощью модели Sentence Transformers.
     - Индексация эмбеддингов в векторной базе данных (FAISS).

4. **Извлечение релевантного контекста (RAG):**

     - Поиск релевантных фрагментов на основе заданных тем (коммуникация, конфликтность).
     - Использование косинусного расстояния для оценки схожести эмбеддингов.

5. **Анализ с помощью LLM:**

     - Промпт-инжиниринг для улучшения качества генерации.
     - Использование локальной LLM для анализа контекста.
     - Получение оценок и рекомендаций.

6. **Формирование отчёта:**

     - Структурирование выводов и рекомендаций.
     - Визуализация данных (графики, диаграммы).
     - Генерация отчёта в формате PDF с использованием WeasyPrint.

7. **Доставка отчёта пользователю:**

     - Отправка отчёта через бота или на электронную почту.
     - Обеспечение безопасности передачи данных.

### 2.3 Этапы решения задачи

**Этап 1: Подготовка данных**

- **Данные:**

    - Переписка пользователей, полученная через Telegram-бот с согласия пользователя.

- **Действия:**

    - Предобработка текста: очистка, токенизация, нормализация.
    - Обеспечение безопасности данных и удаление после обработки.

**Этап 2: Индексация и извлечение контекста (RAG)**

- **Действия:**

    - Разбивка текста на фрагменты и создание эмбеддингов.
    - Индексация в векторной базе данных для быстрого поиска.
    - Извлечение релевантных фрагментов по заданным темам.

**Этап 3: Анализ с помощью LLM**

- **Действия:**

    - Создание эффективных промптов для модели.
    - Анализ контекста и генерация оценок и рекомендаций.
    - Использование модели, дообученной на русском языке.

**Этап 4: Формирование отчёта**

- **Действия:**

    - Компиляция результатов в структурированный отчёт.
    - Визуализация результатов с помощью графиков и диаграмм.
    - Генерация отчёта в формате PDF.

**Этап 5: Доставка отчёта пользователю**

- **Действия:**

    - Отправка отчёта пользователю через Telegram-бот или на электронную почту.
    - Сбор обратной связи от пользователя.

---

## 3. Подготовка пилота

### 3.1 Способ оценки пилота

- **Цель пилота:**

    - Проверить работоспособность системы и удовлетворённость пользователей.

- **Метрики успеха:**

    - Успешная обработка не менее 95% запросов без сбоев.
    - Средняя оценка удовлетворённости пользователей не ниже 4 из 5.
    - Привлечение первых платных пользователей.

- **Способ оценки:**

    - Сбор технических метрик (время обработки, количество ошибок).
    - Проведение опросов пользователей для оценки качества рекомендаций.
    - Анализ обратной связи и предложений по улучшению.

### 3.2 Критерии успешного пилота

- **Успех пилота:**

    - Техническая стабильность системы.
    - Высокий уровень удовлетворённости пользователей.
    - Готовность пользователей рекомендовать приложение другим.

### 3.3 Подготовка к пилоту

- **Действия:**

    - Настройка инфраструктуры и развёртывание приложения.
    - Проведение внутреннего тестирования на ограниченной группе пользователей.
    - Привлечение пользователей для бета-тестирования.
    - Обучение команды поддержки для оперативного решения возникающих вопросов.

---

## 4. Внедрение

### 4.1 Архитектура решения

- **Компоненты:**

    - **Telegram-бот (Aiogram):** Взаимодействие с пользователем, получение переписки, отправка отчётов.
    - **Backend (FastAPI):** Обработка запросов, управление бизнес-логикой, вызов моделей.
    - **RAG-модуль:** Извлечение релевантного контекста из переписки.
    - **LLM-модуль:** Анализ контекста и генерация выводов.
    - **База данных (SQLite):** Хранение информации о сессиях и логах (переписка не сохраняется).
    - **Модуль отчётности (WeasyPrint):** Генерация визуального отчёта в формате PDF.
    - **Сервис отправки:** Доставка отчёта пользователю через бота или на электронную почту.

- **Архитектура решения:**

    <img src="imgs/chat_analysis_scheme.png" alt="Схема анализа чата" style="max-width: 100%; height: auto;">

- **Взаимодействие компонентов:**

    <img src="imgs/service_architecture.png" alt="Схема Взаимодействие компонентов" style="max-width: 100%; height: auto;">

### 4.2 Инфраструктура и масштабируемость

- **Инфраструктура:**

    - Использование серверов с достаточной вычислительной мощностью для локального запуска LLM.
    - Возможное использование облачных решений (VK Cloud, Яндекс.Облако) для масштабирования.
    - Контейнеризация сервисов с помощью Docker для упрощения развёртывания.

- **Масштабируемость:**

    - Горизонтальное масштабирование сервисов по мере роста нагрузки.
    - Использование балансировщиков нагрузки для распределения запросов.
    - Кэширование часто используемых данных для снижения нагрузки.

### 4.3 Требования к работе системы

- **Производительность:**

    - Время от загрузки переписки до получения отчёта не более 10 минут.
    - Обработка до 50 запросов в час в период бета-тестирования.

- **Доступность:**

    - Доступность сервиса не менее 99%.

- **Безопасность:**

    - Шифрование данных при передаче и хранении.
    - Регулярное обновление и патчинг используемого ПО.

### 4.4 Безопасность системы и данных

- **Меры безопасности:**

    - Использование HTTPS для всех сетевых взаимодействий.
    - Ограничение доступа к серверам по IP и использование VPN для администрирования.
    - Регулярные аудиты безопасности и тестирование на уязвимости.

- **Конфиденциальность данных:**

    - Получение согласия пользователя на обработку данных.
    - Удаление переписки после обработки и формирования отчёта.
    - Отсутствие сохранения личных данных пользователей.

### 4.5 Издержки

- **Расчётные издержки в месяц:**

    - **Инфраструктура:**

        - Серверные мощности: ~60 000 руб.
        - Хранение и передача данных: ~10 000 руб.

    - **Персонал:**

        - Техническая поддержка и обслуживание: ~80 000 руб.
        - Маркетинг и продвижение: ~30 000 руб.

    - **Прочие расходы:**

        - Лицензии, домены, сертификаты: ~10 000 руб.

    - **Итого:** ~190 000 руб.

### 4.6 Риски и меры по их снижению

- **Технические риски:**

    - **Риск:** Нестабильность работы LLM при высокой нагрузке.
    - **Меры:** Оптимизация моделей, нагрузочное тестирование, использование кэша.

- **Правовые риски:**

    - **Риск:** Несоответствие требованиям законодательства по обработке персональных данных.
    - **Меры:** Консультации с юристами, разработка политики конфиденциальности, соблюдение GDPR и ФЗ-152.

- **Риски безопасности:**

    - **Риск:** Возможность утечки данных или несанкционированного доступа.
    - **Меры:** Реализация современных мер информационной безопасности, регулярные проверки и обновления.

- **Кадровые риски:**

    - **Риск:** Недостаток экспертизы в области NLP и психологии.
    - **Меры:** Привлечение экспертов, обучение команды, сотрудничество с университетами.

---

## 5. Дополнительно

### 5.1 Планы по развитию

- **Расширение функционала:**

    - Поддержка других мессенджеров (WhatsApp, Viber).
    - Анализ медиафайлов и мультимодальных данных.
    - Реализация анализа по типологиям личности (MBTI).

- **Выход на новые рынки:**

    - Интернационализация продукта и поддержка других языков.
    - Выход на корпоративный рынок для анализа деловой переписки.

- **Монетизация:**

    - Введение модели подписки и дополнительных платных функций.
    - Рассмотрение партнёрств и интеграций с другими сервисами.

---

### Материалы для дополнительного погружения в тему  
docs/) с объяснением каждого раздела  
- [Верхнеуровневый шаблон ML System Design Doc от Google](https://towardsdatascience.com/the-undeniable-importance-of-design-docs-to-data-scientists-421132561f3c) и [описание общих принципов его заполнения](https://towardsdatascience.com/understanding-design-docs-principles-for-achieving-data-scientists-53e6d5ad6f7e).
- [ML Design Template](https://www.mle-interviews.com/ml-design-template) от ML Engineering Interviews  
- Статья [Design Documents for ML Models](https://medium.com/people-ai-engineering/design-documents-for-ml-models-bbcd30402ff7) на Medium. Верхнеуровневые рекомендации по содержанию дизайн-документа и объяснение, зачем он вообще нужен  
- [Краткий Canvas для ML-проекта от Made with ML](https://madewithml.com/courses/mlops/design/#timeline). Подходит для верхнеуровневого описания идеи, чтобы понять, имеет ли смысл идти дальше.