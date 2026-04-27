---
title: Налаштування
permalink: /setup/
---

## Windows

На [сторінці завантажень GAP](http://www.gap-system.org/Releases/),
завантажте інсталятор `.exe` і двічі клацніть файл, щоб запустити його.
Коли вас запитають про шлях інсталяції, зверніть увагу, що він
не повинен містити пробілів. Наприклад, ви можете встановити GAP 4.X.Y у `C:\gap-4.X.Y`
(за замовчуванням), `D:\gap-4.X.Y` чи `C:\Math\GAP\gap-4.X.Y`, але ви не повинні
інсталювати його в такий каталог, як `C:\Program Files\gap-4.X.Y` чи
`C:\Users\alice\My Documents\gap-4.X.Y`.

## macOS

В OS X Вам потрібно інсталювати GAP із джерела, як описано
на [сторінці завантажень GAP](http://www.gap-system.org/Releases/).
Завантажте один із архівів, наданих там, розпакуйте його та запустіть
`./configure && make` у розпакованому каталозі. Потім перейдіть
до підкаталогу `pkg` і викличте `../bin/BuildPackages.sh`, щоб запустити
сценарій, який створить більшість пакетів, які потребують компіляції
(за умови наявності достатньої кількості бібліотек, заголовків і інструментів).

Alternatively, you may also install GAP using [Homebrew](https://brew.sh/).
After installing Homebrew, follow the instructions for the
[GAP Homebrew tap](https://github.com/gap-system/homebrew-gap).

## Linux

On Linux, you need to install GAP from source as explained at the
[GAP Downloads page](https://www.gap-system.org/Releases/).
Download one of the archives provided there, unpack it and run
`./configure && make` in the unpacked directory. Then change to the
`pkg` subdirectory and call `../bin/BuildPackages.sh` to run the
script which will build most of the packages that require compilation
(provided sufficiently many libraries, headers and tools are available).


