# Kali LiveHunter

Неофициальный форк [Kali Linux](https://www.kali.org/) на базе live-build,
заточенный под OSINT, тестирование на проникновение, приватность и разработку.

**Этот проект не аффилирован с Offensive Security и не является официальной
сборкой Kali Linux.**

## Возможности

- Transparent Tor Proxy — весь сетевой трафик по умолчанию через Tor
- RAM-only режим загрузки (toram) по умолчанию — минимизирует следы на диске
- Автоматическая рандомизация MAC-адреса при каждом подключении
- Набор OSINT-инструментов (theHarvester, Spiderfoot, Recon-ng)
- Кастомный интерфейс: Plymouth-тема, GTK-тема, Rofi, Conky HUD
- Мастер создания пользователя при первом запуске (zenity + polkit) + окно/команда со списком установленных инструментов (`livehunter-tools`)

## Полный список инструментов

Актуальный список — в самой системе: `/usr/share/livehunter/tools-list.txt`, или командой `livehunter-tools` в терминале.

**Recon / OSINT:** subfinder, httpx, naabu, nuclei, theHarvester, SpiderFoot, recon-ng, Photon, Sherlock, holehe
**Exploitation:** RouterSploit
**Reverse engineering:** Ghidra + GhidraMCP
**Mobile:** Android-PIN-Bruteforce
**Приватность:** Tor (systemd-сервис), torsocks, proxychains4, macchanger, BleachBit, KeepassXC, GnuPG

See [FEATURES.md](FEATURES.md) for the complete breakdown.

## Требования к железу

- **Минимум 8 ГБ RAM** — обязательно из-за RAM-only режима загрузки по умолчанию
- 64-битный процессор (amd64)
- USB-флешка от 8 ГБ для записи образа

## Установка

Скачайте последний ISO из [Releases](../../releases), проверьте контрольную сумму:

```bash
sha256sum -c SHA256SUMS
```

Запишите на флешку:
```bash
sudo dd if=kali-linux-rolling-live-livehunter-amd64.iso of=/dev/sdX bs=4M status=progress
```

## Сборка из исходников

См. [BUILDING.md](BUILDING.md).

## ⚠️ Дисклеймер по безопасности и анонимности

Этот проект **не проходил независимый security-аудит**. Инструменты вроде
Tor-проксирования, RAM-only режима и MAC-рандомизации снижают некоторые риски,
но **не гарантируют полной анонимности** и не заменяют понимание principles
opsec. Если вам нужна анонимность с более зрелой моделью угроз и историей
аудита — рассмотрите [Tails](https://tails.net/).

Используйте на свой страх и риск. Авторы не несут ответственности за
последствия использования.

## Лицензия

См. [LICENSE](LICENSE). Основано на Kali Linux (GPL и другие открытые лицензии
компонентов).
