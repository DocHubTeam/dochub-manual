# DSL: $prototype

**Устарело. Используйте [$constructor](@document/dochub.dsl.constructor)**

Функционал наследования полезен, когда необходимо частично или полностью переиспользовать 
описание объекта. Например, можно создать описание шаблона таблицы и использовать его
наполняя необходимыми данными. 

Код прототипа:
```code-frame
/docs/dochub.dsl.prototype
```

Код наследника:
```code-frame
/docs/dochub.dsl.prototype.examples.child
```

Результат:
![Унаследованная таблица](@document/dochub.dsl.prototype.examples.child)

**Наследование доступно для только для однотипных объектов.**



