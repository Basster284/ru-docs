---
title: 1.1. Необходимые инструменты C++
order: 2
---

# Необходимые инструменты C++

Чтобы использовать Geode SDK, вам **нужно** как минимум следующее:
 * [Компилятор C++](#компилятор)
 * [CMake](https://cmake.org/download/) - версия 3.29+ обязательна - убедитесь что вы добавили в PATH когда устанавливали на Windows.
 * [Git](https://git-scm.com/downloads) - Эй, ты. Да, ты! Я знаю что большинство людей пропускают этот шаг **но оно тебе надо**. Потом не приходи к нам со словами "не смог найти git для клонирования json-populate".

## Компилятор

To use the Geode SDK, and in turn make Geometry Dash mods, you will need either:
 * [Visual Studio 2022+](#windows) на Windows
 * [Clang](#macos) на MacOS
 * [Секретная третья штука](#linux) на Linux

### Windows

Download Visual Studio from [its website](https://visualstudio.microsoft.com/downloads/). If you want **just** the compiler and not the code editor, look for *Build Tools for Visual Studio* further down in the page.

After launching the installer, select **Desktop development with C++**. You may choose other features, but you **will** need at least ***MSVC*** and ***Windows SDK*** installed.

Once Visual Studio is installed, you should now have a working C++ compiler that is suited for GD mod development.

Please note that Visual Studio **2022** or higher is required. 2019 or lower will not work, as they don't support C++20 properly.

### MacOS

Install [brew](https://brew.sh/) if you don't already have it, and then run:
```bash
brew install llvm
```

### Linux

Linux is a bit more complicated, as there's no official Linux release of GD (yet). Of course, you can run the Windows version of GD through software like [wine](https://www.winehq.org/) quite well, which is probably what you're already doing.

Because of that, this guide will set you up to [cross-compile](https://en.wikipedia.org/wiki/Cross_compiler) Windows Geode mods from Linux.

First, besides Git and CMake, make sure you have `clang` and `lld` installed.

For CMake make sure you have at least version 3.29, some distributions like Debian and Linux Mint ship older versions which are not able to cross-compile Geode mods.

On Ubuntu:

```bash
apt install clang-19 clang-tools-19 lld-19
```

On Arch-based systems:

```bash
pacman -S clang lld
```

The next step will install the Windows SDK and a CMake toolchain. For ease of installation, first install [the Geode CLI](/getting-started/geode-cli.md) and then come back here. If you want to do it manually, you can follow [this guide](https://gist.github.com/matcool/abb65ee59ded3766717c673014c3a2a7).

After installing the CLI, run this command to install all the needed tools:

```bash
geode sdk install-linux
```

Now you can proceed to [setting up Geode CLI](/getting-started/geode-cli.md).
