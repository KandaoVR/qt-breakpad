## Kandao QtBreakpad Demo

1. This a demo to handle c++ crash with [google breakpad](https://github.com/google/breakpad) and launch a Qt app to send crash log back to server.
2. Most of the code are extracted from [Qt Creator](https://github.com/qt-creator/qt-creator) project.
3. The Demo includes a `QtBreakpadDemo` which crashes, launching the `QtCrashHandler` afterwards.

![alt text](https://raw.githubusercontent.com/KandaoVR/qt-breakpad/master/docs/QtBreakpadDemo-screenshot-win.png "QtBreakpad Demo")
    
-----
## Dependency
1. `Qt5`
2. `breakpad`'s source files are used directly. No need to compile `breakpad`


-----
## Build
### 0. clone with submodules （use recursive)
	>> git clone https://github.com/KandaoVR/qt-breakpad.git --recursive

### 1. Windows
- use CMake-Gui to generate Visual Studio Solution
- Build ALL

### 2. Mac
    >> mkdir build
    >> cd build
    >> cmake -DCMAKE_PREFIX_PATH=/path-to-qt/ -DCMAKE_INSTALL_RPATH_USE_LINK_PATH="ON" ../
    >> make -j8 install

    >> cd install/Demo
    >> ./QtBreakpadDemo

- example: cmake -DCMAKE_PREFIX_PATH=~/Qt/5.11.1/clang_64/ -DCMAKE_INSTALL_RPATH_USE_LINK_PATH="ON" ../
- reference: https://stackoverflow.com/questions/47697761/cmake-run-time-error-dyld-library-not-loaded-for-dynamically-linked-resource

### 3. Linux
- Should be working. lol
- Pull request are welcomed!

-----
- contact Neil for more information neil.szj@kandaovr.com
