**JSON** (JavaScript Object Notation) — открытый текстовый формат для обмена данными. Формат разработан [Дугласом Крокфордом](https://ru.wikipedia.org/wiki/%D0%9A%D1%80%D0%BE%D0%BA%D1%84%D0%BE%D1%80%D0%B4,_%D0%94%D1%83%D0%B3%D0%BB%D0%B0%D1%81). Отличительная особенность формата — его легко интерпретировать не только машинам, но и людям.

JSON хорошо читается людьми и прекрасно решает задачу передачи объектов, основанных на наборе из пар ключ-значение. Подробные сведения о формате доступны в документе [RFC 4627](http://tools.ietf.org/html/rfc4627).

Формат JSON берёт начало из JavaScript (вспомните аббревиатуру), но фактически не зависит от языка программирования. Это означает, что применять формат JSON можно и в других языках программирования. PHP, C#, Go, Python и многие другие языки программирования поддерживают JSON по умолчанию (нативно) или с помощью вспомогательных библиотек.

За счёт универсальности, формат JSON упрощает обмен данными. Не важно, какие технологии используются на клиенте или сервере. Они смогут договориться за счёт универсального формата.

JSON не единственный формат для обмена данными. Есть более сложные форматы. Например, [XML](https://ru.wikipedia.org/wiki/XML). Этот формат долгое время был стандартом при обмене данными, но в итоге JSON вытеснил его из многих сценариев за счёт своей лаконичности.

## Синтаксис
Если вы освоили массивы и объекты в JavaScript, то с чтением JSON сложностей не возникнет. Посмотрите и попробуйте разобрать следующий JSON:
```json
{
  "first_name": "Ivan",
  "last_name": "Sidorov"
}
```
Ничего не напоминает? Всё верно, выглядит как описание объекта в JavaScript. На самом деле это текст в формате JSON. Внешне он похож на описание объекта в JavaScript с помощью литерала, но на самом деле это не объект. Обычная строка, которую без проблем можно передать куда угодно. Например, с клиента на сервер.

Содержимое JSON — это коллекция из пар ключ/значение. Ключи определяются слева, а значение отделяется символом двоеточия (```:```). Синтаксически это выглядит так, как если описать обычный JavaScript-объект.

Важная особенность, что ключи должны быть обрамлены в двойные кавычки. Пожалуй, это главное отличие от описания объектов в JavaScript, где кавычки необязательны при описании ключей. Если их не поставить в JSON, то он будет считаться невалидным и его не смогут разобрать парсеры.

## Типы данных в JSON
Мы рассмотрели самый простой пример, где описали JSON-документ с помощью коллекции из пар ключ/значение. Все значения были строками (они взяты в кавычки), но этим формат JSON не ограничивается.

Начнём с главного. В формате JSON вам доступны две структуры данных — коллекция из пар ключ/значение и упорядоченный список значений. Не пугайтесь слов «структура данных». Это понятие мы обязательно обсудим позже. Пока просто запомните, что в данном контексте под ним подразумеваются объекты (коллекция из пар ключ/значение) и массивы (упорядоченный список значений) из JavaScript.

Почему здесь акцентировано внимание именно на JavaScript? Дело в том, что перечисленные коллекции могут по-разному быть реализованы в других языках программирования. В одних языках эти структуры реализуются с помощью объектов и массивов, в других с помощью списков и структур, в третьих по-другому и так далее. Нас интересует JavaScript, поэтому под коллекцией «ключ/значение» будем подразумевать объекты, а под упорядоченным списком — массивы.

Значение в формате JSON может быть одним из шести типов: объект, строка, массив, число, булево и null. Других типов значений JSON содержать не может. Рассмотрим пример:
```json
{
  "band": "Bon Jovi",
  "title": "Slippery when wet",
  "year": 1986,
  "tracks": [
    "Let it rock",
    "You give love a bad name",
    "Livin'on a prayer",
    "Social Disease"
  ],
  "members": [
    {
      "name": "Jon Bon Jovi",
      "role": "singer",
      "active": true
    },
    {
      "name": "Richie Sambora",
      "role": "guitar player",
      "active": false
    }
  ],
  "relatedGroups": null
}
```
В примере выше приведён текст в формате JSON с описанием альбома музыкальной группы. Для демонстрации в нём задействованы все типы данных, поддерживаемые стандартом JSON. Разберём в деталях.

Строковые значения должны быть взяты в двойные кавычки. Здесь мало, что отличается от JavaScript. Единственное — кавычки должны быть двойными. Числа (```year```), булевы значения (```active```) и значения ```null``` пишутся как есть. Без каких-либо символов.

Обратите внимание: в примере используются вложенные структуры. Ключ ```members``` и ```tracks``` — это массивы значений. Внутри массива могут быть любые значения, поддерживаемые в JSON. Мы привели пример со строковыми значениями и объектами.

Начинаться JSON документ не обязан с объекта (как в нашем примере). Может начинаться с массива. Например, сервер возвращает информацию о всех альбомах музыкального коллектива:
```json
[
  {
    "band": "Bon Jovi",
    "title": "Slippery when wet"
  },
  {
    "band": "Bon Jovi",
    "title": "Have a nice day"
  }
]
```

## Оптимизация размера JSON
Представленный выше пример хорош с точки зрения читабельности. Структура прекрасно читается. Однако, это не означает, что JSON обязан быть отбит отступами. Для оптимизации отступы, лишние пробелы можно удалить. Таким образом, JSON превратится в длинную строку, но размер передаваемых данных сократится. Такой подход часто применяется на практике.

## Валидация JSON
К JSON предъявляется несколько требований. Мы уже про них говорили: допустимые типы данных, двойные кавычки для строк, отсутствие висящих запятых (помните об этом, это одна из самых частых ошибок) и так далее. Чтобы убедиться, что подготовленный JSON-документ валиден, есть специальные сервисы, расширения для редакторов и пакеты в NPM.

Для экспериментов можем порекомендовать сервис [JSON Formatter & Validator](https://jsonformatter.curiousconcept.com/). Он позволяет не только проверить JSON на корректность, но и отформатировать его — привести в читабельный вид.

Последняя возможность пригодится, когда вам потребуется разобраться с JSON, получаемым от сервера. Повторимся, зачастую его подвергают минификации, что не позволяет комфортно читать.

## Из JSON и обратно
Для работы с JSON в JavaScript есть одноимённый встроенный объект [```JSON```](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/JSON). Он предоставляет два метода: ```stringify``` (для получения JSON строки, соответствующей переданному значению) и ```parse``` (для разбора JSON строки и получению значения).

Испробуем методы в работе. Все перечисленные примеры вы сможете повторить, воспользовавшись консолью в инструментах разработчиков. Сначала рассмотрим конвертацию JSON-строки в объект. В качестве тестовых данных воспользуемся ранее приведённым примером:
```javascript
const albumJSON = `{
    "band": "Bon Jovi",
    "title": "Slippery when wet",
    "year": 1986,
    "tracks": [
        "Let it rock",
        "You give love a bad name",
        "Livin'on a prayer",
        "Social Disease"
    ]
}`;

// Преобразуем JSON объект в объект
const albumObject = JSON.parse(albumJSON);

// Получим объект
// {band: "Bon Jovi", title: "Slippery when wet", 
//  year: 1986, tracks: Array(4) }
console.log(albumObject);
```
В примере текст в формате JSON сохранён в переменную ```albumJSON```. Обратите внимание на кавычки. Это именно строка, а не литерал объекта. Затем в переменную ```albumObject``` присваиваем результат вызова JSON.parse(). Аргументом этот метод принимает строку в формате JSON, а результатом станет объект, полученный в ходе преобразования.

Обратная процедура выполняется с помощью метода ```stringify```. Рассмотрим на ещё одном примере:
```javascript
const user = {
    firstName: 'Keks',
    type: 'cat',
    favorites: [
        'milk',
        'meat',
    ],
}

// Получим JSON-строку
const jsonText = JSON.stringify(user);

// {"firstName":"Keks","type":"cat","favorites":["milk","meat"]}
console.log(jsonText);
```
Для преобразования (сериализации) объекта ```user``` в JSON мы воспользовались методом ```JSON.stringify()```. Аргументом передали объект, а результатом стала строка — текст в формате JSON.

Методы ```stringify``` и ```parse``` могут принимать дополнительные аргументы для настройки преобразования. Получить информацию по ним вы сможете из [MDN](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/JSON).

## Резюме
JSON — лаконичный формат для обмена данными. Его легко читать машинам и людям. Сегодня формат используется повсеместно при обмене данными бекенда с фронтендом.