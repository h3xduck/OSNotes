# Directory Handling
## By Marcos Sánchez Bajo

### Create directory
```c
int mkdir(const char *name, mode_t mode);
```
Arguments:
* name: directory name.
* mode: protection bits. ̈

Returns: 
* Zero or -1 on error. ̈

Description:
* Creates a directory named name.
* Owner UID = effective UID. 
* Owner GID = effective GID

### Remove directory 
```c
int rmdir(const char *name); 
```
Arguments: 
* name: Directory name. 

Returns: 
* Zero or -1 on error.

Decription: 
* Remove directory if it is empty.
* Otherwise directory is not removed.

### Open a directory
```c
DIR *opendir(char * name);
```
Arguments: 
* dirname: Directory name. ̈

Returns: 
* A pointer to be used with readdir() or closedir().
* NULL on error. ̈

Description:
* Opens a directory as a sequence of entries.
* Places pointer in first entry.

### Close a directory
```c
int closedir(DIR *dirp); 
```
Arguments:
* dirp: Pointer returned by opendir(). 

Returns:
* Zero or -1 if error. 

Description:
* Closes association between dirp and directory entry sequence.

### Read directory entries
```c
struct dirent *readdir(DIR *dirp);
```

Arguments: 
* dirp: pointer returned by opendir().
 
Returns: 
* A pointer to an object of type struct dirent representing directory.
* NULL on error. ̈

Description:
* Returns next entry in directory associated to dirp and advances pointer.* Structure is implementation dependent but you can assume it has a member char* d_name.

### Position directory pointer
```c
void rewindir(DIR *dirp);
```

Arguments: 
* dirp: pointer returned by opendir().

Description:
* Sets directory position pointer to the first entry.

### Create a directory entry
```c
int link(const char *existing, const char *new);
int symlink(const char *existing, const char *new);
```

Arguments:
* existing: Name of existing file.
* new: name of new entry that will be linked to existing file.  

Returns: 
* Zero or -1 if error. 

Description: 
* Create a new physical or symbolic link to an existing file.
* The OS does not record which is the original file and which is the new one.

### Remove a directory entry
```c
int unlink(char *name);
```

Arguments: 
* name: File name.

Returns: 
* Zero or -1 if error. 

̈Description:
* Removes entry to directory and decrements number of links to file.
* When number of links equals zero and no process keeps it open, space if freed and file is no longer accessible.

### Change current directory
```c
int chdir(char *name); 
```
Arguments: 
* name: directory name 

Returns: 
* Zero or –1 if error. 

Description:
* Modifies current directory used to form relative paths.

### Change file name
```c
int rename(char *old, char *new);   
```

Arguments:
* old: Name of existing file.
* new: New file name. ̈

Returns:
* Zero or -1 if error. ̈

Description:
* Change name of file old. 
* New name is new.

### Get name of current directory
```c
char *getcwd(char *buf, size_t size);  
```

Arguments: 
* buf: pointer to buffer to store name of current directory.
* size: Length in byts of buffer. 

Returns:
* Pointer to buf or NULL if error. 

Description:
* Gets name of current directory.