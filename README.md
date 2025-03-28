# Что это?

Это адаптер для запуска популярных конфигураций обхода замедления YouTube  
на базе [Zapret Discord Youtube - Flowseal](https://github.com/Flowseal/zapret-discord-youtube), [Zapret Discord Youtube Linux - Sergeydigl3](https://github.com/Sergeydigl3/zapret-discord-youtube-linux).  

---

# Требования
```sudo , git , grep , sed , ipfw```

# Как запустить

1. **Клонирование репозитория и запуск основного скрипта:**

   ```bash
   git clone https://github.com/Sergeydigl3/zapret-discord-youtube-linux.git
   cd zapret-discord-youtube-linux
   sudo bash main_script.sh
   ```

   Скрипт:
   - Спросит, нужно ли обновление (если папка zapret-latest уже существует).
   - Предложит выбрать стратегию из bat-файлов (например, `general.bat`, `general_mgts2.bat`, `general_alt5.bat`).  
     (При этом bat-файлы автоматически переименовываются через `rename_bat.sh`.)
   - Попросит выбрать сетевой интерфейс.

2. **Сохранение параметров:**

   Ответы можно сохранить в файле `conf.env` и потом запускать скрипт в неинтерактивном режиме:
   
   ```bash
   sudo bash main_script.sh -nointeractive
   ```
   
   Для отладки парсинга используйте флаг `-debug`.

   Пример содержимого файла `conf.env`:
   
   ```bash
   strategy=general.bat
   auto_update=false
   interface=enp0s3
   ```
   
   > **Примечание:** Если требуется автообновление, установите auto_update=true.

3. **Как посмотреть список интерфейсов:**

   ```bash
   ls /sys/class/net
   ```

---

# Важно

- Скрипт работает только с **dvtws**.
- При остановке скрипта все добавленные правила фаервола очищаются, а фоновые процессы `dvtws` останавливаются.
- Если у вас настроены кастомные правила в dvtws, сделайте их резервное копирование — скрипт может удалить их при запуске.

---

# Автозагрузка

Для настройки автозагрузки сервиса запустите скрипт:

```bash
sudo bash service.sh
```

Скрипт service.sh теперь:
- Проверяет наличие файла `conf.env` и обязательных непустых полей.
- Если конфиг отсутствует или поля пустые (например, если у вас:
  ```
  strategy=
  auto_update=
  interface=
  ```
  ), то предложит интерактивно выбрать параметры (интерфейс, стратегию из bat-файлов и автообновление).
- Создаёт systemd-сервис для автозапуска.

Просмотреть статус сервиса можно командой:

```bash
systemctl status zapret_discord_youtube.service
```

Посмотреть логи сервиса:

```bash
journalctl -u zapret_discord_youtube.service
```

Значения для автозагрузки берутся из файла `conf.env`.

---

# Совет

- **Не включайте автоапгрейд.**  
  Если репозиторий [Flowseal/zapret-discord-youtube](https://github.com/Flowseal/zapret-discord-youtube) сильно изменится, возможны проблемы из-за костыльного кода парсинга)

---

# Поддержка

- Если есть идеи по улучшению — создавайте Pull Request (например, добавить поддержку iptables).
- Если что-то не работает, создавайте Issue (пожалуйста, не пишите в личные сообщения) — так мы сможем помочь как можно большему числу пользователей.