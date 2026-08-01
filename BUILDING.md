# Сборка Kali LiveHunter из исходников

## Требования

- Debian/Kali Linux (родная система сборки)
- `live-build`, `debootstrap`, `git`
- 30+ ГБ свободного места на диске
- Стабильное интернет-соединение

```bash
sudo apt install -y git live-build simple-cdd curl
```

## Сборка

```bash
git clone https://github.com/ВАШ_АККАУНТ/kali-livehunter.git
cd kali-livehunter
sudo ./build.sh --variant livehunter --verbose
```

Первая сборка (полный debootstrap + все пакеты) занимает 30–60 минут.
Последующие пересборки при незначительных изменениях быстрее, если
не менялся список пакетов (`sudo lb clean --binary` перед пересборкой
ускоряет процесс).

## Тестирование

Рекомендуется тестировать собранный ISO в виртуальной машине
(VirtualBox/QEMU) перед записью на физический носитель.

## Известные особенности

- RAM-only режим (`toram`) требует минимум 6–8 ГБ RAM
- Opsec-панель в трее (genmon) требует ручного добавления через
  "Add New Items" в настройках панели XFCE — не интегрирована
  автоматически. Статус Tor/MAC/RAM-режима также доступен через
  Conky HUD на рабочем столе по умолчанию.
