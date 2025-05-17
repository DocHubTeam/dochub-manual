# PlantUML

[PlantUML](https://plantuml.com/) — это мощный инструмент для создания диаграмм с помощью простого и понятного текстового языка разметки.
Он позволяет быстро и эффективно визуализировать архитектуру систем, бизнес-процессы, последовательности взаимодействий
и многое другое. Благодаря своей простоте и гибкости PlantUML широко используется архитекторами, разработчиками и
аналитиками для документирования и коммуникации сложных идей и структур.

Код PlantUML:
```
@startuml
    participant Participant as Foo
    actor       Actor       as Foo1
    boundary    Boundary    as Foo2
    control     Control     as Foo3
    entity      Entity      as Foo4
    database    Database    as Foo5
    collections Collections as Foo6
    queue       Queue       as Foo7
    Foo -> Foo1 : To actor
    Foo -> Foo2 : To boundary
    Foo -> Foo3 : To control
    Foo -> Foo4 : To entity
    Foo -> Foo5 : To database
    Foo -> Foo6 : To collections
    Foo -> Foo7 : To queue
@enduml
```

Код DocHub:
```code-frame
/docs/dochub.presentations.plantuml.example
```

Результат:
![Документ](@document/dochub.presentations.plantuml.example)

