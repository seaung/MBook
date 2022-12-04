##### Linux环境下进程的产生方式

1. 通过`fork`函数
2. 通过`system`函数
3. 通过`exec`系列函数

##### `fork`函数创建进程

相关函数原型

```c
#include <sys/types.h>
#include <unistd.h>
pid_t getpid(void);
pid_t getppid(void);

pid_t fork(void);
```

相关函数作用解释

getpid: 获取当前进程id

getppid: 获取当前进程的父进程的id

fork: 以父进程为蓝本复制一个进程

fork函数的特点：

执行一次，返回两次。在父进程和子进程中返回的是不同的值，父进程中返回的是子进程的id号，而子进程返回的是0

fork函数的返回值是进程的id,失败则返回-1

fork函数使用例子

```c
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>

int main(void) {
	pid_t pid;

	pid = fork();
	if(-1 == pid) {
		printf("create process failed!\n");
		return -1;
	} else if (pid == 0) {
		printf("pid = %d child return value of  : %d parent process id %d\n", pid, getpid(), getppid());
	} else {
		printf("pid = %d parent return value of : %d parent process id : %d\n", pid, getppid(), getpid());
	}
	return 0;
}
/*
pid = 1222534 parent return value of : 328898 parent process id : 1222533
pid = 0 child return value of  : 1222534 parent process id 1222533
*/
```

##### `system`函数

相关函数原型

```c
#include <stdlib.h>
int system(const char *command);
```

函数用作解释

system函数调用shell的外部命令在当前进程中开始另一个进程

失败时返回-1，成功返回进程状态值，当sh不能执行，返回127

system函数使用例子

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(void) {
	int res;

	printf("current process id : %d\n", getpid());
	res = system("ping 127.0.0.1 -c 3");
	printf("the return value of : %d\n", res);
	return 0;
}
```

##### `exec系列函数`

相关函数原型

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

exec函数使用例子

```c
#include <stdio.h>
#include <unistd.h>

int main(void)
{
    char args[] = {"/bin/ls", NULL};
    if(execve("/bin/ls", args, NULL) < 0)
        printf("create process error!\n");
    return 0;
}
```



##### fork,system和exec的区别

1. fork,system函数会建立一个新的进程，执行调用者的操作，而原来的进程还会存在，直到用户显示的退出
2. `exec*`函数会用新的进程代替原有的进程，系统会从新的进程运行，新进程的pid值与原来进程的pid值相同
3. `exec*`函数执行成功后不会返回，这是因为执行的新程序已经占用了当前进程的空间资源，这些资源包括代码段，数据段和堆栈等。他们都已经被新的内容取代，而进程的id等标识性的信息仍然是原来的东西。



---

that's all