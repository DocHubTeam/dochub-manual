# Table

Документ предназначен для отображеия данных в табличной форме.

## Предопределенный набор данных

```code-frame
/docs/dochub.presentations.tables.examples.preset
```

Результат:
![Предопределенная таблица](@document/dochub.presentations.tables.examples.preset)

## Запрос к данным архитектуры

Есть возможность выполнять запросы к данным архитектуры. Например, получить список всех интеграций:

```code-frame
/docs/dochub.presentations.tables.examples.select
```

Результат:
![Запрос к данным архитектуры](@document/dochub.presentations.tables.examples.select)

## Хранение запросов в отдельном файле

Располагать запрос непосредственно в манифесте не всегде хорошее решение. Манифест целиком загружается в память. 
Тыжелый запрос создаст излишнюю нагрузку на ОЗУ. Старайтесь тяжелые запросы выделять в отдельные файлы. В этом случае
запрос будет загружаться только при необходимости.

```code-frame
/docs/dochub.presentations.tables.examples.ex_jsonata
```

Результат:

![Предопределенная таблица](@document/dochub.presentations.tables.examples.ex_jsonata)

## Хранение данных в отдельном файле

Вы также можете размещать тяжелые данные в отдельном файле YAML или JSON.

```code-frame
/docs/dochub.presentations.tables.examples.ex_yaml
```

Содержимое файла "autors.yaml"

```yaml
- name: Иванов Иван Иванович
- name: Петров Петр Сергеевич
- name: Сидоров Евгений Вальдемарович
```

Результат:

![Предопределенная таблица](@document/dochub.presentations.tables.examples.ex_yaml)


## Внешние данные

В некоторых случаях полезно иметь возможность получать внешние данные и сопоставлять их с данными архитектуры.
Вы можете сделать это указав в качестве источника данных URI.

```code-frame
/docs/dochub.presentations.tables.examples.ex_http
```

Содержимое файла "showcases.jsonata"

```jsonata
[
    $.{
        "title": name
    }
]^(title)
```

Результат:

![Предопределенная таблица](@document/dochub.presentations.tables.examples.ex_http)


## Генерация таблицы на основании источника данных

Табличные документы могут ссылаться на [источники данных](/docs/dochub.datasets).

```code-frame
/docs/dochub.presentations.tables.examples.dataset
```

Результат:
![Предопределенная таблица](@document/dochub.presentations.tables.examples.dataset)

Источники позволяют многократно переиспользовать себя. Это дает возможность не дублировать запросы в разных документах.

## Обработка данных

Перед визуализацией таблицы есть возможность подвергнуть данные обработке.
Для примера, возьмем за основу источник данных "dochub.docs". Его необходимо указать
в поле "origin". В поле "source" нужно определить JSONata запрос. В примере отбираются документы 
располагающиеся в меню "/DocHub/Презентации/".

```code-frame
/docs/dochub.presentations.tables.examples.post
```

Результат:
![Предопределенная таблица](@document/dochub.presentations.tables.examples.post)
