# POSIX
## By Marcos Sánchez Bajo

### **Fork**
```c
pid_t fork(void);
```
* Duplicates process invoking the call.
* Parent process and child process go on running the same program.
* Child process inherits open files from parent process.
* Open file descriptors are copied.
* Pending alarms are deactivated.
* Returns:
  * 1 on error.
  * In parent process: child process descriptor.
  * In child process: 0.

### **Exec**
Changes current process image.
* path: path to executable file.
* file: Looks for the executable file in all directories specified by PATH.̈
* Description:
  * Returns -1 on error, otherwise it does not return.
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

## POSIX FOR FILES

### **Creat**
Creates a file
```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int creat(char *name, mode_t mode);
```
* Arguments:
  * name: File name
  * mode: Access rights bits.
* Returns: 
  * Return file descriptor or -1 upon error.
* Description: File is open for writing.
* If not existing, creates an empty file.
  * UID_owner = UID_effective 
  * GID_owner = GID_effective
* If existing, truncates without changing access rights bits.

Examples:
```c
fd =creat("data.txt", 0751);
fd =open("data.txt",O_WRONLY | O_CREAT | O_TRUNC, 0751);
```
### **Unlink**
Erase file
```c
#include <unistd.h>intunlink(constchar* path);
```
* Arguments:
  * pathfile name
* Returns:
  * Returns 0 or-1 upon error.
  * Description: 
    * Decrements link counter. If counter is 0, erases file and releases resources.

### **Open**
Opens a file
```c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
int open(char *name, int flag, ...);
```
* Arguments:
  * name: file name
  * flags: options for opening:
    * nO_RDONLY: Read only
    * nO_WRONLY: Write only
    * nO_RDWR: Read/writen
    * O_APPEND: Access pointer moves to file end.
    * O_CREAT: If existing has no effect. If not existing creates.
    * O_TRUNCT: Truncates if open for writing.
* Returns: 
  * A file descriptor or 
  * -1 upon error.
Examples: 
```c
fd =open("/home/peter/data.txt");
fd =open("/home/peter/data.txt",O_WRONLY | O_CREAT | O_TRUNC, 0750);
```

### **Close**
Closing a file
```c
int close(int fd);
```
* Arguments:
  * fd: file descriptor.
* Returns: 
Zero or -1 upon error.
* Description: 
  * Process looses its link with the file.

### **Read** 
Reading from a file
```c
#include <sys/types.h>
ssize_t read(int fd, void *buf, size_t n_bytes);
```
* Arguments: 
  * fd: File descriptor.
  * buf: Buffer for data storage.
  * n_bytes: Number  of bytes to be read
* Returns:
  * Number of bytes effectively read or -1 upon error.
* Description: 
  * Transfers n_bytes
  * Can read less bytes when end of file is reached or interrupted by a signal.
  * After reading the file pointer is incremented with the number of bytes effectively read.

### **Write**
Writing to a file
```c
#include <sys/types.h>
ssize_t write(int fd, void *buf, size_t n_bytes);
```
* Arguments:
  * fd: File descriptor
  * buf: Buffer with data to be written
  * n_bytes: NUmber of bytes to be written
* Returns:
  * Number of bytes effectively written or -1 upon error
* Description:
  * Transfers n_bytes.
  * It may write less data than required if file maximum size is reached or interrupted by a signal.
  * After writing, file pointer is incremented with the number of bytes effectively written.
  * If end of file is rebased, the file size is increased.

### **LSeek**
Moving the file pointer
```c
#include <sys/types.h>
#include <unistd.h>
off_t lseek(int fd, off_t offset, int whence);
```
* Arguments:
  * fd: File descriptor
  * offset: Offset from base position
  * whence: Base position for offset
* Returns:
  * New position or -1 upon error.
* Description:
  * Repositions pointer associated to a fd
  * New position computation:
    * SEEK_SET position = offset
    * SEEK_CUR position = current position + offset
    * SEEK_END position = file size + offset

### **FNCTL**
Attribute modification
```c
#include <sys/types.h>
int fnctl(int fildes, int cmd /*arg*/ ...);
```
* Arguments:
  * fildes: File descriptor
  * cmd: Command to modify attributes
* Returns:
  * 0 on sucess or -1 upon error
* Description:
  * Modifies attrubtes for an open file

### **DUP**
Duplicate a file descriptor
```c
int dup(int fd);
```
* Arguments:
  * fd: file descriptor
* Returns:
  * A file descriptor sharing all the properties of fd or -1 upon error
* Description:
  * Creates a new file descriptor having in common with the previous one:
    * Accesses to the same file
    * Shares the same position pointer
    * Access mode is identical
  * New descriptor gets the lowest available numeric value

### **FTRUNCATE**
Space allocation for a file
```c
#include <unistd.h>
int ftruncate(int fd, off_t length);
```
* Arguments:
  * fd: File descriptor
  * length: New file size
* Returns: 
  * Return 0 or -1 upon error
* Description:
  * New file size is "length"
  * If "length" is 0 file is truncated

### **STAT**
Information on a file
```c
#include<sys/types.h> 
#include<sys/stat.h> 
int stat(char *name,struct stat* buf);
int stat(int fd,struct stat *buf);
```
* Arguments: 
  * name: File name
  * fd: File descriptor
  * buf: Pointer to object of type struct stat
  * File information stored in buf
* Returns: 
  * 0 on sucess or -1 upon error
* Description: 
  * Gets information about a file and stores in object of type "struct stat"
```c
struct stat{
  mode_t st_mode; /*filemode*/
  ino_t st_ino; /*i-node*/
  dev_t st_dev; /*device*/
  nlink_t st_nlink; /*numberoflinks*/
  uid_t st_uid; /*ownerUID*/
  gid_t st_gid; /*ownerGID*/
  off_t st_size; /*numberofbytes*/
  time_t st_atime; /*lastacccess*/
  time_t st_mtime; /*lastmodification*/
  time_t st_ctime; /*lastdatamodification*/};
```
* Types of checks in st_mode:
  * `S_ISDIR(s.st_mode)` Is it a directory?
  * `S_ISCHR(s.st_mode)` Is it a special character file?
  * `S_ISBLK(s.st_mode)` Is it a special block file?
  * `S_ISREG(s.st_mode)` Is it a regular file?
  * `S_ISFIFO(s.st_mode)` Is it a pipe or FIFO? 

### **UTIME**
ALtering date attributes
```C
#include<sys/stat.h>
#include<utime.h>
int utime(char *name,struct utimbuf *times);
```
* Arguments:
  * name: File name
  * times: Structure with last access and modification dates
    * time_t actime: Access date
    * time_t mctime: Modification date
* Returns:
  * Returns 0, or -1 upon error
* Description:
  * Change dates for last access and last modification with values of struct utimbuf

## POSIX FOR DIRECTORIES
### Directories - logical view
* A directory is a file with records of structure DIR
* It can be operated as regular file for reading
  * But do not write to it with regular calls!!
* DIR structure:
  * d_ino; //i-node
  * d_off; //Position in file of element in directory
  * d_reclen; //Directory size
  * d_type; //Element type
  * d_name[0] //File name of variable length

### POSIX services for directories
```c
DIR *opendir(const char *dirname);
```
Opens a directory and returns a pointer of type DIR to the beginning.
___
```c
int readdir_r(DIR *dirp,struct dirent* entry, struct dirent **result);
```
Read next directory entry and returns result in astructdirent.̈
___
```c
long int telldir(DIR *dirp);
```
Get current position of pointer within directory file.
___
```c
void seekdir(DIR *dirp, long int loc);
```
Advance from current position to position specified by loc. Never goes backward.
___
```c
void rewinddir(DIR *dirp);
```
Reset file pointer and move it to the beginning.
___
```c
intclosedir(DIR *dirp);
```
Close directory file.

##PROJECTIONS IN POSIX
```c
void *mmap(void *direc, size_t len,int prot,int flags,int fd, off_t offset);
```
* Sets a projection from a process address space and a file
  * Return memory address where file was projected
  * direc: address where projection is performed. If NULL, the OS selects one
  * len: specifies number of bytes to project
  * prot: Protection bits for the area
  * flags: properties for the region
  * fd: File descriptor to be used in memory
  * offset: Initial offset on the file

* Projection types:
  * PROT_READ: Can read
  * PROT_WRITE: Can write
  * PROT_EXEC: Can execute
  * PROT_NONE: Cannot access
* Properties of a memory region:
  * MAP_SHARED:
    * Shared region
    * Modifications affect the file
    * Child processes share region
  * MAP_PRIVATE:
    * Private region
    * File is not modified
    * Child processes get non-shared duplicates
  * MAP_FIXED:
    * File must be projected in an address specified by the call
## POSIX removing mapping
```c
void munmap(void *direc, size_t len);
```
* Removes part of the process address space from address direc to address direc+len