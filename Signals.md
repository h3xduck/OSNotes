# SIGNALS
## By Marcos Sánchez Bajo

### **kill**
Send signal sig to process pid

Special cases:
* pid==0:
  * Signal to process gid equal to process gid
* pid==-1:
  * Signal to all process (except system process)
* pid<-1
  * Signal to all process with gid equals to absolute value of pid
 
```c
int kill(pid_t pid, int sig);
```

### **sigaction**
* Permits to specify actions to the signalsig.
* Old action can be stored in oact.

```c
int sigaction(int sig, struct sigaction *act, struct sigaction *oact)
```
```c
struct sigaction{
    void (*sa_handler)();/* handlers*/
    sigset_t sa_mask; /* blocked signals*/
    int sa_flags;     /* options*/
};
```
Handler:
* SIG_DFL: Default action (usually terminates process).
* SIG_IGN: Ignores signal.
* Address of handler function.

### **Signal sets**
```c
int sigemptyset(sigset_t *set);
```
Creates an empty signal set
___
```c
int sigfillset(sigset_t *set);
```
Creates a full set of all possible signals.
___
```c
int sigaddset(sigset_t *set, int signo);
```
Adds a signal to a set of signals.
___
```c
int sigdelset(sigset_t *set, int signo);
```
Removes a signal from a signal set.
___
```c
int sigismember(sigset_t *set, int signo);
```
Checks whether a signal belongs to a signal set.

### **Example: ignore Ctrl-C (SIGINT)**
```c
struct sigaction act;
act.sa_handler= SIG_IGN;
act.flags= 0;
sigemptyset(&act.sa_mask);
Sigaction(SIGINT, &act, NULL);
```

## **Signal + handler**
```c
sighandler_t signal(int signum, sighandler_t handler);
```
**Example**
```c
void sig_handler(int signo)
{
  if (signo == SIGINT)
    printf("received SIGINT\n");
}

int main(void)
{
  signal(SIGINT, sig_handler); 
  while(1) 
    sleep(1);
  return 0;
}
```
