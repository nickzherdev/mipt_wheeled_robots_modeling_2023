Домашнее задание #3

В этом домашнем задании мы реализуем ПИД-регулятор для управления рулем автономного автомобиля.
Реализованный регулятор мы протестируем на симуляторе автономного автомобиля, 
разработанного для Udacity Self-Driving Car Nanodegree (https://github.com/udacity/self-driving-car-sim).

Для выполнения домашнего задания мы будем использовать подготовленный docker-контейнер.
В директории, в которую распакован архив соберите docker-образ:

docker build . --tag homework

Выполните команду xhost +local: на своей хост-машине для корректной работы графических приложений из контейнера.

После сборки образа вы можете запустить контейнер командой:

sudo docker run -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix --name hw3 --network host --privileged -it homework

Этой командой мы создаём контейнер с именем hw3 из собранного ранее образа с именем homework

**Внимание!** Если при запуске графических приложений из контейнера вы получаете ошибку, можно попробовать поменять в команде выше DISPLAY=unix$DISPLAY

Для запуска дополнительного терминала в контейнер, можно выяснить CONTAINER ID, выполнив команду docker ps
и найдя в выводе CONTAINER ID, необходимого контейнера. У нас имя контейнера прописано в запуске - это hw3.
Выполните команду:
	docker exec -it hw3 zsh

**Внимание!** При таком способе запуска дополнительного терминала, из него не будет доступен запуск графических приложений.

В контейнере вам необходимо перейти в директорию homework/homework3/:

cd homework/homework3/

В директории с домашним заданием содержаться две папки:
* CarND-PID-Control-Project/ - директория с исходным кодом ПИД-регулятора;
* term2_sim_linux/ - директория с симулятором беспилотного автомобиля.

Для запуска симулятора запустите файл /homework/homework3/term2_sim_linux/term2_sim.x86_64:

/homework/homework3/term2_sim_linux/term2_sim.x86_64

Выберите вкладку "Project 4: PID Controller" и нажмите Select.

В другом терминале контейнера вам необходимо запустить программу управления автомобилем с реализованным ПИД-регулятором.
Для этого перейдите в папку с исходным кодом:

======
C++
======

cd /homework/homework3/cpp/CarND-PID-Control-Project

Создайте директорию build и соберите проект:

mkdir build && cd build
cmake .. && make

Запустите регулятор (**внимание** при запуске регулятора, симулятор должен быть запущен):

./pid

После запуска регулятора машина должна поехать по прямой. Это происходит потому, что код регулятора для управления рулями пока не реализован.
Вам необходимо реализовать ПИД-регулятор на языке C++. Для этого перейдите в директорию:

cd /homework/homework3/CarND-PID-Control-Project/src

В этой директории вам необходимо модифицировать 2 файла:

* PID.cpp - реализация ПИД-регулятора;
* main.cpp - основной файл с функцией main, реализующий соединение с симулятором и вызов ПИД-регулятора.

В PID.cpp вам необходимо реализовать 3 функции:

* void PID::Init(double Kp_, double Ki_, double Kd_) - инициализация регулятора, а также его внутреннего состояния;
* void PID::UpdateError(double cte) - функция, обновляющая внутренне состояние регулятора (пропорциональную, интегральную и дифференциальную),
                                      в зависимости от ошибки поперечного смещения автомобиля от желаемой траектории (cross-track error, cte); 
* double PID::TotalError() - расчет результирующего воздействия регулятора (по формуле ПИД-регулятора из лекции), которое будет подаваться на рули, 
                             в качестве угла поворота колес.

В main.cpp вам необходимо модифицировать функцию main, в которую необходимо добавить инициализацию ПИД-регулятора, а также его использование для расчтеа steer_value -
управляющего сигнала для угла поворота колес в зависимости от текуще ошибки поперечного смещения от траектории (cross-track error, cte).

После внесения необходимых модификаций, пересоберите проект cmake .. && make из папки /homework/homework3/CarND-PID-Control-Project/build.
Запустите симулятор и регулятор, для того, чтобы увидеть обновленное управление автомобилем.
Подберите такие коэффициенты ПИД-регулятора (Kp, Ki, Kd) при которых отклонение от траектории будет минимальным. 


=========
Python 3.6 and higher
=========

cd /homework/homework3/python

Домашняя работа содержит 2 файла:
* control.py - основной файл с функцией main, реализующий соединение с симулятором и вызов ПИД-регулятора.
* pid.py - реализация ПИД-регулятора.

Реализуйте ПИД-регулятор для управления автономным автомобилем. Все инструкции приведены внутри файлов .py. Обратите внимание, что вы должны запустить скрипт control.py в другом терминале (не в том, который вы использовали для запуска симулятора).


**Внимание!** Для удобства программирования в контейнер установлена среда разработки Visual Studio Code.
Запуск Visual Studio Code работает только из терминала, для которого разрешен запуск графических приложений.
Среда Visual Studio Code может быть запущена командой:

В общем виде:
code <source directory> --user-data-dir = /vscode --no-sandbox

Например:
cd homework3
code . --user-data-dir=/vscode --no-sandbox
