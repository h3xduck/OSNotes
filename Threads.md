# THREADS
## By Marcos Sánchez Bajo

### **Creating a thread**
Creates a thread and starts  its execution.
* thread: Must pass the address of a variable of type pthread_t used as handle.
* attr: Must pass the address of a structures  with attributes. NULL may be passed to specify default attributes.
* func: Function with thread execution code.
* arg: Pointer to thread parameter. Only one parameter can be passed.
```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*func)(void *),void *arg);
```

### **Thread identifiers**
```c
pthread_t pthread_self(void);
```
Returns thread identifier for the calling thread.

### **Thread join()**
```c
int pthread_join(pthread_t thread,void **value)
```
* Invoking thread waits until the thread identified by the handle has terminated.
* thread: Handle to thread that must be waited to terminate.
* value: Thread termination value

### **Thread exit()**
```c
int pthread_exit(void *value)
```
* Allows a thread to terminate its execution, stating its 
termination status.
* Termination status cannot be a pointer to a local variable.

### **Thread attributes**
```c
int pthread_attr_init(pthread_attr_t*attr);
```
* Initializes a thread attribute structure.
```c
int pthread_attr_destroy(pthread_attr_t*attr);
```
* Destroys a thread attribute structure.
```c
int pthread_attr_setstacksize(pthread_attr_t*attr,intstacksize);
```
* Defines the stack size for a thread.̈
```c
int pthread_attr_getstacksize (pthread_attr_t*attr,int*stacksize);
```
* Allows to obtain the size of the thread stack.
```c
int pthread_attr_setdetachstate(pthread_attr_t*attr,intdetachstate)
```
* Sets the termination state for a thread.
* If detachstate== PTHREAD_CREATE_DETACHED
    * Thread  releases its resources upon thread  termination.
* If detachstate==PTHREAD_CREATE_JOINABLE 
    * Resources are not released automatically.
    * A call to pthread_join()is needed.

```c
int pthread_attr_getdetachstate(pthread_attr_t*attr,int*detachstate)
```
* Allows to get the termination status.

![alt text](https://i.ibb.co/Z6XJsNV/threadshierarchy.png "Process lifecycle")
