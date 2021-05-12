## Laboratory work III


Данная лабораторная работа посвещена изучению систем автоматизации сборки проекта на примере **CMake**



## Tasks

- [x] 1. Создать публичный репозиторий с названием **lab03** на сервисе **GitHub**
- [x] 2. Ознакомиться со ссылками учебного материала
- [x] 3. Выполнить инструкцию учебного материала
- [x] 4. Составить отчет и отправить ссылку личным сообщением в **Telegram**


## Homework

** для того, чтобы закончить работу быстрее файлы были "запушины ручками" через гитхаб. поэтому нет самой папки silver_application, потому что она много весит для пушинься с певого раза. но прикреплен полученный файл silver. **

Представьте, что вы стажер в компании "Formatter Inc.".
### Задание 1
Вам поручили перейти на систему автоматизированной сборки **CMake**.
Исходные файлы находятся в директории [formatter_lib](formatter_lib).
В этой директории находятся файлы для статической библиотеки *formatter*.
Создайте `CMakeList.txt` в директории [formatter_lib](formatter_lib),
с помощью которого можно будет собирать статическую библиотеку *formatter*.

```sh
git clone https://github.com/tp-labs/lab03
создаем CMakeLists.txt 

cat > CMakeLists.txt <<EOF
> cmake_minimum_required(VERSION 3.4) //минимально необходимая версия для работы файлов
> add_library(formatter STATIC formatter.h formatter.cpp) //статическая библиотека из файлов
> EOF

cmake ~/bogdan1/workspace/tasks/lab03/formatter_lib
make //создаем библиотеку
создался файл formatter.a
```


### Задание 2
У компании "Formatter Inc." есть перспективная библиотека,
которая является расширением предыдущей библиотеки. Т.к. вы уже овладели
навыком созданием `CMakeList.txt` для статической библиотеки *formatter*, ваш 
руководитель поручает заняться созданием `CMakeList.txt` для библиотеки 
*formatter_ex*, которая в свою очередь использует библиотеку *formatter*.
```sh
cp -r formatter_lib formatter_ex_lib/


cat > CMakeLists.txt <<EOF
>cmake_minimum_required(VERSION 3.4) //минимально необходимая версия для работы файлов
>project(formatter_ex)
>include_directories(formatter_lib) //дир-ий с заголовочными файлами
>add_subdirectory(formatter_lib) // подключили дир-рий с библиотекой
>add_library(formatter_ex STATIC formatter_ex.h formatter_ex.cpp) //делаем статическую библиотеку из файлов
>target_link_libraries(formatter_ex formatter) //для компиляции formatter_ex используем formatter
```

### Задание 3
Конечно же ваша компания предоставляет примеры использования своих библиотек.
Чтобы продемонстрировать как работать с библиотекой *formatter_ex*,
вам необходимо создать два `CMakeList.txt` для двух простых приложений:
* *hello_world*, которое использует библиотеку *formatter_ex*;
* *solver*, приложение которое испольует статические библиотеки *formatter_ex* и *solver_lib*.

```sh
1).

mv formatter_ex_lib/ hello_world_application/

cat > CMakeLists.txt <<EOF
>cmake_minimum_required(VERSION 3.4)
>project(hello_world)
>include_directories(formatter_ex_lib)
>add_subdirectory(formatter_ex_lib)
>add_executable(hello_world hello_world.cpp)
>target_link_libraries(hello_world formatter_ex)
>EOF
cmake 
~/bogdan1/workspace/tasks/lab03/hello_world_application$ cmake ~/bogan1/workspace/tasks/lab03/hello_world_application
-- Configuring done
-- Generating done
-- Build files have been written to: /home/bogdan/bogdan1/workspace/tasks/lab03/hello_world_application
make
в hello_world_application появился файл hello_world
```

```sh
2).
создаем cmake's для solver_lib и solver_application
заполянем solver_lib:
cmake_minimum_required(VERSION 3.4) 
add_library(solver_lib STATIC solver.h solver.cpp)

заполняем solver_application:
cmake_minimum_required(VERSION 3.4)
project(solver)
add_executable(solver equation.cpp)
include_directories(formatter_ex_lib)
add_subdirectory(formatter_ex_lib)
include_directories(solver_lib)
add_subdirectory(solver_lib)
target_link_libraries(solver formatter_ex)
target_link_libraries(solver solver_lib)
все АНАЛОГИЧНО пунктам выше!!
Повторяем команды: сборка, запуск.
Но!! 
При использовании команды make понял, что в файле solver.cpp
есть ошибки. исправляю команду sqrt, добавил cmath
И ВСЕ ЗаРаБОТало
появился файл solver
```

