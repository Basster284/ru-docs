---
title: 3. Создание и сборка нового мода
order: 5
---

# Создание нового мода

После долгой настройки вы сможете создать свой первый мод!

Чтобы сделать это, откройте терминал там, где вы хотите создать мод и исполните:
```bash
geode new
```
Следуйте указанным инструкциям и после кучи промптов у вас должна появиться новая папка, содержащая код вашего мода.

## Файлы

Вы можете заметить, что у проекта уже есть несколько файлов. Пройдёмся по ним всем:
 * `CMakeLists.txt` - Главный файл для вашего CMake прректа.
 * `about.md` - Здесь вы можете написать очень долгое описание вашего мода, в markdown. Думайте об этом файле как о README к вашему моду! Этот файл технически необязателен, но очень рекомендуется.
 * `logo.png` - Это иконка для вашего мода, которая появляется в игре. Этот файл технически необязателен, но очень рекомендуется.
 * `mod.json` - Этот json файл содержит все метаданные о вашем моде, такие как: имя мода, версия, ресурсы, настройки и т.д. См. раздел ['конфигурация мода'](/mods/configuring)

Если вы планируете выпускать свой мод, не забудьте отредактировать about.md и logo.png!

Исходный код вашего мода находится внутри папки `src`.

## Дополнительные файлы

Geode также посмотрит эти файлы в папке мода:
 * `changelog.md` - Список всех изменений между версиями мода; См. раздел ['подробная информация'](/mods/md-files)
 * `support.md` - Файл-информация о том, как оказать поддержку разработчику мода; См. раздел ['подробная информация'](/mods/md-files)

# Сборка

Сейчас, чтобы собрать мод, у вас есть несколько вариантов:

Если вы используете IDE по типу CLion, VScode или Visual studio, см. раздел ['Настройка IDE'](/getting-started/ide-setup).

Если вы собираете мод на Android, посмотрите [секцию Android](#build-for-android).

В противном случае, если вы хотите собрать мод самостоятельно из командной строки, запустите эту команду в папке с модом:
```bash
# Настраивает & собирает для текущей платформы
geode build
```

> Запустите `geode build --help` для других опций!

Если у вас проблемы с запуском этой команды по каким-либо причинам ([дайте нам знать!](https://github.com/geode-sdk/cli/issues)), вы можете собрать мод тем же способом, используя эти команды:
```bash
# Настройка CMake
cmake -B build

# Сборка проекта
cmake --build build --config RelWithDebInfo
```

Если вы [создали профиль в CLI](/getting-started/geode-cli), собранный мод должен автоматически установлен в GD. Если нет, то собранный `ваш.мод.geode` должен быть в папке `build`, где вы можете сами установвиь моды в игру.

## Build for Android

To build mods for Android you must install the [Android NDK](https://developer.android.com/ndk/downloads). Extract it somewhere and set the `ANDROID_NDK_ROOT` enviroment variable to its path.

> On **Windows** you must also install [Ninja](https://github.com/ninja-build/ninja/releases). If you have Scoop, you can do this via `scoop install ninja`.

You must also get the Geode binaries for Android, you can do this using the CLI:
```bash
geode sdk install-binaries -p android64
# Or if you're using a 32 bit phone
geode sdk install-binaries -p android32
```

Now you can build your mod for Android via:
```bash
geode build -p android64
# Or if you're using a 32 bit phone
geode build -p android32
```

You can then copy the built .geode file from the `build-android64` folder to your phone at this location:
```
/storage/emulated/0/Android/media/com.geode.launcher/game/geode/mods/
```

## Building Windows mods on Linux

If you have followed the steps earlier and installed all the required tools with `geode sdk install-linux`, building should be as simple as on Windows:

```bash
geode build
```

If you have installed the Windows SDK and the toolchain in a different way, you will have to provide paths to them manually:

```bash
geode build -- -DCMAKE_TOOLCHAIN_FILE=/path/to/clang-msvc-sdk/clang-msvc.cmake -DSPLAT_DIR=/path/to/splat
```
