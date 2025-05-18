# Шаблоны

Шаблоны служат для автоматизированного создания документов, основываясь на результатах выполнения различных запросов.
Для этого используется язык шаблонов [mustache](https://mustache.github.io/), который позволяет гибко и удобно
формировать структуру и содержимое документов. Данные, подставляемые в шаблоны, могут быть получены с помощью
языка запросов [JSONata](https://jsonata.org/).

Такой подход значительно упрощает процесс генерации документов, делая его более динамичным и адаптируемым под
различные задачи.

## Markdown

Пример использования markdown шаблона:
```code-frame
/docs/dochub.presentations.templates
```

Код шаблона:
```mustache
{{=<% %>=}}
Результат:
* Идентификатор документа: **{{id}}**
* Автор: **{{autor}}**
* Согласующие: {{#approvers}}**{{.}}**; {{/approvers}}
* Другие документы автора:
{{#docs}}
  * [{{title}}](@document/{{id}})
{{/docs}}
<%={{ }}=%>
```

Результат:
* Идентификатор документа: **{{id}}**
* Автор: **{{autor}}**
* Согласующие: {{#approvers}}**{{.}}**; {{/approvers}}
* Другие документы автора:
{{#docs}}
  * [{{title}}](@document/{{id}})
{{/docs}}

## PlantUML

Пример генерации PlantUML документов:
```code-frame
/docs/dochub.presentations.templates.pml
```


Код шаблона:
```mustache
{{=<% %>=}}
@startuml
title Сущности использованные при описании архитектуры
{{#entities}}
{{{.}}} {{{.}}} as {{{.}}}
{{/entities}}
@enduml
<%={{ }}=%>
```

Результат:
![PlantUML по шаблону](@document/dochub.presentations.templates.pml)

## AsyncAPI

Важным преимуществом шаблонов является возможность объединения разрозненных артефактов в единый документ.
Например, в каждом отдельном компоненте можно описать контракты, а затем собрать их в общий.

Код DocHub:
```code-frame
/docs/dochub.presentations.templates.asyncapi
```

Шаблон:
```mustache
{{=<% %>=}}
{
  "asyncapi": "2.4.0",
  "info": {
    "title": "Пример генерации асинхронных контрактов",
    "version": "1.0.0",
    "description": "Информация о контрактах собирается по всей архитектуре"
  }
  {{#content}}
    ,"{{field}}":{{&body}}
  {{/content}}
}
<%={{ }}=%>
```

Код компонентов:

```code-frame
/components/dochub.examples.orders
/components/dochub.examples.payment
```

Результат:
![AsyncAPI по шаблону](@document/dochub.presentations.templates.asyncapi)


## OpenAPI

Аналогично AsyncAPI можно собрать единый контракт для OpenAPI.

Манифест документа:
```code-frame
/docs/dochub.presentations.templates.openapi
```

Шаблон:
```mustache
{{=<% %>=}}
{
  "openapi": "3.0.0",
  "info": {
    "title": "Пример шаблона OpenAPI",
    "description": "Контракты собираются из нескольких компонентов архитетуры"
  },
  "servers": [
    {
      "url": "http://host.net",
      "description": "Сервис заказов"
    }
  ]
  {{#content}}
    ,"{{field}}":{{&body}}
  {{/content}}
}
<%={{ }}=%>
```

Манифест компонентов:
```code-frame
/components/dochub.examples.orders
/components/dochub.examples.payment
```

Результат:
![OpenAPI по шаблону](@document/dochub.presentations.templates.openapi)


