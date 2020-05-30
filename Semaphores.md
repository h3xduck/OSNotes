# SEMAPHORES
## By Marcos Sánchez Bajo

### **POSIX for semaphores**
```c
int sem_init(sem_t *sem, int shared, int val);
```
Initializes unnamed  semaphore.
___
___

```c
int sem_destroy(sem_t *sem);
```
Destroys unnamed semaphore.
___
___
```c
sem_t *sem_open(char *name, int flag, mode_t mode, int val);
```
Opens (creates) a named  semaphore.
___
___
```c
int sem_close(sem_t *sem);
```
Closes a named  semaphore.
___
___
```c
int sem_unlink(char *name);
```
Deletes a named  semaphore. ̈
___
___
```c
int sem_wait(sem_t *sem);
```
Performs wait operation on a  semaphore. 
___
___
```c
int sem_trywait(sem_t*sem);
```
Try wait. If blocked returns -1 without doing anything.
___
___
```c
int sem_post(sem_t *sem);
```
Performs signal operation on a semaphore.

### Example: unnamed semaphore
#### Creation
```c
sem_t elements;
``` 
#### Initialization
```c
sem_init (&elements,0,0);
```
#### Deletion
```c
sem_destroy (&elements);
```
#### Decrease semaphore
```c
sem_wait(&elements)
```
#### Increase semaphore
```c
sem_post(&elements)
```

### Example: named semaphore
Remember, it is good when multiple process exist and they need synchronization
#### Creation
```c
sem_t *sem_lec;
``` 
#### Initialization
```c
sem_lec = sem_open("/tmp/sem_2", O_CREAT, 0644, 1)
```
#### Closing
```c
sem_close(sem_lec);
```
#### Deletion
```c
sem_unlink("/tmp/sem_2");
```
#### Decrease semaphore
```c
sem_wait(sem_lec);
```
#### Increase semaphore
```c
sem_post(sem_lec);
```
