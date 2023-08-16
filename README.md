# Тестирование новой версии сервиса Яндекс.Маршруты
## Часть 1
Текущая версия Яндекс.Маршрутов  отличается от предыдущей версии. Например, теперь в приложении можно заказать каршеринг. В первой части проекта нужно протестировать эту функциональность в двух окружениях:
- Яндекс.Браузер, разрешение экрана 800x600;
- Firefox, разрешение экрана 1920x1080.
### Подготовить тестовую документацию, чтобы проверить вёрстку формы бронирования и навигационной карты. 
1. Изучить макеты каршеринга в Figma — вкладка «Marshruti car sharing».
- Обрати внимание на блок «Требования к заказу». В макетах изображен не каршеринг, а такси: это вкладка «Marshruti taxi» → UI kit. Но принцип работы везде одинаковый. Отличаются только тарифы и текст.
- Подробнее о работе панели «Требования к заказу» в каршеринге можно прочитать в текстовых требованиях.
2. Составь чек-лист, по которому будешь тестировать вёрстку формы бронирования и навигационной карты.
- Выбрать только один тариф (тариф Повседневный).
- Валидацию полей, а также вёрстку окон «Добавление прав», «Способ оплаты», «Добавление карты» тестировать не нужно.
### Подготовить тестовую документацию, чтобы проверить логику работы. 
1. Проанализируй требования к функциональности каршеринга.
2. Составь чек-лист, по которому будешь проверять функциональность окон «Способ оплаты» и «Добавление карты». Здесь пригодятся соответствующие пункты требований: «Поле “Способ оплаты”» и «Окно “Добавление карты”».
- Во время составления чек-листа, использовать технику разбиения на классы эквивалентности и выделения граничных значений.
- Сделать позитивные и негативные проверки
3. Подготовить тест-кейсы:
- На логику работы кнопки «Забронировать» — см. пункт требований «Кнопка “Забронировать”».
- На логику функциональности бронирования — см. пункт требований «Бронь машины».
- Сделать позитивные и негативные проверки.
> На кнопке «Забронировать» указаны расстояние и время в пути. Например: «Маршрут составит 3 км и займёт 4 мин». Не нужно проверять, правильно ли рассчитаны эти данные.
> Таймер, который отсчитывает время бесплатного ожидания, тестировать также не нужно.
### Протестировать приложение
1. Протестировать функциональность каршеринга по чек-листам и тест-кейсам. Использовать только две конфигурации окружения:
- Яндекс.Браузер при разрешении экрана 800x600;
- Firefox при разрешении экрана 1920x1080.
2. Тестирование вёрстки нужно провести в обоих окружениях. Логику приложения достаточно проверить в одном окружении.
> В требованиях сказано, что после заполнения поля «Добавить права» включается таймер на 30 секунд. В текущей реализации его нет.
> Разработчики специально передали тестовый стенд: так тебе не придётся ждать каждый раз, когда выполняешь проверку.

## Часть 2
Частные авиаперевозки развиваются быстро, и нужно успеть занять нишу. Поэтому в Яндекс.Маршруты добавили новый вид транспорта — аэротакси.
Нужно протестировать эту функциональность в Яндекс.Браузере
### Постановка задачи
Чтобы ускорить разработку, фронтенд и бэкенд для аэротакси делали одновременно: фронтенд уже готов, а бэкенд задерживается. 
Предстоит протестировать реализацию на фронтенде, не дожидаясь бэкенда. Для этого будет использоваться в Charles.
1. Чек-лист 
- Если добавить новый вид транспорта в ответ сервера, он появляется в интерфейсе.
- Значок нового вида транспорта отображается в разделе видов транспорта, интерфейс не разъезжается.
- Значок нового вида транспорта переходит в выключенное состояние при смене режима со «Свой» на любой другой.
- Значок нового вида транспорта переходит в выбранное состояние при клике на него в режиме «Свой».
- Производится подсчёт стоимости поездки, если все поля заполнены корректно и выбран новый вид транспорта.
- В результатах подсчёта отображается название нового вида транспорта, которое пришло с сервера.
- В результатах подсчёта отображается стоимость и время поездки на выбранном новом виде транспорта.
2. Добавить аэротакси в интерфейс:
- Запустить Яндекс.Маршруты.
- Открыть Charles, найти в нём адрес сервера с Маршрутами → папка api → types. Здесь содержатся запросы и ответы.
- Добавить в ответ ещё одно значение:
```
{
    "id": "aero",
    "name": "Аэротакси",
    "icons": {
        "inactive": "helicopter.svg",
        "active": "helicopter-active.svg"
    }
}
```
- Перезагрузи страницу Маршрутов. Теперь на фронтенде должен отображаться новый вид транспорта.
2. Проверить, что расчёт стоимости для аэротакси отобразился в интерфейсе:
- Перехватить ответ бэкенда, в котором содержатся расчёты стоимости: api → tariffs. Логика расчётов для аэротакси такая же, как и для остальных видов транспорта.
- Подставить в ответ блок с аэротакси. Чтобы понять, куда его вставить, — изучи, как расположены блоки с расчётами для других видов транспорта в теле ответа.
```
"aero": {
    "price": 3000,
    "duration": 0.1
}
```
- На базе изменённых ответов создаnm автоматический ответ.

## Результаты тестирования в Google docs:
[Результаты тестирования](https://docs.google.com/document/d/11tgiRw0PuDDd78WIpT_tBY_wCLm9WrvoCF_2yRZdTDs/edit?usp=sharing "Google docs")
