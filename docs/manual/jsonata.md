# Расширение JSONata

Проект использует язык запросов [JSONata](https://jsonata.org/) для анализа данных архитектуры.

Расширение.

Для решения специфических задач инстурумента, базовая поставка JSONata расширена дополнительными функциями
и переменными.

## Дополнительные функции 

### $mergedeep()

Сигнатура: $mergedeep(array<object>)

Объединяет несколько структур в одну. В отличии от $merge, объединение происходит на всех уровнях
вложенности, а не только на первом.

Запрос:
```json
$mergedeep([
    {
        "key1": 1,
        "struct1": {
            "key2": 1,
            "key3": 2,
            "struct2": {
                "key4": 1,
                "key5": 2
            }
        }
    },
    {
        "key1": 1,
        "struct1": {
            "key6": 1,
            "key7": 2,
            "struct2": {
                "key8": 1,
                "key9": 2
            }
        }
    }
])
```

приведет к результату:
```json
{
  "key1": 1,
  "struct1": {
    "key2": 1,
    "key3": 2,
    "struct2": {
      "key4": 1,
      "key5": 2,
      "key8": 1,
      "key9": 2
    },
    "key6": 1,
    "key7": 2
  }
}
```

В то время, как запрос:

```json
$merge([
    {
        "key1": 1,
        "struct1": {
            "key2": 1,
            "key3": 2,
            "struct2": {
                "key4": 1,
                "key5": 2
            }
        }
    },
    {
        "key1": 1,
        "struct1": {
            "key6": 1,
            "key7": 2,
            "struct2": {
                "key8": 1,
                "key9": 2
            }
        }
    }
])
```

к другому результату:
```json
{
  "key1": 1,
  "struct1": {
    "key6": 1,
    "key7": 2,
    "struct2": {
      "key8": 1,
      "key9": 2
    }
  }
}
```

### $jsonschema()

Сигнатура: $jsonschema(object)

На базе [JSON schema](https://json-schema.org/) создает валидатор, который можно использовать 
для проверки структуры данных. Реализация функции основана на NPM пакете [ajv](https://www.npmjs.com/package/ajv).

В параметре **object** передается схема. Возвращается функция-валидатор. Пример использования:

```json
(
	/* Создаем валидатор */
	$validator := $jsonschema({
      "type": "object",
      "properties": {
          "foo": { "type": "integer" },
          "bar": { "type": "string"}
      },
      "required": ["foo"],
      "additionalProperties": false
  });

  /* Проверяем структуру */
  $validator({
      "foo": "error",
      "bar": "abc"
  });
)
```

Результат:
```json
[
    {
        "instancePath": "/foo",
        "schemaPath": "#/properties/foo/type",
        "keyword": "type",
        "params": {
            "type": "integer"
        },
        "message": "должно быть integer"
    }
]
```

В фукнцию может быть передана схема с использованием $rels (см. [entities](/entities/docs/blank?dh-doc-id=dochub.flex_metamodel.entities))
```json
    /* Создаём валидатор */
    $validator := $jsonschema({
      "type": "object",
        "properties": {
            "foo": { "$ref": "#/$rels/customer.foo" },
            "bar": { "type": "string"}
        },
        "$rels": {
            "customer.foo": {
                "type": "string",
                "enum": ["foo", "dochub.foo"]
            }
        }
    });

    /* Проверяем структуру */
    $validator({
      "foo": "error",
      "bar": "abc"
    });
```

Результат:
```json
 [
    {
        "instancePath": "/foo",
        "schemaPath": "#/$rels/customer.foo/enum",
        "keyword": "enum",
        "params": {
            "allowedValues": [
                "foo",
                "dochub.foo"
            ]
        },
        "message": "must be equal to one of the allowed values"
    }
]
```

### $manifestschema()

Сигнатура: $manifestschema()

Возвращает json schema всего манифеста.

Пример использования для валидации всех объектов в озере данных:

```json
    /* получаем схему */
    $schema := $manifestschema();
        
    /* Создаём валидатор */
    $validator := $jsonschema($schema);

    /* Проверяем структуру */
    $validator($);
```

Результат, если ошибок нет:
```json
    true
```

## Дополнительные переменные

### $self

Содержит структу объекта, выполняющего запрос.

**В настоящий момент поддерживается только в [шаблонах документов](/docs/dochub.templates).**

Объект документа:
```yaml
docs:
  ...
  dochub.templates.pml: # Пример генерации PlantUML документа по шаблону
    type: PlantUML
    source: ...
    subjects:
      - dochub.front
      - dochub.front.spa
      - dochub.front.spa.blank
      - dochub.front.spa.blank.doc
    template: examples/template.puml
  ...  

```

Будет доступен при выполненнии JSONata запроса:

```json
    $self
```

в таком виде:

```json
    {
      "_id": "dochub.templates.pml",
      "type": "PlantUML",
      "source": ...,
      "subjects": [
        "dochub.front",
        "dochub.front.spa",
        "dochub.front.spa.blank",
        "dochub.front.spa.blank.doc"
      ],
      "template": "examples/template.puml"
    }
```



