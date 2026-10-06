# Конфигурация
Расположение файла конфигурации экспортера зависит от вашей операцинной системы
{{#tabs}}

{{#tab name=Linux}}
```xpath2
/etc/persona-exporter/config.yaml
```
{{#endtab}}

{{#tab name=Windows}}
```xpath2
C:\ProgramData\PersonaMetrics\PersonaExporter\config.yaml
```
{{#endtab}}

{{#endtabs}}

Как вы могли заметить формат файла конфигурации **.yaml** что полезно так как в файле используется
большая вложенность параметров. По умолчанию *(при условии что вы запустили экспортер от имени
**суперпользователя**)* все директории создадутся сами и в конфиг запишется шаблон для **InfluxDB**

```yaml
server:
  push:
    url: "https://localhost:8086/api/v2/write"
    http_headers:
      Content-Type: "text/plain; charset=utf-8"
      Authorization: "Authorization: Token ${INFLUX_DB_TOKEN}"
    # Your url params, out: https://example.com?example=true&user_id=3
    url_params:
      org: "nikita-group"
      bucket: "nikita-bucket"
      precision: "ns"
    # Metric sending interval in seconds
    send_interval: 10

agent:
  # "line_protocol" "json"
  data_type: "line_protocol"
  # "push" / "pull" (soon)
  send_model: "push"

metrics:
  # For Line Protocol
  global_tags:
    # variable_name: value
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
    # Maximum size of the process list (excluding information about the exporter itself)
    process_limit: 5
    include_exporter_metrics: true
    remove_dead_processes: true
    # "cpu_usage" / "memory" / "virtual_memory" / "run_time" / "start_time"
    sort_by: "cpu_usage"
  network:
    enabled: true
  components:
    enabled: true

```

Также, как вы могли заметить в блоке http-заголовках
```yaml
Authorization: "Authorization: Token ${INFLUX_DB_TOKEN}"
```
Используется конструкция **${INFLUX_DB_TOKEN}**, где **INFLUX_DB_TOKEN** - это переменная
окружения вашей операционной системы / текущей сессии. Это полезно, к примеру, для 
секретных токенов авторизации которые лучше не "хард-кодить" прямо в файле конфигурации.

