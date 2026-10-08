# Конфигурация приложения

Ниже приведено полное описание всех параметров конфигурационного файла.

### Пример полного файла конфигурации

```yaml
server:
  push:
    url: "https://localhost:8086/api/v2/write"
    http_headers:
      Content-Type: "text/plain; charset=utf-8"
      Authorization: "Authorization: Token ${INFLUX_DB_TOKEN}"
    url_params:
      org: "nikita-group"
      bucket: "nikita-bucket"
      precision: "ns"
    send_interval: 10

agent:
  data_type: "line_protocol"
  send_model: "push"

metrics:
  global_tags:
    hostname: "name-your-server-please"
    env: "test-servers"
  cpu:
    enabled: true
  memory:
    enabled: true
  system:
    enabled: true
  disks:
    enabled: true
  processes:
    enabled: true
    process_limit: 5
    include_exporter_metrics: true
    remove_dead_processes: true
    sort_by: "cpu_usage"
  network:
    enabled: true
  components:
    enabled: true
```

---

# **`server`** {#server}
**Тип**: `object`

Корневой блок для настройки сетевого взаимодействия и отправки данных на удаленный сервер.

---
## **`push`** {#server-push}
**Тип**: `object`

**Полный путь**: `server.push`

Настройки модели отправки метрик (Push-модель) во внешнее хранилище.

---

### **`url`** {#server-push-url}
**Тип**: `string`

**Полный путь**: `server.push.url`

**Пример:**: 
```yaml
server:
  push:
    url: http://example.com
``` 

Полный URL-адрес эндпоинта, на который агент будет отправлять собранные метрики с помощью HTTP-запросов.

---

### **`http_headers`** {#server-push-http-headers}
**Тип**: `HashMap`

**Полный путь**: `server.push.http_headers`

**Пример**:
```yaml
server:
  push:
    http_headers:
      Content-Type: "text/plain; charset=utf-8"
      Authorization : "Authorization: Token ${INFLUX_DB_TOKEN}"
```
Произвольные HTTP-заголовки, которые будут добавлены в каждый запрос при отправке метрик. 
Используется для указания типов данных, токенов авторизации и прочего.

см. также [url_params](#server-push-url-params)

---

### **`url_params`** {#server-push-url-params}
**Тип**: `HashMap`

**Полный путь**: `server.push.url_params`

**Пример**: 
```yaml
server:
  push:
    url_params:
      # Сформирует URl вида "http(s)://myurl?org=my-org&example=true"
      org: "my-org"
      example: "true"
```
Параметры строки запроса *(Query parameters)*, которые автоматически добавляются к 
конечному URL. Вы можете жестко прописать параметры вручную в [секции URL](#server-push-url),
но указание их в отедельной секции будет считаться более идиоматичным и чистым способом.

см. также [http_headers](#server-push-http-headers)

---

### **`send_interval`** {#server-push-send-interval}
**Тип**: `interger`

**Полный путь**: `server.push.send_interval`

**По умолчанию:** `10`

Интервал отправки собранных метрик на сервер (в секундах).

---

# **`agent`** {#agent}
**Тип**: `object`

Общие настройки поведения и формата работы самого агента сбора метрик.

---

## **`data_type`** {#agent-data-type}
**Тип**: `enum (string)`

**Полный путь**: `agent.data_type`

**Допустимые значения**: `"line_protocol"`, `"json"`

**По умолчанию**: `"json"`

**Пример**:
```yaml
agent:
  data_type: "line_protocol"
```

Формат сериализации данных перед их отправкой. 

---

## **`send_model`** {#agent-send-model}
**Тип**: `enum (string)`

**Полный путь**: `agent.send_model`

**Допустимые значения**: `"push"`, `"pull"` *(в разработке)*

**По умолчанию**: `"push"`

**Пример**:
```yaml
agent:
  send_model: "push"
```
Режим распространения метрик. При значении `"push"` агент сам инициирует отправку данных на указанный `server.push.url`.

---

# **`metrics`** {#metrics}
**Тип**: `object`

Глобальный конфигурационный блок для управления собираемыми метриками и системными компонентами.

---

## **`global_tags`** {#metrics-global-tags}
**Тип**: `object`

**Полный путь**: `metrics.global_tags`

**По умолчанию**: 
```yaml
...
hostname: "name-your-server"
...
```

**Пример**: 
```yaml
metrics:
  global_tags:
    hostname: "nixos-25.11-2cpu-4ram"
    unique_id: "1"
    env: "test-servers"
    other: "readme"
```
Пользовательские теги (метки) в формате `ключ: значение`, которые будут автоматически 
добавляться ко всем отправляемым метрикам (актуально для формата `line_protocol`). 
Используются в основном для идентификации серверов и окружения на стороне СУБД.

---

## **`cpu`**  {#metrics-cpu}
**Тип**: `object`

**Полный путь**: `metrics.cpu`

**`enabled`** : `boolean` (по умолчанию: `true`) — Включает или выключает сбор метрик процессора (загрузка ядер, утилизация).

---

## **`memory`** {#metrics-memory}
**Тип**: `object`

**Полный путь**: `metrics.memory`

**`enabled`** : `boolean` (по умолчанию: `true`) — Включает или выключает сбор метрик оперативной памяти (общая, занятая, свободная, swap).

---

## **`system`** {#metrics-system}
**Тип**: `object`

**Полный путь**: `metrics.system`

**`enabled`** : `boolean` (по умолчанию: `true`) — Включает или выключает общие системные метрики (аптайм, load average).

---

## **`disks`** {#metrics-disks}
**Тип**: `object`

**Полный путь**: `metrics.disks`

**`enabled`** : `boolean` (по умолчанию: `true`) — Включает или выключает сбор информации о дисковой подсистеме (свободное место на разделах, операции ввода-вывода IOPS).

---

## **`processes`** {#metrics-processes}
**Тип**: `object`

**Полный путь**: `metrics.processes`

Конфигурация сбора детальной статистики по запущенным в системе процессам.
Является одной из самых прожорливых метрик, так как сначала нужно собрать информацию
о каждом процессе в системе *(А их может быть десятки или даже сотни)* и только потом отсортировать
и отфильтровать

---

### **`enabled`** {#metrics-processes-enabled}
**Тип**: `boolean`

**По умолчанию**: `true`

**Полный путь**: `metrics.processes.enabled`

Включает или выключает мониторинг процессов.

---

### **`process_limit`** {#metrics-processes-process-limit}
**Тип**: `integer`

**По умолчанию**: `5`

**Полный путь**: `metrics.processes.process_limit`

Максимальное количество процессов, информация о которых попадет в финальный отчет 
(исключая сам процесс экспортера, если включена соответствующая опция). 
Защищает от переполнения буфера при большом количестве процессов в ОС.

---

### **`include_exporter_metrics`** {#metrics-processes-include-exporter-metrics}
**Тип**: `boolean`

**По умолчанию**: `true`

**Полный путь**: `metrics.processes.include_exporter_metrics`

Определяет, нужно ли включать в собираемую статистику собственные метрики утилизации 
ресурсов самим экспортером.

---

### `remove_dead_processes` : `boolean` {#metrics-processes-remove-dead-processes}
* **По умолчанию:** `true`

Автоматически очищает и не отправляет данные о процессах, которые завершили свою работу 
(перешли в состояние завершенных/зомби)к моменту итерации сбора.

---

### `sort_by` : `string` {#metrics-processes-sort-by}
* **Допустимые значения:** `"cpu_usage"`, `"memory"`, `"virtual_memory"`, `"run_time"`, `"start_time"`
* **По умолчанию:** `"cpu_usage"`

Критерий, по которому сортируется список процессов перед применением ограничения `process_limit`. Позволяет выявлять топ самых «прожорливых» процессов в системе.

---

## `network` : `object` {#metrics-network}

* **`enabled`** : `boolean` (по умолчанию: `true`) — Включает или выключает сбор метрик сетевых интерфейсов (трафик, пакеты, ошибки, скорость).

---

## `components` : `object` {#metrics-components}

* **`enabled`** : `boolean` (по умолчанию: `true`) — Включает или выключает сбор метрик аппаратных компонентов (например, температура датчиков материнской платы, процессора, статус кулеров).
