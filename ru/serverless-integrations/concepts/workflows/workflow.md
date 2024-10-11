# Рабочий процесс

_Рабочий процесс_ — событийно-ориентированное приложение, состоящее из _шагов_ и переходов между ними. Атрибуты шага:
* входные данные;
* набор действий, которые нужно сделать во время выполнения шага;
* таймаут выполнения;
* выходные данные;
* политика повторных попыток, которая применяется, если шаг завершился с ошибкой. Если шаг не может быть выполнен из-за исчерпания квоты или его выполнение завершилось с ошибкой, {{ sw-name }} будет пытаться повторно выполнить шаг согласно политике повторных попыток. В итоге шаг может выполниться успешно или завершиться с ошибкой, если последняя попытка запуска окажется неуспешной. При завершении шага с ошибкой весь [запуск](execution.md) переходит в статус `Ошибка` и все дальнейшие шаги не выполняются.

Шаги описываются в [спецификации YaWL](yawl.md) и делятся на две группы — интеграционные и управляющие.

В настройках рабочего процесса можно указать:
* пользовательскую сеть, в которой он будет выполняться. Рабочий процесс при этом будет иметь доступ к ресурсам в этой сети.
* сервисный аккаунт, который будет использоваться для получения доступа к закрытым ресурсам, например потокам данных {{ yds-full-name }}.

Рабочий процесс можно запустить с помощью консоли управления, API или {{ er-name }}. Подробнее см. [{#T}](execution.md).

## Состояние рабочего процесса {#state}

_Состояние рабочего процесса_ — это JSON-объект. В начале запуска рабочего процесса в состоянии хранятся только входные данные, которые передал пользователь. Во время выполнения запуска:
* состояние рабочего процесса передается в качестве входных данных (`input`) в шаг;
* выходные данные (`output`) шагов добавляются в состояние рабочего процесса в том порядке, в котором выполняются шаги.

Используя jq-выражения, можно фильтровать:
* состояние рабочего процесса, которое передается в качестве входных данных (`input`) в шаг;
* выходные данные (`output`), которые добавляются в состояние рабочего процесса.

{% note info %}

Состояние рабочего процесса в каждый момент времени является JSON-объектом.

{% endnote %}

Выходные данные каждого шага добавляются в состояние рабочего процесса путем слияния их с текущим состоянием по верхнеуровневым ключам. Поэтому возможна перезапись части данных состояния. Например, если перед запуском шага состояние было JSON-объектом вида `{“numbers“: [1,2,3,4], “strings”: [“a”, “b”, “c”]}`, а выходные данные шага — `{“strings”: [“d”, “e”]}`, новым состоянием станет JSON-объект вида `{“numbers“: [1,2,3,4], “strings”: [“d”, “e”]}`.

Результат выполнения рабочего процесса — отфильтрованные выходные данные последнего шага, если для него задано поле `output`. Иначе — полное состояние всего рабочего процесса.

### Состояние рабочего процесса при выполнении шага Parallel {#state-for-Parallel}

Для шага [Parallel](yawl.md#Parallel) состояние рабочего процесса копируется в каждую ветку и в рамках каждой ветки изменяется независимо. Выходными данными шага Parallel до фильтрации является объект, где ключи — уникальные имена веток выполнения, а значения — выходные данные последних шагов соответствующих веток.

### Состояние рабочего процесса при выполнении шага Foreach {#state-for-Foreach}

После фильтрации входных данных шаг [Foreach](yawl.md#Foreach) принимает на вход массив JSON-объектов. После выполнения последовательности шагов в `do` для каждого элемента списка в результате формируется массив JSON-объектов. Это значит, что для шага Foreach поля `input` и `output` обязательны:
* `input` — для преобразования состояния из JSON-объекта в массив;
* `output` — для преобразования результата выполнения шага из массива в JSON-объект, который можно добавить в состояние рабочего процесса.

## См. также

* [Пример спецификации YaWL](yawl.md#spec-example)