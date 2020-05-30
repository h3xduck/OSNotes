# TIMERS
## By Marcos Sánchez Bajo

### **POSIX for timers**
Remember that the OS sends SIGALRM when timer elapses.

```c
int alarm(unsigned int sec)
```
* Sets a timer.
* If argument is zero, deactivates timer.

### Example: message every 10 seconds
```c
#include <signal.h>
#include <stdio.h>
void handle_alarm(void) {
    printf("Activated \n");
    }
int main(){
    structsigactionact; /*Setishandler for SIGALRM*/
    act.sa_handler= handle_alarm;
    act.sa_flags= 0;
    sigaction(SIGALRM, &act, NULL);
    act.sa_handler= SIG_IGN; /*ignore SIGINT*/
    sigaction(SIGINT, &act, NULL);
    for(;;){  * SIGALRM every 10 secons*/
        alarm(10);
        pause();
    }
}
```