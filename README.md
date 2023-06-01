# Лабораторная работа №7

### Цель - изучене систем автоматизации развёртывания и управления приложениями на примере Docker

Установлю **docker**: 
```
$ sudo apt update && sudo apt install -y curl
$ curl -fsSL https://get.docker.com/ | sudo sh
```
Создам файл Dockerfile:
```
FROM ubuntu:18.04 

RUN apt update 
RUN apt install -yy gcc g++ cmake
 
RUN mkdir hello_world 
COPY . hello_world/ 
WORKDIR hello_world 
RUN cmake -H. -B_build -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=_install 
RUN cmake --build _build 
RUN cmake --build _build --target install
 
ENTRYPOINT ./_build/hello_world_app 
```
Создам файл CMakeLists.txt:
```
cmake_minimum_required(VERSION 3.2) 
 
project (project_hw) 
 
add_executable(hello_world_app 
    hello_world.cpp 
) 
 
install(TARGETS hello_world_app hello_world_app RUNTIME DESTINATION /bin)

```
Создам файл hello_world:
```
#include <iostream> 
 
int main() { 
std::cout << "Hello world" << std::endl; 
}
```
Сделаю билд докера:
```
sudo docker build -t hello_world .
```
Запущу для проверки:
```
docker run hello_world
```
