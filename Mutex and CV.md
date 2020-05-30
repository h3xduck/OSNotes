# MUTEX AND CONDITIONAL VARIABLES
## By Marcos Sánchez Bajo

### **POSIX for mutex**
```c
pthread_mutex_t mutex;
```
Creation of mutex
___
___
```c
int pthread_mutex_init(pthread_mutex_t *mutex, pthread_mutexattr_t * attr);
```
Initialize mutex.
___
___
```c
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```
Destroy mutex.
___
___
```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
```
Try to get access to mutex.

Blocks thread if mutex is already acquired by other thread.
___
___
```c
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```
Unblock mutex.

### **POSIX for Conditional Variables**
```c
pthread_cond_t non_empty;
```
Creation of conditional variable.
___
___
```c
int pthread_cond_init(pthread_cond_t*cond, pthread_condattr_t*attr);
```
Initialize a condition variable.
___
___
```c
int pthread_cond_destroy(pthread_cond_t *cond);
```
Destroy a condition variable.
___
___
```c
int pthread_cond_signal(pthread_cond_t *cond);
```
Unblocks one or more threads that are suspended in the condition variable cond. 

Has no effect if there is no thread waiting (difference with semaphores).
___
___
```c
int pthread_cond_broadcast(pthread_cond_t *cond);
```
All blocked threads in condition variable cond are unblocked.

Has no effect if there is no thread waiting.
___
___
```c
int pthread_cond_wait(pthread_cond_t*cond, pthread_mutex_t*mutex);
```
Suspend thread until another thread signals condition variable cond.

Automatically releases the mutex. When thread is unblocked it contends again for the mutex.