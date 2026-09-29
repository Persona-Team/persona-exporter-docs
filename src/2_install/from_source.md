# Сборка из исходников
Для того чтобы собрать проект из исходных файлов потребуется установить компилятор Rust и Git (опционально)

### Rust 
Перейдите на официальные сайт
{{#tabs}}

{{#tab name=Linux}}
Перейдите на официальный сайт или выполните:
```bash
sudo apt update
sudo apt install -y persona-exporter
```
{{#endtab}}

{{#tab name=Docker}}
Развертывание через Docker:
```bash
docker run -d --name persona-exporter persona-exporter:latest
```
{{#endtab}}

{{#tab name=Windows}}
Скачайте .exe файл и выполните команду:
```cmd
persona-exporter.exe --install
```
{{#endtab}}

{{#endtabs}}

