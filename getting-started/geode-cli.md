---
title: 1.2. Geode CLI
order: 3
---

# Geode CLI

В Geode есть свой CLI, который помогает в большинстве задач при создании модов, таких как:
упаковка ассетов, генерация шрифтов, управление установленных версий SDK, и т.д. Пока это технически возможно использовать Geode без CLI, причины на это очень малы так как **он нужен практически для всего**.

# Установка

* [Windows](#windows)
* [MacOS](#macos)
* [Linux](#linux)

## Windows

### Scoop

Вы можете использовать [Scoop](https://scoop.sh/) чтобы легко установить CLI выполняя команды:
```bash
scoop bucket add extras
scoop install geode-sdk-cli
```
Позже, вы легко сможете обновить Geode CLI введя следующую команду:
```bash
scoop update geode-sdk-cli
```

### winget

Если вы захотите, вы также можете использовать [WinGet](https://learn.microsoft.com/ru-ru/windows/package-manager/winget/):
```bash
winget install GeodeSDK.GeodeCLI
```
Для обновления CLI введите это:
```bash
winget upgrade GeodeSDK.GeodeCLI
```

---

**(Не рекомендуется!)** В противном случае, вы можете самостоятельно установить Geode CLI по этому плану:
1. Скачайте последний релиз для Windows на [GitHub](https://github.com/geode-sdk/cli/releases/latest)
1. Извлеките `geode.exe` в какую нибудь папку на вашем компьютере
1. Выберите `geode.exe` в Проводнике, нажмите Shift + ПКМ и выберите `Копировать как путь`
1. Ищите `Изменение системных переменных среды` в поиске Windows. Вы также можете открыть Панель управления и поискать уже там, затем выберите `Редактировать системные переменные среды` или **сразу перейдите к шагу 6, выбрав `Переменные среды`**.
1. Нажмите на `Переменные среды...`
1. В секции `Переменные среды пользователя для (ваше_имя_пользователя)`, выберите переменную `Path` и нажмите `Редактировать`
1. Теперь нажмите `Создать`, вставьте путь 
.exe файла CLI, который вы скопировали на шаге 3. **Удалите `\geode.exe` из конца;** путь должен указывать на _папку_ с Geode CLI, а не на сам CLI.
1. Нажмите ОК чтобы закрыть окно переменных среды.

---

После установки CLI, вы теперь можете запустить `geode --version` в вашей командной строке и увидеть номер версии! (Если это не сработало, попробуйте перезапустить ваш терминал и/или компьютер.)

Рекомендуется дальнейшая [настройка профиля](#profile-setup).

## MacOS

Вы можете легко установить CLI через [Brew](https://brew.sh)
```bash
brew install geode-sdk/geode/geode-cli
```

Рекомендуется дальнейшая [настройка профиля](#profile-setup).

## Linux

We provide prebuilt Linux binaries in [the CLI releases page](https://github.com/geode-sdk/cli/releases/latest). Since Linux distros differ between each other, you need to figure out yourself how to add this binary to your global path so CMake can find it. As long as `geode --version` works anywhere, everything should be fine.

Once you figure that out, it is recommended that you [set up a profile afterwards](#profile-setup).

# Profile Setup

A profile is just an instance of Geometry Dash. The CLI allows keeping multiple separate installations of Geometry Dash at once, though most users will just have a single installation of GD with Geode on it. If you do have GDPSes with Geode on them installed, you can run `geode profile add` to add them to the list of known profiles. You need to have at least one profile set up so your mods can be automatically installed post build.

To setup a new profile, simply run the `geode config setup` command on your terminal.
