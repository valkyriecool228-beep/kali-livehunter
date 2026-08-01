# Kali LiveHunter — полный список возможностей

Сводный документ по всему, что входит в вариант `livehunter`. Источник истины по пакетам — `kali-config/variant-livehunter/package-lists/`, по дополнительным инструментам — `kali-config/variant-livehunter/hooks/live/`.

## Брендинг и оформление

- Собственные обои (`usr/share/backgrounds/kali-livehunter/`)
- Собственное лого (`.png` / `.svg`)
- Кастомная Plymouth-тема загрузочного экрана
- Изменённые `/etc/os-release`, `/etc/issue`, `/etc/lsb-release`
- Кастомная тема LightDM greeter

## Приватность / анонимность / OPSEC

| Компонент | Назначение |
|---|---|
| `tor` (systemd-сервис `livehunter-tor`) | Анонимизация трафика, автозапуск при загрузке |
| `torsocks`, `proxychains4` | Проксирование трафика приложений через Tor |
| `macchanger` | Рандомизация MAC-адреса |
| `bleachbit` | Очистка следов активности |
| `keepassxc` | Менеджер паролей |
| `gnupg` | Шифрование и цифровые подписи |
| `iptables`, `iptables-persistent` | Transparent Tor-proxy, файрвол |

## Recon / OSINT

| Инструмент | Назначение |
|---|---|
| `theharvester` | Сбор email/поддоменов из открытых источников |
| `spiderfoot` | Автоматизированный OSINT-фреймворк |
| `recon-ng` | Модульный recon-фреймворк |
| `subfinder` | Поиск поддоменов (Go, ProjectDiscovery) |
| `httpx` | HTTP-пробинг (Go, ProjectDiscovery) |
| `naabu` | Сканер портов (Go, ProjectDiscovery) |
| `nuclei` | Сканер уязвимостей по шаблонам (Go, ProjectDiscovery) |
| `photon` | Веб-краулер для OSINT |
| `sherlock` | Поиск username в соцсетях |
| `holehe` | Проверка email на регистрации в сервисах |

## Эксплуатация

| Инструмент | Путь | Назначение |
|---|---|---|
| RouterSploit | `/opt/routersploit` | Фреймворк эксплойтов для роутеров/IoT |

## Реверс-инжиниринг

| Инструмент | Путь | Назначение |
|---|---|---|
| Ghidra | системный пакет | Дизассемблер/декомпилятор |
| GhidraMCP | `/opt/ghidra-mcp` | MCP-интеграция для Ghidra |

## Mobile / Android

| Инструмент | Путь | Назначение |
|---|---|---|
| Android-PIN-Bruteforce | `/opt/android-pin-bruteforce` | Брутфорс PIN-кода Android |

## Крипто / собственные инструменты

| Инструмент | Путь | Назначение |
|---|---|---|
| `cryptsetup`, `age` | системные пакеты | Шифрование дисков/файлов |
| DracoLure | `/opt/dracolure` | — |
| RS-Key | `/opt/rs-key` | Прошивка аппаратного security-key |

## Сетевой анализ

- `wireshark` — анализатор трафика (GUI)
- `tshark` — анализатор трафика (CLI)

## Безопасность системы

- `selinux-basics`, `selinux-policy-default`, `policycoreutils`, `setools`

## Разработка

- `git`, `build-essential`, `python3-pip`, `python3-venv`
- `golang-go` — для сборки Go-based тулов
- `cargo`, `rustc` — для сборки RS-Key
- `vim`, `neovim`, `tmux`, `docker.io`

## Первый запуск / UX

- **Мастер создания пользователя** (`/usr/local/bin/livehunter-user-wizard`) — zenity-диалог логина/пароля, вызывает root-хелпер через `pkexec`
- **Root-хелпер** `/usr/local/sbin/livehunter-create-user` — создаёт пользователя (`useradd` + `chpasswd`, группа `sudo`)
- **Polkit policy** `org.livehunter.create-user.policy` — разрешает вызов хелпера через `pkexec`
- Пакеты `polkitd`, `pkexec` — стек авторизации (в текущем kali-rolling заменяют устаревший `policykit-1`)
- **Список инструментов**: единый источник `/usr/share/livehunter/tools-list.txt`
- `livehunter-tools` — просмотр списка в терминале
- `livehunter-tools-gui` — просмотр списка в графическом окне (zenity)
- Автозапуск мастера при первом входе (`/etc/skel/.config/autostart/`)
- Ярлык `livehunter-tools.desktop` на рабочем столе

## Базовая платформа

- `kali-linux-core`, `kali-desktop-live`, `kali-desktop-xfce`, `kali-linux-default`

---

## ⚠️ Ответственное использование

Часть инструментов в этом списке (RouterSploit, Android-PIN-Bruteforce, sherlock, holehe, nuclei и др.) предназначена для тестирования на проникновение и OSINT-разведки. Используйте их только на системах/аккаунтах, на которые у вас есть явное разрешение владельца, и в рамках действующего законодательства. См. также раздел «Дисклеймер» в [README.md](README.md).
