# ENVIRONMENT OF A PROCESS
## By Marcos Sánchez Bajo

```c
char * getenv(const char * var);
```
Get the value of an environment variable.
___
```c
int setenv(const char * var, const char * val, int overwrite);
```
Modifies or adds an environment variable
___
```c
int putenv(const char * par);
```
Modifies or adds a pair var=value.
___