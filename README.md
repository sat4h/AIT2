# AIT2

## Ионов Артем группа 6231-010402D

### Report

- После настройки VS Code, появилась проблема при билде, CUDA, что-бы я не делал, не хотела устанавливаться:

![image](https://github.com/user-attachments/assets/77226828-6519-4701-8fa3-f46de08591da)

- Далее у меня появилась другая проблема уже с ubuntu, тут я подумул может просто указать другую версию и это помогло:

```
 1 warning found (use --debug to expand):
 - InvalidDefaultArgInFrom: Default value for ARG ubuntu:$ubuntu_ver results in empty or invalid base image name (line 2)
MyDocker.dockerfile:28
```

``ARG ubuntu_ver=20.04``

- Тут появилась ошибка с build_thread_count, я вспомнил что я не задал ей значение ``export build_thread_count=1``,но это не решило проблемы.
```
  75 |     -D BUILD_EXAMPLES=ON .." && echo ${cmake_command} && cmake ${cmake_command}
  76 |     
  77 | >>> RUN make -j${build_thread_count}
  78 |     RUN make install
  79 |     RUN ldconfig

ERROR: failed to solve: process "/bin/sh -c make -j${build_thread_count}
```

- Проблемы были с конфигурацией numpy, после добавления следующих строк все исправилось:

```
# Очистка кеша перед запуском CMake
RUN rm -rf /usr/local/Dev/opencv-${ocv_ver}/build/*

# CMake конфигурация для сборки OpenCV
RUN pip3 install numpy
```

- Значение у ``export build_thread_count=10`` вернул к 10.

Билд закончен
![image](https://github.com/user-attachments/assets/0622b1f8-7f48-41b9-8f72-1750065c707a)



Обновленный [Docker файл](MyDocker). Основные изменения:

- В изначальном докер файле используется ubuntu:22.04 (позже задается переменной ubuntu_ver).
Во втором файле используется ubuntu:20.04.
- Убраны переменные:
```
ARG cuda_ver
ARG cuda_distro
ARG cuda_arch
RUN apt -y install linux-headers-$(uname -r) build-essential
RUN apt-key del 7fa2af80
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/$cuda_distro/$cuda_arch/cuda-keyring_1.1-1_all.deb
RUN dpkg -i cuda-keyring_1.1-1_all.deb
RUN apt update
RUN apt install cuda-$cuda_ver
```
- И отключена CUDA конфигурация: ``-D WITH_CUDA=OFF``
