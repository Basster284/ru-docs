---
title: 3.1. Настройка IDE
order: 6
---

# Настройка IDE

Вы можете использовать один из приведённых IDE для разработки модов на Geode. Использование IDE крайне рекомендуется, ибо оно даёт подказки к коду, intellisense и в прицнипе упрощает разработку модов. Мы рекомендуем VS Code, для которого мы имеем [отдельное расширение](https://marketplace.visualstudio.com/items?itemName=GeodeSDK.geode).

* [Visual Studio Code](#visual-studio-code)
* [Visual Studio](#visual-studio)
* [CLion](#clion)

# Visual Studio Code

* [VS Code на Windows](#vs-code-на-windows)
* [VS Code на Mac](#vs-code-на-mac)
* [VS Code на Linux](#vs-code-на-linux)

## VS Code на Windows

Для VS Code, мы рекомендуем несколько расширений:
 * [`C/C++`](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
 * [`CMake Tools`](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)
 * [`Geode`](https://marketplace.visualstudio.com/items?itemName=GeodeSDK.geode) - Необязательно, но имеет кучу крутых фишек.
Выполните несколько шагов, чтобы правильно настроить intellisense и завершение кода (в идеале это делается один раз на один проект) 

1. В VS Code откройте ваш проект, нажмите F1 и запустите `CMake: Выберите набор`. Это покажет список установленных на компьютере компиляторов.

![Картинка, на которой куча компиляторов, обнаруженных в VS Code](/assets/win_compilers.png)

Выберите компилятор **Visual Studio 2022+** (лучше `amd64` версию) или Clang, ничто другое!

> :warning: Это КРИТИЧЕСКИ важно

2. Выберите вариант сборки, нажмите F1 и запустите `CMake: Выбор варианта`. Рекомендуем **RelWithDebInfo** для облегчения отладки, as Debug may sometimes cause obscure crashes.

![Картинка, на которой видно типы сборок на Windows: Debug, Release, MinSizeRel, and RelWithDebInfo](/assets/win_relwithdebinfo.png)

3. Идентифицируйте CMake для расширения C++ как **Поставщик конфигурации**, нажав F1 и запустив `C/C++: Изменить конфигурации (пользовательский интерфейс)`. Пролистайте до раздела **Дополнительные параметры**, и в поле с загаловком "Поставщик конфигурации" вставьте `ms-vscode.cmake-tools`.

![Картинка, на которой запущена команда в VS Code "C/C++: Изменить конфигурации (пользовательский интерфейс)"](/assets/win_usecmake.png)

Прямо сейчас, соберите ваш мод нажав F1 и выбрав `CMake: Сборка`. **Вы должны сначала собрать мод, чтобы не было ошибок по типу `#include <Geode/modify/MenuLayer.hpp> not found`.** Если мод был успешно собран, код выхода должен быть 0. Если всё ещё есть какие-то ошибки после сборки мода, перезапустите VS Code.

## VS Code на Mac

Для VS Code на Mac, мы рекомендуем несколько расширений:
 * [`clangd`](https://marketplace.visualstudio.com/items?itemName=llvm-vs-code-extensions.vscode-clangd)
 * [`CMake Tools`](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools)
 * [`Geode`](https://marketplace.visualstudio.com/items?itemName=GeodeSDK.geode) - Необязательно, но имеет кучу крутых фишек.

Выполните несколько шагов, чтобы правильно настроить intellisense и завершение кода (в идеале это делается один раз на один проект):

1. С открытоым проектом в VS Code, нажмите F1 и запустите `CMake: Выбор варианта`. Выберите **Clang**.

Прямо сейчас, соберите ваш мод нажав F1 и выбрав `CMake: Сборка`. **Вы должны сначала собрать мод, чтобы не было ошибок по типу `#include <Geode/modify/MenuLayer.hpp> not found`.** Если мод был успешно собран, код выхода должен быть 0. Если всё ещё есть какие-то ошибки после сборки мода, перезапустите VS Code.

## VS Code на Linux

Процесс настройки очень похож на настройку [VS Code на Mac](#vs-code-на-mac), так что вы можете следовать тем же шагам. Главная разница в том, что вам самому нужно создать набор, что можно сделать так:

1. Откройте Палитру команд (F1 или Ctrl + Shift + P) и запустите **CMake: Edit User-Local CMake Kits** (требует расширение CMake Tools и открытый в редакторе CMake проект)

2. Добавьте новый набор в список:

```json
{
    "name": "Geode Windows 64-bit",
    "compilers": {
        "C": "/usr/bin/clang",
        "CXX": "/usr/bin/clang++"
    },
    "isTrusted": true,
    "toolchainFile": "${userHome}/.local/share/Geode/cross-tools/clang-msvc-sdk/clang-msvc.cmake",
    "cmakeSettings": {
        "HOST_ARCH": "x86_64",
        "SPLAT_DIR": "${userHome}/.local/share/Geode/cross-tools/splat",
        "CMAKE_EXPORT_COMPILE_COMMANDS": 1
    }
}
```

Если набор инструментов и/или splat не были установлены не через `geode sdk install-linux`, измените пути на соответственные.

А если у вас установлен Ninja то вы можете использовать его вместо `make`, так как он быстрее и использует несколько ядер процессора, в отличии от `make`. Добавьте в JSON это:

```json
"preferredGenerator": {
    "name": "Ninja"
}
```

Теперь вы можете выбрать набор и собирать моды!

Кроме того, что не повлияет на 90% игроков, если вы используете SIMD-интринсики или библиотеку из-за которой у вас появляются ошибки при компиляции, смените `clang` на `clang-cl`:

* `"toolchainFile"` на `${userHome}/.local/share/Geode/cross-tools/clang-msvc-sdk/clang-cl-msvc.cmake`
* `"compilers"` / `"C"` на `/usr/bin/clang-cl`
* `"compilers"` / `"CXX"` на `/usr/bin/clang-cl`

# Visual Studio

Рекомендуется иметь опыт в Visual Studio перед тем как вы начнёте пользоваться ей, но если у вас опыта не было, ничего страшного.

Современная Visual Studio может управлять проектом CMake самостоятельно. Убедившись, что у вашего VS есть поддержка CMake, откройте папку с модом. Вы поймёте что всё работает, когда консоль появится снизу и начнёт получать информацию с CMake.
Перед сборкой мода, измените эти настройки (их надо изменять каждый раз при создании нового мода):

1. Нажмите на "Конфигурация" (это раскрывающееся меню в верху вашего экрана, на котором написано "x64-Debug")
2. Нажмите "Управление конфигурациями..." в раскрывшемся меню

![Картинка, на которой нарисована кнопка "Управление конфигурациями..." в раскрывшемся меню](/assets/vs_manage_configurations.png)

3. Измените тип конфигурации на `Release` или `RelWithDebInfo`. Мы рекомендуем `RelWithDebInfo`, так как он облегчает отладку. **Вы не можете использовать `Debug`!**
4. Убедитесь что у набора инструментов выбран `msvc_x86`
5. По желанию поменяйте название конфига на что-нибудь дружелюбное по типу "deafult", "release" или подобное
6. Ну и Ctrl + S прожмите чтобы изменения сохранить
Пример рабочей конфигурации:

![Картинка с рабочим конфигом](/assets/vs_example_config.png)

Как захотите сделать мод, жмите F7 или Ctrl + B (Если комбинации клавиш не работают, нажмите на Сборка сверху, а затем Собрать или Собрать всё)

Если встречаются ошибки как на VS Code (по типу `#include <Geode/modify/MenuLayer.hpp> not found`) после сборки, перезапустите Visual Studio.

# CLion

* [CLion на Windows](#clion-на-windows)
* [CLion на Linux](#clion-на-linux)

## CLion на Windows

Для CLion нужно никаких плагинов - всё что нужно это правильно настроить набор инструментов. Когда вы открываете папку с модом в первый раз, вы встретите так называемого "Open Project Wizard":

![Картинка с открытым "Open Project Wizard"](/assets/clion_openprojectwizard.png)

Если вы  запустили CLion в первый раз, нажмите на кнопку "Manage toolchains...", которая откроет окно где вы можете добавить новый набор инструментов.

Пример как настроить набор инструментов Visual Studio:

![Картинка с настроенным набором инструментов Visual Studio в CLion](/assets/clion_toolchainmsvc.png)

И как настроить набор инструментов Clang:

![Картинка с настроенным набором инструментов Clang в CLion](/assets/clion_toolchainllvm.png)

После этого, в Open Project Wizard, убедитесь что:

1. Тип сборки установлен на `Release` или `RelWithDebInfo` (мы рекомендуем `RelWithDebInfo`, ибо с `Debug` могут быть проблемы)
2. Набор инструментов это либо *Visual Studio*, либо *Clang/LLVM*
3. Генератор это *Visual Studio 17 2022* или *Ninja* (если он установлен)

В конце концов всё должно выглядеть так:  

![Картинка с настроенным Open Project Wizard для Geode](/assets/clion_openprojectwizardsetup.png)

Жмите OK и CMake запустится в первый раз. Подождите, пока он закончит и вы увидете раскрывающееся меню в правом углу экрана на котором написано `NameOfTheMod` (или как там вы его назвали).
Нажмите на него, и выберите `NameOfTheMod_PACKAGE`, что начнёт шаги пост-сборки и установит моды в Geometry Dash.

![Картинка с раскрывающимся меню настройки в CLion](/assets/clion_addconfiguration.png)

Нажмите на иконку молота в правом углу рядом со сборкой мода:

![Картинка с кнопокй сборки в CLion](/assets/clion_buildmod.png)

Сборка должна завершиться с надписью `Build finished` и (убедившись что вы запускали `geode config setup` раннее) моддолжен автоматически установиться в Geometry Dash.

Не забудьте перезагрузить CMake как только вы добавите новые файлы в проект.

![Картинка на которой нарисована кнопки перезагрузки CMake в CLion](/assets/clion_reloadcmake.png)

## CLion на Linux

Точно также как и на [Windows](#clion-на-windows) за исключением настройки набора инструментов.

Сперва создадите файл окружения со следующим содержимым:

```bash
export GEODE_SDK=$HOME/Documents/Geode
export SPLAT_DIR=$HOME/.local/share/Geode/cross-tools/splat
export CMAKE_TOOLCHAIN_FILE=$HOME/.local/share/Geode/cross-tools/clang-msvc-sdk/clang-msvc.cmake
export HOST_ARCH=x86_64
```

Если набор инструментов и/или splat не были установлены через `geode sdk install-linux`, измените пути.

Сохраните как `geode-env.sh` в вашей домашней директории (или где угодно, просто запомните путь).

> :warning: Если используете нестандартную оболочку (по типу fish), вам нужно настроить файл окружения для правильного синтаксиса. К примеру, fish использует `set -gx VAR value` вместо `export VAR=value`.

Теперь, в настройке набора инструментов в правой части с полем **Name**, нажмите *Add environment* и выберите файл, который вы только что создали.

![Картинка на которой показано, как настраивать кросс-компилятор в CLion](/assets/clion_toolchainlinux.png)

После этого, вы можете следовать тем же шагам что и на Windows для сборки мода.

Для некоторых модов/библиотек (большинству людей это не надо), вы может захотите настроить  набор инструментов clang-cl. Всё то же самое как с clang, за исключением что вам надо использовать файл набора инструментов `clang-cl-msvc.cmake`. (Скопируйте файл окружения и измените значение переменной `CMAKE_TOOLCHAIN_FILE` на расположение файла `clang-cl-msvc.cmake`.)  
Также убедитесь что вы используете `clang-cl` в качестве компилятора в настройках набора инструментов.