## 1.Create a new text file and write user input to it
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 1.Write a C program to create a new text file and write user input to it    *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main(void)
{
  int fd,ref,len;
  char buf[100];
  printf("Enter a string:\n");
  scanf("%[^\n]",buf);           // be careful while using this format specifier --string length <= size-1
  len=strlen(buf);
  printf("%d\n",len);
  fd=open("file.txt",O_CREAT|O_RDWR|O_TRUNC,0666); //O_CREAT use for if file is not exist then its create
  printf("%d\n",fd);                               //O_RDWR use for write and file operation on files
                                                   // O_TRUNC use for previous content removes 
  if(fd<0)                                   // open system call return -1 open system call is failed 
  {
    printf("open system call failed\n");
    return 0;
  }
  ref=write(fd,buf,strlen(buf));                //write system return number of bytes written
  printf("%d\n",ref);
  if(ref<0)                                    // write system call return -1 write system call is failed
  {
    printf("Write system call is failed\n");
    close(fd);
    return 0;
  }
  close(fd);                      //  close fd other than next runtime open system call fails
  return 0;
}
```
## 2.Open an existing text file and display its contents 
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 2.Develop a C program to open an existing text file and display its contents  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>

int main(void)
{
  int fd,ref,len;
  char buf[100];
  fd=open("file.txt",O_RDWR,0666); 
  if(fd<0)                                   
  {
    printf("open system call failed\n");
    return 0;
  }
  while((ref=read(fd, buf, sizeof(buf)-1)) > 0)
  {
    buf[ref]='\0';	  
    printf("%s",buf);
  }
  if(ref<0)
  {
    printf("read system call is failed\n");
    close(fd);
  }
  close(fd);                     
  return 0;
}
```
## 3.Create a new directory in the current directory?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 3.Implement a C program to create a new directory named "Test" in the current directory?  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#if 0                     // conditional compilation if it is 0 not execute
#include <stdlib.h>
#include <stdio.h>

int main(void) 
{
    int status;
    status = system("mkdir Test");
    if (status == 0)
    {
      printf("Directory created using system()!\n");
    }
    else
    {
      printf("Failed to create directory.\n");
    }
    return 0;
}
#endif
# if 1                      //if value is non zero value its execute
#include <stdio.h>
#include <unistd.h>
#include <sys/syscall.h>
#include <sys/stat.h>                     

int main(void) 
{
    int result;
    // SYS_mkdir is the syscall number for mkdir
    result = syscall(SYS_mkdir, "Test", 0777);
    if (result == 0)
    {
        printf("Directory 'Test' created successfully.\n");
    }
    else
    {
        perror("syscall mkdir failed"); //error message
    }
    return 0;
}
#endif
```
## 4.Check if a file exists in the current directory
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 4.Write a C program to check if a file named "sample.txt" exists in the current directory?  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <unistd.h>
#include <sys/syscall.h>
#include <sys/stat.h>

int main(void) 
{
  if(access("Test", F_OK)==0)
  {
    printf("File is are there\n");
  }
  else
  {
      perror("No such file or directory\n");
  }
  return 0;
}
```
## 5.rename a file ?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 5. Develop a C program to rename a file from "oldname.txt" to "newname.txt"?  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>

int main(void) 
{
  // Attempt to rename the file
  if (rename("rename_file.c", "5_rename_file.c") == 0) 
  {
     printf("File renamed successfully.\n");
  }
  else
  {
     perror("Error renaming file");
  }
  return 0;
}
```
## 6.Delete a file named "a.out"?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 6. Implement a C program to delete a file named "a.out"?    *  
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>

int main(void) 
{
  // remove the file
  if (remove("a.out") == 0) 
  {
     printf("File renamed successfully.\n");
  }
  else
  {
     perror("Error renaming file");
  }
  return 0;
}
```
## 7.Copy the contents of one file to another?  
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 7. Write a C program to copy the contents of one file to another?           * 
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>

int main(void)
{
  int src_fd,des_fd,read_ref,write_ref,len;
  char buf[100];
  src_fd=open("src_file.txt",O_RDONLY); // source file must be exist
  printf("src_fd=%d\n",src_fd);                                                                      
  if(src_fd<0) 
  {
    printf("open system call failed\n");
    return 0;
  }
  des_fd=open("des_file.txt",O_CREAT|O_WRONLY|O_TRUNC,0666); 
  printf("des_fd=%d\n",des_fd);                                                                      
  if(des_fd<0) 
  {
    printf("open system call failed\n");
    close(src_fd);
    return 0;
  }
  while((read_ref=read(src_fd,buf,sizeof(buf)))>0)
  { 
    write_ref=write(des_fd,buf,read_ref);
    if(write_ref !=read_ref)
    {
      perror("write system call is failed\n");
      close(src_fd);
      close(des_fd);
      return 0;
    }
  }
  if(read_ref==0)
  {
    printf("Read from file is done\n");
  }
  else
  {
    perror("write system call is failed\n");
    close(src_fd);
    close(des_fd);
    return 0;
  }
}
```
### 8.Move a file from one directory to another?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 8.Develop a C program to move a file from one directory to another?   *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>

int main(void) 
{
  char *old_path="src_file.txt";
  char *new_path="../destination/des.txt";
  // Attempt to rename the file
  if (rename(old_path,new_path) == 0) 
  {
     printf("File renamed successfully.\n");
  }
  else
  {
     perror("Error renaming file");
  }
  return 0;
}
```
### 9.List all files in the current directory?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 9. Implement a C program to list all files in the current directory?  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <stdlib.h>
#include <dirent.h>

int main(void)
{
  struct dirent *dir_entry;  // Pointer for directory entry
  // Open the current directory
  DIR *dir_stream = opendir(".");  //DIR for directory stream 
  if (dir_stream == NULL)
  {
    perror("Could not open current directory");
    return 1;
  }
  printf("Files in the current directory:\n");
  while ((dir_entry = readdir(dir_stream)) != NULL)
  {
    printf("%s\n", dir_entry->d_name); //structure member
  }
  closedir(dir_stream);
  return 0;
}
```
### 10.Get the size of a file named "file.txt"?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 10. Write a C program to get the size of a file named "file.txt"?     *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <stdlib.h>
#include <sys/stat.h>

int main(void)
{
  struct stat file_stat;        //structure stat -- it stores file type,size,permissions
  if (stat("10_size_of_file.c", &file_stat) == -1) //stat is a system call 
  {
    perror("stat failed");
    return 1;
  }
  printf("Size of file.txt: %ld bytes\n", file_stat.st_size); 
  return 0;
}
```
