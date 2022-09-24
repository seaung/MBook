##### 进程号

```c
#include <sys/types.h>
#include <unistd.h>

pid_t getpid(void); // 返回当前进程的id号
pid_t getppid(void); // 返回当前进程的父进程的id号
```

##### 进程复制

fork函数执行一次返回两次，父进程返回的是子进程的id,子进程返回0

```c
#include <sys/types.h>
#include <unistd.h>

pid_t fork(void); 复制进程
```

system函数，调用shell的外部命令在当前的进程中开始另一个进程(阻塞当前进程直到命令执行完毕)

成功返回0,失败返回-1,当shell不能执行时，返回127

```c
int system(const char * command);
```

exec函数族

```c
#include <unistd.h>
extern char **environ;
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ..., char * const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execve(const char *filename, char *const argv[], char *const envp[]);
```

##### fork,system和exec的区别

1. fork,system函数会建立一个新的进程，执行调用者的操作，而原来的进程还会存在，直到用户显示的退出
2. `exec*`函数会用新的进程代替原有的进程，系统会从新的进程运行，新进程的pid值与原来进程的pid值相同
3. `exec*`函数执行成功后不会返回，这是因为执行的新程序已经占用了当前进程的空间资源，这些资源包括代码段，数据段和堆栈等。他们都已经被新的内容取代，而进程的id等标识性的信息仍然是原来的东西。



----

that's all