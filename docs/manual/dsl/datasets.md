# DSL: datasets

Именованные источники данных в DocHub — это мощный инструмент для организации и управления архитектурными 
данными в репозитории. Они представляют собой специально определённые наборы данных, которые можно использовать
повторно и комбинировать для различных целей анализа и визуализации. По своей сути именованные источники
данных работают аналогично представлениям (view) в реляционных базах данных SQL: они позволяют создавать
витрины данных на основе запросов к основным данным, упрощая доступ и обработку информации.

В контексте архитектурного DataLake DocHub, именованные источники данных решают задачи структурирования,
фильтрации и агрегации архитектурных элементов, обеспечивая удобный и эффективный способ получения нужных
данных для построения отчетов, диаграмм и других артефактов. Благодаря этому подходу архитекторы могут
легко управлять сложными взаимосвязями и получать актуальную информацию без необходимости дублирования
данных или сложных манипуляций с исходными наборами.

## Предопределенные данные в источниках

Источники могут содержать предопределенные данные описанные в формате YAML или JSON.

```code-frame
/datasets/dochub.dsl.datasets.example.preset
```

Результат в виде [таблицы](@document/dochub.presentations.tables):

![Предопределенные данные в источниках](@document/dochub.dsl.datasets.examples.preset)


## Запросы к данным архитектуры

Есть возможность выполнять запросы к архитектурным данным с помощью JSONata.

Например, следующий запрос находит все архитектурные компоненты, принадлежащие DocHub, и сортирует их по
названию.

```code-frame
/datasets/dochub.dsl.datasets.example.query
```

Результат:

![Таблица на основании источника данных](@document/dochub.dsl.datasets.examples.query)

## Зависимость источника данных

Источники данных могут зависеть друг от друга. В примере данные получаются из источника "dochub.components" 
и дополнительно обрабатываются.

```code-frame
/datasets/dochub.dsl.datasets.example.dependence
```

Результат:

![Зависимый источник](@document/dochub.dsl.datasets.examples.dependence)

## Множественные зависимости

Иногда возникает необходимость консолидировать данные из нескольких источников для последующей обработки.
Это возможно сделать через структуру в origin.

```yaml
  dochub.multi_dependencies:
    origin:
      integrations: dochub.integrations             # Указываем зависимость от источника "dochub.integrations"
      criticality: dochub.components.criticality    # Указываем зависимость от источника "dochub.components.criticality"
      manifest: "($)"                               # Указывается зависимость от результата запроса JSONata - "($)"
    source: >
      (
        /* Сохраняем контекст для использования в глубине */
        $CONTEXT := $;

        /* Обрабатываем данные подготовленные источником "dochub.integrations" */
        integrations.(
            /* Восстанавливаем ID компонента, чтобы получить по нему данные */
            $ID_FROM := $reverse($split($."link-from", "/"))[0];
            $ID_TO := $reverse($split($."link-to", "/"))[0];

            /* Получаем данные критичности */
            $CRITICALLY_FROM := $lookup($CONTEXT.manifest.components, $ID_FROM).criticality;
            $CRITICALLY_FROM := $CRITICALLY_FROM ? $CRITICALLY_FROM : 4;
            $CRITICALLY_TO := $lookup($CONTEXT.manifest.components,$ID_TO).criticality;
            $CRITICALLY_TO := $CRITICALLY_TO ? $CRITICALLY_TO : 4;
            $CRITICALLY := $CRITICALLY_FROM < $CRITICALLY_TO ? $CRITICALLY_FROM : $CRITICALLY_TO;

            /* Обогащаем данные DataSet "dochub.integrations" информацией о критичности связи */
            $merge([$, {
                "criticality": $CONTEXT.criticality[id="k" & $CRITICALLY].title
            }])
        );
      )
```


