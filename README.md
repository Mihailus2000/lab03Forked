Пол:1 http://mirror-1.truenetwork.ru/kali kali-rolling/main amd64 tree amd64 1.8.0-1 [49,3 kB]
Получено 49,3 kB за 2с (27,9 kB/s)
Выбор ранее не выбранного пакета tree.
(Чтение базы данных … на данный момент установлено 441793 файла и каталога.)
Подготовка к распаковке …/tree_1.8.0-1_amd64.deb …
Распаковывается tree (1.8.0-1) …
Настраивается пакет tree (1.8.0-1) …
Обрабатываются триггеры для man-db (2.8.5-2) …## Laboratory work III

Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**

```ShellSession
$ open https://cmake.org/
```

## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [ ] 2. Ознакомиться со ссылками учебного материала
- [ ] 3. Выполнить инструкцию учебного материала
- [ ] 4. Составить отчет и отправить ссылку личным сообщением в **Slack**

## Tutorial

```ShellSession
$ export GITHUB_USERNAME=<имя_пользователя>
```

```ShellSession
$ cd ${GITHUB_USERNAME}/workspace
$ pushd .

~/Mihailus2000/workspace ~/Mihailus2000/workspace

$ source scripts/activate
```

```ShellSession
$ git clone https://github.com/${GITHUB_USERNAME}/lab02.git projects/lab03
$ cd projects/lab03
$ git remote remove origin
$ git remote add origin https://github.com/${GITHUB_USERNAME}/lab03.git
```

```ShellSession
$ g++ -std=c++11 -I./include -c sources/print.cpp
$ ls print.o

print.o

$ nm print.o | grep print

0000000000000095 t _GLOBAL__sub_I__Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000000 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSo
0000000000000026 T _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E

$ ar rvs print.a print.o

ar: создаётся print.a
a - print.o

$ file print.a

print.a: current ar archive

$ g++ -std=c++11 -I./include -c examples/example1.cpp
$ ls example1.o

example1.o

$ g++ example1.o print.a -o example1
$ ./example1 && echo

hello

```

```ShellSession
$ g++ -std=c++11 -I./include -c examples/example2.cpp
$ nm example2.o

                 U __cxa_atexit
                 U __dso_handle
0000000000000000 V DW.ref.__gxx_personality_v0
                 U _GLOBAL_OFFSET_TABLE_
0000000000000128 t _GLOBAL__sub_I_main
                 U __gxx_personality_v0
0000000000000000 T main
                 U _Unwind_Resume
00000000000000df t _Z41__static_initialization_and_destruction_0ii
                 U _Z5printRKNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEERSt14basic_ofstreamIcS2_E
                 U _ZNSaIcEC1Ev
                 U _ZNSaIcED1Ev
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEEC1EPKcSt13_Ios_Openmode
                 U _ZNSt14basic_ofstreamIcSt11char_traitsIcEED1Ev
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_
                 U _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEED1Ev
                 U _ZNSt8ios_base4InitC1Ev
                 U _ZNSt8ios_base4InitD1Ev
0000000000000000 r _ZStL19piecewise_construct
0000000000000000 b _ZStL8__ioinit

$ g++ example2.o print.a -o example2
$ ./example2
$ cat log.txt && echo

hello

```

```ShellSession
$ rm -rf example1.o example2.o print.o
$ rm -rf print.a
$ rm -rf example1 example2
$ rm -rf log.txt
```

```ShellSession
$ cat > CMakeLists.txt <<EOF
cmake_minimum_required(VERSION 3.4)
project(print)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
add_library(print STATIC \${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp) 
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF
include_directories(\${CMAKE_CURRENT_SOURCE_DIR}/include)
EOF
```

```ShellSession
$ cmake -H. -B_build

-- The C compiler identification is GNU 8.3.0
-- The CXX compiler identification is GNU 8.3.0
-- Check for working C compiler: /usr/bin/cc
-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/mic/Mihailus2000/workspace/projects/lab03/_build

$ cmake --build _build

Scanning dependencies of target print
[ 50%] Building CXX object CMakeFiles/print.dir/sources/print.cpp.o
[100%] Linking CXX static library libprint.a
[100%] Built target print

```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

add_executable(example1 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example1.cpp)     # CMAKE_CURRENT_SOURCE_DIR - путь до project
add_executable(example2 \${CMAKE_CURRENT_SOURCE_DIR}/examples/example2.cpp)
EOF
```

```ShellSession
$ cat >> CMakeLists.txt <<EOF

target_link_libraries(example1 print)
target_link_libraries(example2 print)
EOF
```

```ShellSession
$ cmake --build _build

-- Configuring done
-- Generating done
-- Build files have been written to: /home/mic/Mihailus2000/workspace/projects/lab03/_build
[ 33%] Built target print
Scanning dependencies of target example2
[ 50%] Building CXX object CMakeFiles/example2.dir/examples/example2.cpp.o
[ 66%] Linking CXX executable example2
[ 66%] Built target example2
Scanning dependencies of target example1
[ 83%] Building CXX object CMakeFiles/example1.dir/examples/example1.cpp.o
[100%] Linking CXX executable example1
[100%] Built target example1

$ cmake --build _build --target print       #указываем что собрать: статические библиотеки или ex1/ex2

[100%] Built target print

$ cmake --build _build --target example1

[ 50%] Built target print
[100%] Built target example1

$ cmake --build _build --target example2

[ 50%] Built target print
[100%] Built target example2

```

```ShellSession
$ ls -la _build/libprint.a

-rw-r--r-- 1 mic mic 3118 апр 29 02:34 _build/libprint.a

$ _build/example1 && echo

hello

$ _build/example2
$ cat log.txt && echo

hello

$ rm -rf log.txt
```
Получаем только CmakeLists из github
```ShellSession
$ git clone https://github.com/tp-labs/lab03 tmp

Клонирование в «tmp»…
remote: Enumerating objects: 70, done.
remote: Total 70 (delta 0), reused 0 (delta 0), pack-reused 70
Распаковка объектов: 100% (70/70), готово.

$ mv -f tmp/CMakeLists.txt .
$ rm -rf tmp
```

```ShellSession
$ cat CMakeLists.txt

cmake_minimum_required(VERSION 3.0)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_EXAMPLES "Build examples" OFF)

project(print)

add_library(print STATIC ${CMAKE_CURRENT_SOURCE_DIR}/sources/print.cpp)

target_include_directories(print PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

if(BUILD_EXAMPLES)
  file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
  foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_SOURCE})
    target_link_libraries(${EXAMPLE_NAME} print)
    install(TARGETS ${EXAMPLE_NAME}
      RUNTIME DESTINATION bin
    )
  endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()

install(TARGETS print
    EXPORT print-config
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)
install(EXPORT print-config DESTINATION cmake)

$ cmake -H. -B_build -DCMAKE_INSTALL_PREFIX=_install

-- Configuring done
-- Generating done
-- Build files have been written to: /home/mic/Mihailus2000/workspace/projects/lab03/_build

$ cmake --build _build --target install

Install the project...
-- Install configuration: ""
-- Installing: /home/mic/Mihailus2000/workspace/projects/lab03/_install/lib/libprint.a
-- Installing: /home/mic/Mihailus2000/workspace/projects/lab03/_install/include
-- Installing: /home/mic/Mihailus2000/workspace/projects/lab03/_install/include/print.hpp
-- Installing: /home/mic/Mihailus2000/workspace/projects/lab03/_install/cmake/print-config.cmake
-- Installing: /home/mic/Mihailus2000/workspace/projects/lab03/_install/cmake/print-config-noconfig.cmake

$ tree _install

_install
├── cmake
│   ├── print-config.cmake
│   └── print-config-noconfig.cmake
├── include
│   └── print.hpp
└── lib
    └── libprint.a

3 directories, 4 files


```

```ShellSession
$ git add CMakeLists.txt
$ git commit -m"added CMakeLists.txt"

[master fe8595f] added CMakeLists.txt
 1 file changed, 36 insertions(+)
 create mode 100644 CMakeLists.txt

$ git push origin master

Username for 'https://github.com': Mihailus2000
Password for 'https://Mihailus2000@github.com': 
Перечисление объектов: 33, готово.
Подсчет объектов: 100% (33/33), готово.
При сжатии изменений используется до 4 потоков
Сжатие объектов: 100% (28/28), готово.
Запись объектов: 100% (33/33), 10.88 KiB | 1.81 MiB/s, готово.
Всего 33 (изменения 9), повторно использовано 0 (изменения 0)
remote: Resolving deltas: 100% (9/9), done.
To https://github.com/Mihailus2000/lab03.git
 * [new branch]      master -> master

```

## Report

```ShellSession
$ popd

~/Mihailus2000/workspace

$ export LAB_NUMBER=03
$ git clone https://github.com/tp-labs/lab${LAB_NUMBER} tasks/lab${LAB_NUMBER}

Клонирование в «tasks/lab03»…
remote: Enumerating objects: 70, done.
remote: Total 70 (delta 0), reused 0 (delta 0), pack-reused 70
Распаковка объектов: 100% (70/70), готово.

$ mkdir reports/lab${LAB_NUMBER}
$ cp tasks/lab${LAB_NUMBER}/README.md reports/lab${LAB_NUMBER}/REPORT.md
$ cd reports/lab${LAB_NUMBER}
$ edit REPORT.md
$ gistup -m "lab${LAB_NUMBER}"
```

## Homework

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в папке [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

**Удачной стажировки!**

## Links
- [Основы сборки проектов на С/C++ при помощи CMake](https://eax.me/cmake/)
- [CMake Tutorial](http://neerc.ifmo.ru/wiki/index.php?title=CMake_Tutorial)
- [C++ Tutorial - make & CMake](https://www.bogotobogo.com/cplusplus/make.php)
- [Autotools](http://www.gnu.org/software/automake/manual/html_node/Autotools-Introduction.html)
- [CMake](https://cgold.readthedocs.io/en/latest/index.html)

```
Copyright (c) 2015-2019 The ISC Authors
```
