# DSL: $base

$base — это свойство доступное в [презентациях](@document/dochub.presentations), которое указывает базовый
путь к **данным** в архитектурном Data Lake, от которого разрешаются относительные пути к **файлам**. Оно
помогает точно определить, откуда брать файлы, особенно когда декларация структуры склеена из нескольких
файлов с использованием [imports](@document/dochub.dsl.imports).

## Пример применения

В DocHub ссылки на файлы указываются в структурах. Например, в документе может быть ссылка вида:
```yaml
docs:
  example:
    type: markdown
    title: Пример относительного пути файла документа
    source: readme.md   # Относительная ссылка на файл данных для документа
```

Для точного ответа на вопрос "Где расположен файл readme.md?" необходимо определить путь к файлу, в
котором содержится код декларации документа. Однако существует дополнительная сложность — DocHub поддерживает
склейку структур, и документ может иметь сложную, распределённую декларацию.

Например, первоначально документ декларируется в файле `docs/root.yaml`:
```yaml
docs:
  example:
    type: markdown
    title: Пример относительного пути файла документа
    template: template.md   # Шаблон документа
    source: data.json       # Данные для шаблона документа
```

А затем, документ частично переопределяется в файле `/dochub.yaml` с указанием другого источника данных:
```yaml
imports:
  - docs/root.yaml
docs:
  example:
    source: other_data.json
```

В результате склеивания декларации документа возникнет две относительные ссылки.
* ссылка на файл `template.md`, который расположен в `/docs/template.md`;
* ссылка на файл `other_data.json`, который расположен в `/other_data.json`.

Чтобы презентация документа прошла успешно, необходимо точно определить файл, в котором объявлено конкретное
свойство документа. Для этого в DocHub используется свойство $base, указывающее точку в Data Lake,
от которой разрешаются относительные пути.

Чтобы сообщить DocHub, где искать `template.md` необходимо определить $base как:
```yaml
  ...
  $base: /docs/example/template
  ...
```

В этом случае DocHub определит файл, в котором объявлено свойство "template", и использует его как основу для
разрешения относительных путей.

Пример использования $base в [конструкторе](@document/dochub.dsl.constructor) документов:
```code-frame
/entities/docs/presentations/blank
```

