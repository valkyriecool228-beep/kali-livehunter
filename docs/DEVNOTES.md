# Kali LiveHunter

Кастомный дистрибутив на базе Kali Linux (live-build), заточенный под:
- OSINT
- Pentesting / хакинг
- Приватность и анонимность
- Закрытую (open source) разработку

## Структура проекта

```
~/kali-livehunter/
├── auto/config              # Скрипт конфигурации сборки (lb config)
├── build.sh                 # Скрипт запуска сборки
├── kali-config/
│   ├── common/               # Общие конфиги Kali (не трогаем)
│   └── variant-livehunter/   # Наш кастомный вариант
│       ├── package-lists/
│       │   └── livehunter-extra.list.chroot   # Доп. пакеты
│       ├── rootfs/           # Файлы, которые копируются "как есть" в систему
│       │   ├── etc/
│       │   │   ├── os-release, lsb-release, issue   # Идентификация системы
│       │   │   ├── lightdm/lightdm-gtk-greeter.conf # Экран входа
│       │   │   ├── tor/torrc                       # Конфиг Tor
│       │   │   └── systemd/system/livehunter-tor.service
│       │   └── usr/
│       │       ├── share/backgrounds/kali-livehunter/  # Обои
│       │       ├── share/pixmaps/                      # Лого (PNG/SVG)
│       │       ├── share/plymouth/themes/kali/          # Тема загрузки (заменён логотип)
│       │       └── local/sbin/livehunter-tor-on         # Скрипт iptables-редиректа на Tor
│       └── hooks/live/
│           └── 9010-livehunter-tor-enable.hook.chroot  # Включает Tor-сервисы при сборке
└── images/                   # Готовые ISO-файлы после сборки
```

## Что уже сделано

### 1. Базовая настройка
- Создан вариант `variant-livehunter` (копия `variant-default`)
- Переименование системы: `os-release`, `lsb-release`, `issue`
- Название ISO изменено в `auto/config`:
  - `--iso-application "Kali LiveHunter"`
  - `--iso-publisher "LiveHunter Project"`
  - `--iso-volume "Kali LiveHunter"`

### 2. Брендинг
- **Обои**: `livehunter_wallpaper.png` → прописаны по умолчанию для Xfce через `/etc/skel/.config/xfce4/xfconf/.../xfce4-desktop.xml`
- **Лого**: `kali_livehunter_crest.png/svg` → скопировано в `/usr/share/pixmaps/`
- **LightDM (экран входа)**: настроен фон + лого через `lightdm-gtk-greeter.conf`
- **Plymouth (загрузочный экран)**: тема `kali` скопирована в вариант, логотип (`kali-logo.png`, `kali-logo_small.png`) заменён на герб LiveHunter (вписан по пропорциям 336×250 и 269×200 через ImageMagick). Файлы `fade`/`container`/`outline` пока оставлены оригинальными — требуют визуальной проверки после сборки.

### 3. Набор пакетов (`livehunter-extra.list.chroot`)
- **Приватность/opsec**: `torsocks`, `proxychains4`, `macchanger`, `bleachbit`, `keepassxc`, `gnupg`
- **OSINT**: `theharvester`, `spiderfoot`, `recon-ng`
- **Разработка**: `git`, `build-essential`, `python3-pip`, `python3-venv`
- **Tor**: `tor`, `iptables`, `iptables-persistent`

### 4. Transparent Tor Proxy (весь трафик через Tor по умолчанию)
- `/etc/tor/torrc` — настроен `TransPort 9040`, `DNSPort 5353`, `SocksPort 9050`
- `/usr/local/sbin/livehunter-tor-on` — скрипт iptables, перенаправляющий весь TCP/DNS трафик через Tor (кроме loopback и локальных сетей)
- `livehunter-tor.service` (systemd) — запускает скрипт при загрузке
- Hook `9010-livehunter-tor-enable.hook.chroot` — включает `tor.service` и `livehunter-tor.service` в самой системе при сборке

**Важно**: это даёт transparent proxy на уровне сети, но НЕ полную амнезию (диск не стирается между сессиями) и НЕ автоматическую MAC-рандомизацию при каждой загрузке — это отдельные задачи, если нужен уровень строгости Tails.

## Как пересобрать ISO

```bash
cd ~/kali-livehunter
sudo ./build.sh --variant livehunter --verbose
```

Полная сборка занимает 30-45+ минут в зависимости от набора пакетов.

## Известные проблемы / история

- Первая сборка (без брендинга) прошла успешно за ~32 минуты
- Вторая попытка (с Tor/pkg изменениями) упала на `dpkg-deb: error: failed to read archive` из-за повреждённого кэша `.deb`-пакетов от предыдущей прерванной сборки
- Решение: `sudo rm -rf cache chroot binary && sudo lb clean --purge` перед повторной сборкой

## TODO / возможные улучшения

- [ ] Проверить визуально Plymouth-анимацию (fade/container/outline) после сборки
- [ ] Адаптировать `outline.png` под форму герба LiveHunter (требует графредактора)
- [ ] Амнезия (не сохранять сессию на диск между перезагрузками, как Tails)
- [ ] Автоматическая рандомизация MAC-адреса при каждой загрузке
- [ ] Проверить/добавить `veracrypt` (может не быть в стандартных репах Kali)
- [ ] Кастомизация установщика (Calamares) под брендинг, если планируется offline-инсталляция
