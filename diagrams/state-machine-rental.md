[⬅️ Вернуться к оглавлению](../README.md) | [← Предыдущая: Sequence Diagram](sequence-rental.md)

# 3. State Machine Diagram — Жизненный цикл аренды

Диаграмма состояний описывает все возможные статусы аренды и велосипеда, а также переходы между ними. Это ключевая диаграмма для разработки backend-логики и обработки граничных случаев.

![State Machine Diagram](https://mermaid.ink/img/eyJjb2RlIjogInN0YXRlRGlhZ3JhbS12MlxuICAgIFsqXSAtLT4gQm9va2VkOiBcdTA0MWFcdTA0M2JcdTA0MzhcdTA0MzVcdTA0M2RcdTA0NDIgXHUwNDNkXHUwNDMwXHUwNDM2XHUwNDMwXHUwNDNiIFx1MDQxN1x1MDQzMFx1MDQzMVx1MDQ0MFx1MDQzZVx1MDQzZFx1MDQzOFx1MDQ0MFx1MDQzZVx1MDQzMlx1MDQzMFx1MDQ0Mlx1MDQ0Y1xuXG4gICAgQm9va2VkIC0tPiBBY3RpdmU6IFx1MDQyMVx1MDQ0Mlx1MDMwMFx1MDQwMFx1MDQ0MiBcdTA4MzBcdTA4NDBcdTA4MzVcdTA4M2RcdTA4MzRcdTA4NGIgKFx1MDQ0MFx1MDMzMFx1MDM3N1x1MDMzMVx1MDMzYlx1MDMzZVx1MDMzOFx1MDM0MFx1MDMzZVx1MDMzMlx1MDMzYVx1MDMzMCBcdTA0MzdcdTA0MzBcdTA0M2NcdTA0M2FcdTA0MzApXG4gICAgQm9va2VkIC0tPiBDYW5jZWxsZWQ6IFx1MDQxOFx1MDQ0MVx1MDQ0Mlx1MDQzNVx1MDQzYSBcdTA0NDFcdTA0NDBcdTA0M2VcdTA0M2EgXHUwNDMxXHUwNDQwXHUwNDNlXHUwNDNkXHUwNDM4IDE1IFx1MDQzY1x1MDQzOFx1MDQzZFxuICAgIEJvb2tlZCAtLT4gQ2FuY2VsbGVkOiBcdTA0MWFcdTA0M2JcdTA0MzhcdTA0MzVcdTA0M2RcdTA0NDIgXHUwNDNlXHUwNDQyXHUwNDNjXHUwNDM1XHUwNDNkXHUwNDM4XHUwNDNiIFx1MDQzMVx1MDQ0MFx1MDQzZVx1MDQzZFx1MDQ0Y1xuXG4gICAgQWN0aXZlIC0tPiBDb21wbGV0ZWQ6IFx1MDQxMlx1MDQzZVx1MDQzN1x1MDQzMlx1MDQ0MFx1MDQzMFx1MDQ0MiBcdTA0M2RcdTA4MzAgXHUwNDQxXHUwNDQyXHUwNDMwXHUwNDNkXHUwNDQ2XHUwNDM4XHUwNDRlIChcdTA0MzNcdTA0MzVcdTA0M2VcdTA0M2JcdTA0M2VcdTA0M2FcdTA4MzBcdTA4NDZcdTA4MzhcdTA4NGYgPCAyMFx1MDQzYylcbiAgICBBY3RpdmUgLS0-IENvbXBsZXRlZDogXHUwNDIwXHUwNDQzXHUwNDQ3XHUwNDNkXHUwNDNlXHUwNDM1IFx1MDQzN1x1MDQzMFx1MDQzMlx1MDQzNVx1MDQ0MFx1MDQ0OFx1MDQzNVx1MDQzZFx1MDQzOFx1MDQzNSBcdTA4MGVcdTA4M2ZcdTA4MzVcdTA4NDBcdTA4MzBcdTA4NDJcdTA4M2VcdTA4NDBcdTA4M2VcdTA4M2NcbiAgICBBY3RpdmUgLS0-IEZvcmNlQ29tcGxldGVkOiBcdTA0MTJcdTA0MzVcdTA0M2JcdTA4M2VcdTA4NDFcdTA4MzhcdTA4M2ZcdTA4MzVcdTA4MzQgXHUwNDNkXHUwNDM1IFx1MDQzMlx1MDQzZVx1MDQzN1x1MDQzMlx1MDQ0MFx1MDQzMFx1MDQ0OVx1MDQzNVx1MDQzZCBcdTA0MzRcdTA0M2UgMjM6MDBcbiAgICBBY3RpdmUgLS0-IEluY2lkZW50OiBcdTA0MWVcdTA0NDJcdTA0NDdcdTA4MzVcdTA4NDIgXHUwNDNlIFx1MDQzZlx1MDQzZVx1MDQzYlx1MDQzZVx1MDQzY1x1MDQzYVx1MDQzNVxuXG4gICAgSW5jaWRlbnQgLS0-IENvbXBsZXRlZDogXHUwNDFlXHUwNDNmXHUwNDM1XHUwNDQwXHUwNDMwXHUwNDQyXHUwNDNlXHUwNDQwIFx1MDQzZlx1MDQzZVx1MDQzNFx1MDQ0Mlx1MDMzMlx1MDMzNVx1MDQ0MFx1MDM0OFx1MDMzYiBcdTA0MzJcdTA0M2VcdTA0MzdcdTA4MzJcdTA4NDBcdTA4MzBcdTA4NDJcbiAgICBJbmNpZGVudCAtLT4gTWFpbnRlbmFuY2U6IFx1MDQxZVx1MDQzZlx1MDQzNVx1MDQ0MFx1MDQzMFx1MDQ0Mlx1MDQzZVx1MDQ0MCBcdTA0M2ZcdTA0M2VcdTA4MzRcdTA4NDJcdTA4MzJcdTA4MzVcdTA4NDBcdTA4MzRcdTA4MzhcdTA4M2IgXHUwNDNmXHUwNDNlXHUwNDNiXHUwNDNlXHUwNDNjXHUwNDNhXHUwNDQzXG5cbiAgICBNYWludGVuYW5jZSAtLT4gQXZhaWxhYmxlOiBcdTA0MjBcdTA4MzVcdTA4M2NcdTA4M2VcdTA4M2RcdTA4NDIgXHUwNDM3XHUwNDMwXHUwNDMyXHUwNDM1XHUwNDQwXHUwNDQ4XHUwNDM1XHUwNDNkXG4gICAgQXZhaWxhYmxlIC0tPiBCb29rZWQ6IFx1MDQxZFx1MDQzZVx1MDQzMlx1MDQ0Ylx1MDQzOSBcdTA4M2FcdTA4M2JcdTA4MzhcdTA4MzVcdTA4M2RcdTA4NDIgXHUwNDM3XHUwNDMwXHUwNDMxXHUwNDQwXHUwNDNlXHUwNDNkXHUwNDM4XHUwNDQwXHUwNDNlXHUwNDMyXHUwNDMwXHUwNDNiXG5cbiAgICBDb21wbGV0ZWQgLS0tIFsqXVxuICAgIEZvcmNlQ29tcGxldGVkIC0tPiBbKl1cbiAgICBDYW5jZWxsZWQgLS0tIFsqXVxuXG4gICAgbm90ZSByaWdodCBvZiBBY3RpdmVcbiAgICAgICAgXHUwNDIyXHUwNDMwXHUwNDQwXHUwNDM4XHUwNDQ0XHUwNDM4XHUwNDNhXHUwNDMwXHUwNDQ2XHUwNDM4XHUwNDRmIFx1MDQzMFx1MDQzYVx1MDQ0Mlx1MDM4OFx1MDMzMlx1MDMzZFx1MDMzMFxuICAgICAgICBcdTA0MTNcdTA0MzVcdTA0M2VcdTA4M2JcdTA4M2VcdTA4M2FcdTA4MzBcdTA4NDZcdTA4MzhcdTA4NGYgXHUwNDNlXHUwNDQyXHUwNDQxXHUwNDNiXHUwNDM1XHUwNDM2XHUwNDM4XHUwNDMyXHUwNDMwXHUwNDM1XHUwNDQyXHUwNDQxXHUwNDRmIFx1MDQzYVx1MDQzMFx1MDQzNlx1MDQzNFx1MDQ0Ylx1MDQzNSAxMCBcdTA0NDFcdTA0MzVcdTA4M2FcbiAgICBlbmQgbm90ZVxuXG4gICAgbm90ZSByaWdodCBvZiBCb29rZWRcbiAgICAgICAgXHUwNDEyXHUwNDM1XHUwNDNiXHUwNDNlXHUwNDQxXHUwNDM4XHUwNDNmXHUwNDM1XHUwNDM0IFx1MDQzN1x1MDQzMFx1MDQzMVx1MDQzYlx1MDQzZVx1MDQzYVx1MDQzOFx1MDQ0MFx1MDQzZVx1MDQzMlx1MDQzMFx1MDQzZCBcdTA4MzRcdTA4NGJcdTA4NGZcdTA4MzNcdTA4MzhcdTA4NDVcbiAgICAgICAgXHUwNDIyXHUwNDMwXHUwNDM5XHUwNDNjXHUwNDM1XHUwNDQwOiAxNSBcdTA4M2NcdTA4MzhcdTA4M2RcdTA4NDNcdTA4NDJcbiAgICBlbmQgbm90ZSIsICJtZXJtYWlkIjogeyJ0aGVtZSI6ICJkZWZhdWx0In19)

> 💡 _Если изображение не отображается, кликните по нему — откроется интерактивная версия на [Mermaid.ink](https://mermaid.ink/)_

## Описание состояний

| Состояние                                    | Описание                                                                                      |
| -------------------------------------------- | --------------------------------------------------------------------------------------------- |
| **Booked** (Забронирован)                    | Велосипед зарезервирован конкретным клиентом на 15 минут. Недоступен для других.              |
| **Active** (Активен)                         | Аренда начата, замок разблокирован. Идет тарификация, геолокация отслеживается каждые 10 сек. |
| **Completed** (Завершена)                    | Велосипед возвращен на станцию, средства списаны.                                             |
| **ForceCompleted** (Принудительно завершена) | Велосипед не возвращен до 23:00. Система автоматически завершила аренду.                      |
| **Incident** (Инцидент)                      | Клиент сообщил о поломке. Ожидает решения оператора.                                          |
| **Maintenance** (На ремонте)                 | Оператор подтвердил поломку. Велосипед исключен из доступного парка.                          |
| **Available** (Доступен)                     | Велосипед исправен и готов к новой аренде.                                                    |
| **Cancelled** (Отменена)                     | Бронь истекла (15 мин) или была отменена клиентом.                                            |

## Ключевые переходы

- **Booked → Active**: Клиент подошел к велосипеду и нажал «Начать аренду».
- **Booked → Cancelled**: Истек таймер брони (15 минут) без старта аренды.
- **Active → Completed**: Геолокация показала возврат на станцию (радиус < 20м).
- **Active → ForceCompleted**: Время 23:00, велосипед все еще в аренде.
- **Active → Incident**: Клиент нажал «Сообщить о поломке».
- **Incident → Maintenance**: Оператор подтвердил неисправность.
- **Maintenance → Available**: Ремонт завершен, велосипед снова в строю.

---

[⬅️ Вернуться к оглавлению](../README.md) | [← Предыдущая: Sequence Diagram](sequence-rental.md)
