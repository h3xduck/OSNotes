# PIPES
## By Marcos Sánchez Bajo

### **POSIX for pipes**
**Example, command ls | grep a**
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h> 
int main(int argc, char *argv[]) {
    intfd[2];
    pipe(fd);
    if(fork()!=0) { /* parent */
        close(STDIN_FILENO);
        dup(fd[STDIN_FILENO]);
        close(fd[STDIN_FILENO]);
        close(fd[STDOUT_FILENO]);
        execlp(“grep", “grep", ”a”, NULL);
    } else{ /* child */ 
        close(STDOUT_FILENO);
        dup(fd[STDOUT_FILENO]);
        close(fd[STDOUT_FILENO]);
        close(fd[STDIN_FILENO]);
        execlp("ls", "ls", NULL);
    }
    return 0;
}
```