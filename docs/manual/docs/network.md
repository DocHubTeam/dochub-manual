# Network

Сетевые диаграммы позволяют наглядно представлять топологию различных связей на основании данных архитектуры.
Для визуализации сетей используется библиотека [vis.js](https://visjs.org/).

Данными для построения сети являются константные данные в поле source, [источник данных](/docs/dochub.datasets) или результат запроса JSONata.

Итоговым результатом должна стать структура в соответсвии с [документацией vis.js](https://visjs.github.io/vis-network/docs/network/) :
```json
    {
        "nodes": [ /* Перечисление нод https://visjs.github.io/vis-network/docs/network/nodes.html */
            {
               "id": "node1",           /* Идентификатор ноды */
               "label": "Node 1",       /* Описание */
               ...
            },
            {
               "id": "node2",           
               "label": "Node 2",
               ...
            }
        ],
        "edges": [                       /* Перечисление вязей нод https://visjs.github.io/vis-network/docs/network/edges.html */
            {
                "id": "link1",           /* Идентификатор связи */
                "from": "node1",         /* Источник */
                "to": "node2",           /* Приемник */
                "label": "Пример связи", /* Описание */
                ...
            }
        ],
        "options": {                     /* Параметры рендеринга диаграммы https://visjs.github.io/vis-network/docs/network/ */
            "clickToUse": false,         /* Отключаем обязательный клик перед взаимодействем с пользователем */
            ...
        }
    }
```

Пример описания сетевой диаграммы:

```code-frame
/docs/dochub.presentations.network.example
```

Пример рендеринга документа:

![Документ](@document/dochub.presentations.network.example)

