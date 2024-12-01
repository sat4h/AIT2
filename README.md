# AIT2

## Ионов Артем группа 6231-010402D

### Report

- После настройки VS Code, появилась проблема при билде, CUDA не хотела устанавливаться, чтобы я не делал, ушло много времени на это:

![image](https://github.com/user-attachments/assets/77226828-6519-4701-8fa3-f46de08591da)

- Далее у меня появилась другая проблема уже с ubuntu, тут я подумул может просто указать другую версию и это помогло:

```
 1 warning found (use --debug to expand):
 - InvalidDefaultArgInFrom: Default value for ARG ubuntu:$ubuntu_ver results in empty or invalid base image name (line 2)
MyDocker.dockerfile:28
```

``ARG ubuntu_ver=20.04``

- Тут появилась ошибка с build_thread_count, но я вспомнил что я не задал ей значение ``export build_thread_count=1``

```

MyDocker.dockerfile:77
--------------------
  75 |     -D BUILD_EXAMPLES=ON .." && echo ${cmake_command} && cmake ${cmake_command}
  76 |     
  77 | >>> RUN make -j${build_thread_count}
  78 |     RUN make install
  79 |     RUN ldconfig
--------------------
ERROR: failed to solve: process "/bin/sh -c make -j${build_thread_count}

```


MyDocker.dockerfile:77
--------------------
  75 |     -D BUILD_EXAMPLES=ON .." && echo ${cmake_command} && cmake ${cmake_command}
  76 |     
  77 | >>> RUN make -j${build_thread_count}
  78 |     RUN make install
  79 |     RUN ldconfig
--------------------
ERROR: failed to solve: process "/bin/sh -c make -j${build_thread_count}" did not complete successfully: exit code: 2
```


