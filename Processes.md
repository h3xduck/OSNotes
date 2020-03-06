# PROCESSES 
## By Marcos Sánchez Bajo

### **Fork**
Duplicates process involving the call
Parent process and child process go on running the same program.

Child process inherits open files from parent process.

Returns:
* 1 on error.
* In parent process: child process descriptor.
* In child process: 0
 
```c
pid_t fork(void);
```
### **Exec**
Changes current process image.
* path: path to executable file.
* file: Looks for the executable file in all directories specified by PATH.̈
* Description:
  * Returns-1 on error, otherwise it does not return.
  * The same process runs another program.
  * Open files remain open.
  * Signals with default action remain defaulted, signals with handler take defaultaction.
```c
int execl(constchar *path,constchar *arg, ...);
int execv(constchar* path, char*constargv[]);
int execve(constchar* path, char*constargv[], char*constenvp[]);
int execvp(constchar *file, char *constargv[])
```

### **Exit**
Finalizes process execution
* All open files descriptors are closed.
* All process resources are released.
* PCB (Process Control Block) is released.
```c
void exit(status);
```
![alt text](https://media.geeksforgeeks.org/wp-content/uploads/Wait_system_call_in_c.jpg "Process lifecycle")

### **Wait**
* If there are at least one child processes running when the call to wait() is made, the caller will be blocked until one of its child processes exits. At that moment, the caller resumes its execution.
* If there is no child process running when the call to wait() is made, then this wait() has no effect at all. That is, it is as if no wait() is there. 

```c
pid_t wait(int *stat_loc); 
```

Waitpid() waits for a specific child process: 
```c 
pid_t waitpid (child_pid, &status, options);
```
* Return value of waitpid():
  * pid of child, if child has exited
  * 0, if using WNOHANG and child hasn’t exited
