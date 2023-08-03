---
layout: multipage-overview
title: Контекстные абстракции
scala3: true
partof: scala3-book
overview-name: "Scala 3 — Book"
type: chapter
description: В этой главе представлено введение в концепцию контекстных абстракций Scala 3.
language: ru
num: 59
previous-page: types-others
next-page: ca-extension-methods
---

## Предпосылка

Контекстные абстракции — это способ абстрагироваться от контекста.
Они представляют собой единую парадигму с большим разнообразием вариантов использования, среди которых:

- реализация тайп классов (_type classes_)
- установление контекста
- внедрение зависимости (_dependency injection_)
- выражение возможностей
- вычисление новых типов и доказательство взаимосвязей между ними

В этом отношении Scala оказала влияние на другие языки. Например, трейты в Rust или protocol extensions Swift.
Предложения по дизайну также представлены для Kotlin в качестве разрешения зависимостей во время компиляции,
для C# в качестве Shapes и Extensions или для F# в качестве Traits.
Контекстные абстракции также являются общей особенностью средств доказательства теорем, таких как Coq или Agda.

Несмотря на то, что в этих проектах используется разная терминология,
все они являются вариантами основной идеи вывода терминов (term inference):
учитывая тип, компилятор синтезирует "канонический" термин, который имеет этот тип.

## Редизайн в Scala 3

В Scala 2 контекстные абстракции поддерживаются пометкой `implicit` определений (методов и значений) или параметров
(см. [Параметры контекста]({% link _overviews/scala3-book/ca-context-parameters.md %})).

Scala 3 включает в себя переработку контекстных абстракций.
Хотя эти концепции постепенно "открывались" в Scala 2, теперь они хорошо известны и понятны, и редизайн использует эти знания.

Дизайн Scala 3 фокусируется на **намерении**, а не на **механизме**.
Вместо того, чтобы предлагать одну очень мощную функцию имплицитов,
Scala 3 предлагает несколько функций, ориентированных на варианты использования:

- **Расширение классов задним числом**.
  В Scala 2 методы расширения должны были кодироваться с использованием [неявных преобразований][implicit-conversions] или [неявных классов]({% link _overviews/core/implicit-classes.md %}).
  Напротив, в Scala 3 [методы расширения][extension-methods] теперь встроены непосредственно в язык, что приводит к улучшению сообщений об ошибках и улучшению вывода типов.

- **Абстрагирование контекстной информации**.
  [Предложения Using][givens] позволяют программистам абстрагироваться от информации,
  которая доступна в контексте вызова и должна передаваться неявно.
  В качестве улучшения по сравнению со Scala 2 подразумевается, что предложения using могут быть указаны по типу,
  освобождая сигнатуры функций от имен переменных, на которые никогда не ссылаются явно.

- **Предоставление экземпляров тайп-классов**.
  [Given экземпляры][givens] позволяют программистам определять _каноническое значение_ определенного типа.
  Это делает программирование с [тайп-классами][type-classes] более простым без утечек деталей реализации.

- **Неявное преобразование одного типа в другой**.
  Неявное преобразование было [переработано с нуля][implicit-conversions] как экземпляры тайп-класса `Conversion`.

- **Контекстные абстракции высшего порядка**.
  _Совершенно новая_ функция [контекстных функций][contextual-functions] делает контекстные абстракции объектами первого класса.
  Они являются важным инструментом для авторов библиотек и позволяют выражать лаконичный DSL.

- **Полезная обратная связь от компилятора**.
  Если компилятор не может разрешить неявный параметр, теперь он предлагает [предложения по импорту](https://www.scala-lang.org/blog/2020/05/05/scala-3-import-suggestions.html), которые могут решить проблему.

## Преимущества

Эти изменения в Scala 3 обеспечивают лучшее разделение вывода терминов от остального языка:

- существует единственный способ определить данные
- существует единственный способ ввести неявные параметры и аргументы
- существует отдельный способ [импорта givens][given-imports], который не позволяет им прятаться в море обычного импорта
- существует единственный способ определить [неявное преобразование][implicit-conversions], которое четко обозначено как таковое и не требует специального синтаксиса

К преимуществам этих изменений относятся:

- новый дизайн позволяет избежать взаимодействия функций и делает язык более согласованным
- implicits становятся более легкими для изучения и более сложными для злоупотреблений
- значительно улучшается ясность 95% программ Scala, использующих implicits
- есть потенциал, чтобы сделать вывод термов однозначным способом, который также доступен и удобен.

В этой главе в следующих разделах представлены многие из этих новых функций.

[givens]: {% link _overviews/scala3-book/ca-context-parameters.md %}
[given-imports]: {% link _overviews/scala3-book/ca-given-imports.md %}
[implicit-conversions]: {% link _overviews/scala3-book/ca-implicit-conversions.md %}
[extension-methods]: {% link _overviews/scala3-book/ca-extension-methods.md %}
[context-bounds]: {% link _overviews/scala3-book/ca-context-bounds.md %}
[type-classes]: {% link _overviews/scala3-book/ca-type-classes.md %}
[equality]: {% link _overviews/scala3-book/ca-multiversal-equality.md %}
[contextual-functions]: {{ site.scala3ref }}/contextual/context-functions.html