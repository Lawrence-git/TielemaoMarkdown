# progress - Coreutils Progress Viewer

## What is it?

This tool can be described as a **Tiny**, Dirty, Linux-and-OSX-Only C command that looks for coreutils basic commands (cp, mv, dd, tar, gzip/gunzip, cat, etc.) currently running on your system and displays the **percentage** of copied data. It can also show **estimated time** and **throughput**, and provides a "top-like" mode (monitoring).

[![progress screenshot with cp and mv](https://camo.githubusercontent.com/14048573a12e356a41c84a7ba8001db708d2113e/68747470733a2f2f7261772e6769746875622e636f6d2f5866656e6e65632f70726f67726573732f6d61737465722f636170747572652e706e67)](https://camo.githubusercontent.com/14048573a12e356a41c84a7ba8001db708d2113e/68747470733a2f2f7261772e6769746875622e636f6d2f5866656e6e65632f70726f67726573732f6d61737465722f636170747572652e706e67)

_(After many requests: the colors in the shell come from [powerline-shell](https://github.com/milkbikis/powerline-shell). Try it, it's cool.)_

Formerly known as cv (Coreutils Viewer).

## [](https://github.com/Xfennec/progress#how-do-you-build-it)How do you build it?

```
make && make install

```

It depends on library ncurses, you may have to install corresponding packages (may be something like 'libncurses5-dev' or 'ncurses-devel').

## [](https://github.com/Xfennec/progress#how-do-you-run-it)How do you run it?

Just launch the binary, `progress`.

## [](https://github.com/Xfennec/progress#what-can-i-do-with-it)What can I do with it?

A few examples. You can:

*   monitor all current and upcoming instances of coreutils commands in a simple window:

    ```
      watch progress -q

    ```

*   see how your download is progressing:

    ```
      watch progress -wc firefox

    ```

*   look at your Web server activity:

    ```
      progress -c httpd

    ```

*   launch and monitor any heavy command using `$!`:

    ```
      cp bigfile newfile & progress -mp $!

    ```

and much more.

## [](https://github.com/Xfennec/progress#how-does-it-work)How does it work?

It simply scans `/proc` for interesting commands, and then looks at directories `fd` and `fdinfo` to find opened files and seek positions, and reports status for the largest file.

It's very light, and compatible with virtually any command.