### 1.The use of fork() system call
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 3.Write a C program to demonstrate the use of fork() system call  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>

int a=10;  //global variable

int main(void)
{
  int i;
  pid_t pid;    // datatype it holds the process id
  printf("this is main function\n");
  pid=fork();    // process creation
  if(pid < 0)    // return -1 that means fails
  {
    printf("Fork() system call is failed\n");
    exit(15);    // process termination
  }
  else if(pid == 0)  // if fork return 0 that mean this is a child process
  {
    printf("I am child process\n");
    for(i=0; i<10; i++)
    {
      a++;
    }
    printf("child process a value=%d\n",a);
    exit(10);
  }
  else                // fork return not zero value (child pid)
  {
    printf("I am parent process\n");
    for(i=0; i<10; i++)
    {
      a--;
    }
    printf("parent process a value=%d\n",a);
    sleep(3);
  }
  printf("with in main=%d\n",a);
  return 0;
} 
```
### 2.The use of the execvp() function.
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 6.Write a C program to illustrate the use of the execvp() function. *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
    char *args[] = {"ls", "-l", NULL};  // Command to execute: ls -l
    printf("Before execvp() call\n");
    // Replace current process image with ls -l
    if (execvp(args[0], args) == -1)
    {
      perror("execvp failed\n");
    }
    // This line will not execute if execvp() is successful
    printf("This will only print if execvp() fails\n");
    return 0;
}

```
### 3.Create a child process using fork() and print its PID.
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 10.Write a program in C to create a child process using fork() and print its PID. *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main(void)
{
  pid_t pid;
  pid = fork();  // Create a child process
  if (pid < 0)
  {
    // Fork failed
    printf("Fork failed.\n");
    return 1;
  }
  else if (pid == 0)
  {
    // Child process
    printf("Child Process:\n");
    printf("Child PID=%d\n", getpid());
    printf("Parent PID=%d\n", getppid());
  }
  else
  {
    // Parent process
    sleep(1);
    printf("Parent Process:\n");
    printf("Parent PID=%d\n", getpid());
    printf("Child PID=%d\n", pid);
  }
  return 0;
}
```
### 4.multiple child processes using fork() and display their PIDs.
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * 
 * 15. Write a C program to create multiple child processes using fork() and display their PIDs. *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<sys/types.h>
#include<sys/wait.h>

int main(void)
{
  int n, i;
  pid_t pid;
  printf("Enter number of child processes to create: ");
  scanf("%d", &n);
  for(i=0;i<n;i++)
  {
    pid = fork();
    if(pid < 0)
    {
      perror("fork failed");
      return 1;
    }
    else if(pid == 0)
    {
      // Child process
      printf("Child %d created with PID: %d : %d\n", i + 1, getpid(),getppid());
      return 0;  // Child exits after printing its PID
    }
  }
  printf("parent PID: %d\n", getpid());
  // Only parent reaches here
  for (i = 0; i < n; i++)
  {
    wait(NULL);  // Wait for all child processes
  }
  return 0;
}
```
### 5.create a zombie process and explain how to avoid it.
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 19. Write a program in C to create a zombie process and explain how to avoid it.*
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>


#if 1
int main(void)
{
  pid_t pid=fork();
  if(pid < 0)
  {
    // Fork failed
    perror("fork");
    return 1;
  }
  else if(pid == 0)
  {
    // Child process
    printf("Child process (PID: %d) exiting...\n", getpid());
    exit(0);  // Exits immediately
  }
  else
  {
    // Parent process
    printf("Parent process (PID: %d) sleeping, child is now a zombie.\n", getpid());
    sleep(30);  // Keeps parent alive, child becomes zombie
    printf("Parent process exiting.\n");
  }
  return 0;
}
#endif

#if 0

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>  // Required for wait()

int main(void)
{
  pid_t pid = fork();
  if (pid < 0)
  {
    perror("Fork failed");
    return 1;
  }
  if (pid == 0)
  {
    // Child process
    printf("Child process (PID: %d) exiting.\n", getpid());
    exit(0);
  }
  else
  {
    // Parent process
    wait(NULL);  // Wait for child to terminate
    printf("Parent (PID: %d) reaped child. No zombie created.\n", getpid());
  }
  return 0;
}
#endif
```
### 6.The use of the waitpid() function for process synchronization
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 22. Write a C program to demonstrate the use of the waitpid() function for process synchronization  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(void)
{
  pid_t pid, wpid;
  int status;
  pid = fork();
  if(pid < 0)
  {
    perror("fork failed");
    exit(EXIT_FAILURE);
  }
  if (pid == 0)
  {
    // Child process
    printf("Child Process (PID: %d) is running...\n", getpid());
    sleep(3);  // Simulate work
    printf("Child Process (PID: %d) finished.\n", getpid());
    exit(42);  // Exit with custom status
  }
  else
  {
    // Parent process
    printf("Parent Process (PID: %d) is waiting for child (PID: %d)...\n", getpid(), pid);
    wpid = waitpid(pid, &status, 0);  // Wait for specific child
    if(wpid == -1)
    {
      perror("waitpid failed");
      exit(EXIT_FAILURE);
    }
    if (WIFEXITED(status))
    {
      printf("Child exited with status %d\n", WEXITSTATUS(status));
    }
    else
    {
      printf("Child did not terminate normally\n");
    }
    printf("Parent Process (PID: %d) continues...\n", getpid());
  }
  return 0;
}
```
### 7.daemon process.
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 25. Write a program in C to create a daemon process.  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * */
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(void)
{
  pid_t pid, wpid;
  int status;
  pid = fork();
  if(pid < 0)
  {
    perror("fork failed");
    exit(EXIT_FAILURE);
  }
  if (pid == 0)
  {
    // Child process
    printf("Child Process (PID: %d) is running...\n", getpid());
    sleep(3);  // Simulate work
    printf("Child Process (PID: %d) finished.\n", getpid());
    exit(42);  // Exit with custom status
  }
  else
  {
    // Parent process
    printf("Parent Process (PID: %d) is waiting for child (PID: %d)...\n", getpid(), pid);
    wpid = waitpid(pid, &status, 0);  // Wait for specific child
    if(wpid == -1)
    {
      perror("waitpid failed");
      exit(EXIT_FAILURE);
    }
    if (WIFEXITED(status))
    {
      printf("Child exited with status %d\n", WEXITSTATUS(status));
    }
    else
    {
      printf("Child did not terminate normally\n");
    }
    printf("Parent Process (PID: %d) continues...\n", getpid());
  }
  return 0;
}
```
### 8.The use of the system() function for executing shell  commands
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 28. Write a C program to demonstrate the use of the system() function for executing shell  commands.*
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */


#include <stdio.h>
#include <stdlib.h>

int main(void)
{
  int choice;
  while (1)
  {
    printf("\n--- Menu ---\n");
    printf("1. List files\n");
    printf("2. Show current directory\n");
    printf("3. Display date and time\n");
    printf("4. Exit\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);
    getchar(); // To consume newline
    switch (choice)
    {
      case 1:
        system("ls -l");
        break;
      case 2:
        system("pwd");
        break;
      case 3:
        system("date");
        break;
      case 4:
        exit(0);
      default:
        printf("Invalid choice!\n");
    }
  }
  return 0;
}
```
### 9.Pass arguments to the child process
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 32. Write a C program to create a process using fork() and pass arguments to the child process. *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/wait.h>

int main(void)
{
  pid_t pid;
  // Fork a child process
  pid = fork();
  if(pid < 0)
  {
    perror("Fork failed");
    return 1;
  }
  else if (pid == 0)
  {
    // Child process
    printf("Child: PID = %d\n", getpid());

    // Prepare command arguments ("pwd")
    char *args[] = {"pwd", NULL};

    // Execute the command
   execvp(args[0], args);
   // If execvp() fails
   perror("execvp failed");
   exit(1);
  }
  else
  {
    // Parent process
    printf("Parent: PID = %d, Child PID = %d\n", getpid(), pid);
    wait(NULL); // Wait for child to finish
    printf("Parent: Child completed.\n");
  }
  return 0;
}
```
### 10.Process synchronization using semaphores
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 35.Write a program in C to demonstrate process synchronization using semaphores.*
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ipc.h>
#include <sys/sem.h>
#include <sys/types.h>
#include <sys/wait.h>

// Define union semun if not defined
union semun
{
  int val;
  struct semid_ds *buf;
  unsigned short *array;
};

void sem_wait(int semid);
void sem_signal(int semid);

int main(void)
{
  key_t key = ftok("semfile", 65); // Unique key
  int semid = semget(key, 1, 0666 | IPC_CREAT); // Create semaphore

  union semun sem_union;
  sem_union.val = 1; // Initial value = 1
  semctl(semid, 0, SETVAL, sem_union);

  pid_t pid = fork();

  if (pid == 0)
  {
    // Child process
    sem_wait(semid);
    printf("Child entering critical section...\n");
    sleep(2); // Simulate critical section work
    printf("Child leaving critical section.\n");
    sem_signal(semid);
  }
  else
  {
    // Parent process
    sem_wait(semid);
    printf("Parent entering critical section...\n");
    sleep(2);
    printf("Parent leaving critical section.\n");
    sem_signal(semid);
    wait(NULL); // Wait for child to finish
    semctl(semid, 0, IPC_RMID); // Remove the semaphore
  }
  return 0;
}

void sem_wait(int semid)
{
  struct sembuf sb = {0, -1, 0}; // Decrement operation (P)
  semop(semid, &sb, 1);
}
void sem_signal(int semid)
{
  struct sembuf sb = {0, 1, 0}; // Increment operation (V)
  semop(semid, &sb, 1);
}

```
### 50. Child Running Continuously Parent should Terminate.
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 50.Write a C program to Create Child process                        *
 *    The child process should continuously run Printing a message.    *
 *    The parent process should terminate the child process            *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <signal.h>
#include <sys/wait.h>

int main(void)
{
  int stat;
  pid_t pid = fork();
  if(pid<0)
  {
    perror("Fork failed");
    exit(1);
  }
  if(pid == 0)
  {
    // Child process
    while(1)
    {
      printf("Child is running...\n");
      sleep(1);
    }
  }
  else
  {
    // Parent process
    printf("Parent: Child PID = %d\n", pid);
    sleep(10);
    //wait(&stat);
    printf("Parent: Killing child...\n");
    kill(pid, SIGKILL);
    printf("Parent: Child killed\n");
  }
  return 0;
}
```
### Output
```c
Parent: Child PID = 1731
Child is running...
Child is running...
Child is running...
Child is running...
Child is running...
Child is running...
Child is running...
Child is running...
Child is running...
Child is running...
Parent: Killing child...
Parent: Child killed
```

