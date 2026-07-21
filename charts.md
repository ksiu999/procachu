## Use Case Diagram

flowchart LR
subgraph Акторы
Client((👤 Клиент))
Operator((🎧 Оператор))
CTO((⚙️ CTO))
end

    subgraph Система "Прокачу"
        UC1[Регистрация по телефону/email]
        UC2[Просмотр карты станций]
        UC3[Бронирование велосипеда]
        UC4[Старт аренды / Разблокировка замка]
        UC5[Завершение аренды]
        UC6[Просмотр истории поездок]
        UC7[Отчет о поломке]
        UC8[Пополнение баланса / Оплата]
        UC9[Обработка возвратов средств]
        UC10[Управление парком и станциями]
        UC11[Мониторинг системы и логов]
    end

    Client --> UC1
    Client --> UC2
    Client --> UC3
    Client --> UC4
    Client --> UC5
    Client --> UC6
    Client --> UC7
    Client --> UC8

    Operator --> UC9
    Operator --> UC6

    CTO --> UC10
    CTO --> UC11
    CTO --> UC6

    UC5 -.->|include| UC12[Проверка геолокации]
    UC4 -.->|include| UC13[Команда Bluetooth-замку]
    UC8 -.->|include| UC14[Интеграция с PCI DSS шлюзом]
    UC7 -.->|extend| UC15[Автоматическое исключение из парка]

    subgraph Внешние системы
        UC12
        UC13
        UC14
        UC15
    end

## Sequence Diagram

sequenceDiagram
autonumber
participant C as Клиент
participant App as Мобильное приложение
participant API as Backend API
participant Lock as Bluetooth-замок
participant Pay as Платежный шлюз
participant DB as База данных

    Note over C,DB: Сценарий: Успешная аренда с автозавершением по геолокации

    C->>App: Открывает карту, выбирает станцию
    App->>API: GET /api/v1/stations
    API-->>App: Список станций с координатами и кол-вом велосипедов
    App-->>C: Отображает карту с маркерами

    C->>App: Нажимает "Забронировать" (велосипед ID-123)
    App->>API: POST /api/v1/bookings {stationId, bikeId}
    API->>DB: Резервирует велосипед на 15 минут
    API-->>App: {bookingId, expiresAt}
    App-->>C: Push: "Велосипед забронирован на 15 минут"

    C->>App: Подходит к велосипеду, нажимает "Начать аренду"
    App->>API: POST /api/v1/rentals/start {bookingId, location}
    API->>DB: Создает запись аренды (status=ACTIVE)
    API->>Lock: Команда разблокировки (через BLE)
    Lock-->>API: Подтверждение: замок открыт
    API-->>App: {rentalId, unlockToken, tariff}
    App-->>C: Экран активной аренды (таймер, стоимость)

    Note over C,DB: Поездка... Приложение отправляет геолокацию каждые 10 сек

    loop Мониторинг геолокации
        App->>API: POST /api/v1/rentals/{id}/location {lat, lng}
        API->>DB: Обновляет координаты
    end

    C->>App: Возвращает велосипед на станцию
    App->>API: POST /api/v1/rentals/{id}/finish {location}
    API->>API: Проверяет: расстояние до станции < 20м?
    API->>Lock: Команда блокировки замка
    Lock-->>API: Замок закрыт
    API->>DB: Рассчитывает стоимость (тариф × время)
    API->>Pay: POST /charges {amount, token}
    Pay-->>API: {transactionId, status: SUCCESS}
    API->>DB: Обновляет статус аренды (status=COMPLETED)
    API-->>App: {receipt, amount}
    App-->>C: Push: "Аренда завершена. Списано: 225 руб."
    App-->>C: Email с чеком

## State Machine Diagram

stateDiagram-v2
[*] --> Забронирован: Клиент нажал "Забронировать"

    Забронирован --> Активен: Старт аренды (разблокировка замка)
    Забронирован --> Отменена: Истек срок брони (15 мин)
    Забронирован --> Отменена: Клиент отменил бронь

    Активен --> Завершена: Возврат на станцию (геолокация < 20м)
    Активен --> Завершена: Ручное завершение оператором
    Активен --> Принудительно_завершена: Велосипед не возвращен до 23:00
    Активен --> Инцидент: Отчет о поломке

    Инцидент --> Завершена: Оператор подтвердил возврат
    Инцидент --> На_ремонте: Оператор подтвердил поломку

    На_ремонте --> Доступен: Ремонт завершен
    Доступен --> Забронирован: Новый клиент забронировал

    Завершена --> [*]
    Принудительно_завершена --> [*]
    Отменена --> [*]

    note right of Активен
        Тарификация активна
        Геолокация отслеживается каждые 10 сек
    end note

    note right of Забронирован
        Велосипед заблокирован для других
        Таймер: 15 минут
    end note
