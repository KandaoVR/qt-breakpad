## Kandao QtBreakpad Demo

1. This a demo to handle c++ crash with [google breakpad](https://github.com/google/breakpad) and lanch a Qt app to send crash log back to server.
2. Most of the code are extracted from [Qt Creator](https://github.com/qt-creator/qt-creator) project.
3. The Demo includes a `QtBreakpadDemo` which crashes, lauching the `QtCrashHandler` afterwards.

![]("docs/QtBreakpadDemo-screenshot-win.png")
    
-----
## Dependency
1. `Qt5`
2. `breakpad`'s source files are used directly. No need to compile `breakpad`


-----
## Build
### 1. Windows
- use CMake-Gui to generate Visual Studio Solution
- Build ALL

### 2. Mac
    >> mkdir build
    >> cd build
    >> cmake ../
    >> make install

    >> cd install/Demo
    >> ./QtBreakpadDemo

### 3. Linux
- Should be working. lol
- Pull request are welcomed!

-----
- contact Neil for more information neil.szj@kandaovr.com


    /* This is a code block */