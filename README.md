# bash-deploy
---
### Зависимости
##### Скорее всего уже предустановлены
```
sudo apt-get install curl openssh-client unzip
```
##### Для подключения по паролю (требуется, если нет постоянного подключения по токену)
```
sudo apt-get install sshpass
```

### Использование
##### Возможные аргументы командной строки
```
-c || --config    путь к файлу конфигурации (def: config_path/.deployconfig)
-h || --hosts    путь к файлу с информацией о хостах (def: config_path/.hosts)
-a || --artifacts    путь к файлу с информацией об артефактах (def: config_path/.artifacts)
-s || --scenario    путь к файлу с комбинациями артефакт-хост-директория
-p    немедленный запуск сценария


config_path := ~/.config/artifacts-deploy
```
##### Формат содержания конфигурационных файлов
- Все поля в одной строке разделяются между собой \t
```
--config:
api_url  <url>
token  <token>

--hosts:
На каждой строке:
host_name  IP  port  username  password

--artifacts:
На каждой строке:
artifact_name  projectID  branch  job  file_path;file_path;...

--scenario
artifact  host  deploy_path
```
##### Формирование сценария
1. Указать url и token ("Configure API")
2. Добавить машины ("Hosts modification")
3. Добавить артефакты  ("Artifacts modification")
4. Добавить комбинации артефакт-машина-директория ("Scenario modification")
##### PS
- Никакие имена и названия, присваиваемые пользователем, не должны содержать пробелы и специальные символы
- `_`, `-` в названиях допустимы
- Любой путь, не начинающийся с `/` выполняется из $HOME
- Не нужно писать `~/` для указания $HOME директории, иначе путь будет воспринят некорректно
- Для Windows, указание абсолютного пути (с диском) должно начинаться так: /**drive_letter**/... (e.g. /c/Users/admin/...)
- Если к машине можно подключиться по ключу (не вводя его), пароль можно не указывать
- Для артефактов можно указать конкретные файлы, разделяя их `;`
- Если для одного артефакта на разные машины нужно загрузить разные файлы, придётся добавить несколько артефактов (можно использовать опцию 'copy')
- В разделах "... modification" и "Configure API" есть опция "print"
