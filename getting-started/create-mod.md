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

# Build

Now, to build your mod you have a few options:

If youre using an IDE such as Clion, VScode or Visual Studio, head over to the [IDE Setup](/getting-started/ide-setup) page.

If you're building for Android, check out the [Android section](#build-for-android).

Otherwise if you want to build your mods manually from the command line you can do that by simply running these commands in your mod's folder:
```bash
# Configures & builds for the current platform
geode build
```

> Check out `geode build --help` for other options!

If you have an issue running that command for whatever reason ([do let us know!](https://github.com/geode-sdk/cli/issues)), you can build your mod the same way using these commands:
```bash
# Configure CMake
cmake -B build

# Build the project
cmake --build build --config RelWithDebInfo
```

If you [created a profile in CLI](/getting-started/geode-cli), then the mod should be automatically installed to GD. If not, then the built `your.mod.geode` package should be in your build folder, from where you can manually install it in-game.

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
