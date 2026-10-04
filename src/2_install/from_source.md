# Сборка из исходников
Для того чтобы собрать проект из исходных файлов потребуется установить компилятор **Rust** и **Git**

### Установка Rust Lang
Перейдите на официальный сайт [Rust Programming Language](https://rust-lang.org/tools/install/)
в раздел установки и следуйте инструкциям.

### Скачивание репозитория через git
Перед началом установите саму систему контроля версий [git](https://git-scm.com/install/windows)
выберите вашу операционную систему в разделе установки и следуйте инструкциям.

После установки выполните ряд команд чтобы добавить репозиторий с исходным кодом **Persona Exporter**
```bash
git clone https://github.com/Persona-Team/persona-exporter.git

# Переходим в сам репозиторий
cd ./persona-exporter

# Меняем ветку на стабильную main
git checkout main
```

### Сборка
После того как вы установили **Rust**, **Git** и  перешли в директорию с исходным кодом выполните 
одну простую команду
```bash
cargo build --release
```
Подождите ~2 минуты (в зависимости от мощности вашего железа) после чего вы увидите что-то вроде

<pre><code><span style="color: #2ea44f; font-weight: bold;">Compiling</span> persona-exporter v0.2.0 (/home/nikita/persona-exporter)
<span style="color: #2ea44f; font-weight: bold;">Finished</span> `release` profile [optimized] target(s) in 1m 15s</code></pre>
Готовый бинарный файл будет лежать в директории ```./target/release/persona-exporter```




[//]: # ({{#tabs}})

[//]: # ()
[//]: # ({{#tab name=Linux}})

[//]: # (Перейдите на официальный сайт или выполните:)

[//]: # (```bash)

[//]: # (sudo apt update)

[//]: # (sudo apt install -y persona-exporter)

[//]: # (```)

[//]: # ({{#endtab}})

[//]: # ()
[//]: # ({{#tab name=Docker}})

[//]: # (Развертывание через Docker:)

[//]: # (```bash)

[//]: # (docker run -d --name persona-exporter persona-exporter:latest)

[//]: # (```)

[//]: # ({{#endtab}})

[//]: # ()
[//]: # ({{#tab name=Windows}})

[//]: # (Скачайте .exe файл и выполните команду:)

[//]: # (```cmd)

[//]: # (persona-exporter.exe --install)

[//]: # (```)

[//]: # ({{#endtab}})

[//]: # ()
[//]: # ({{#endtabs}})

