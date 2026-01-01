## ----------------------Create Thread programms------
 
### 1.Prints "Hello, World!"?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 1.Write a C program to create a thread that prints "Hello, World!"? *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<pthread.h>

void *create_thread(void *arg);
int main(void)
{
  pthread_t t_id;                                               // Thread ID
  if(pthread_create(&t_id,NULL,create_thread,"Hello World")!=0)     // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  pthread_join(t_id,NULL);                                      // wait for the thread to finish
  printf("main thread id=%u\n",(unsigned int)pthread_self());   // get pthread id
  printf("process id=%u\n",getpid());                           // get process id
}

void *create_thread(void *arg)                                // thread function
{
  char *ptr=(char *)(arg);
  printf("%s\n",ptr);
  printf("process id=%u\n",getpid());                        //get process id
  return 0;
}
```
## 2.Multiple threads,each printing its own message? 
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 2.Write a c program to create multiple threads,each printing its own message? *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<pthread.h>

void *create_thread1(void *arg);
void *create_thread2(void *arg);
void *create_thread3(void *arg);
void *create_thread4(void *arg);
int main(void)
{
  pthread_t tid1,tid2,tid3,tid4;                    // Threads ID
  if(pthread_create(&tid1,NULL,create_thread1,NULL)!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  if(pthread_create(&tid2,NULL,create_thread2,NULL)!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  if(pthread_create(&tid3,NULL,create_thread3,NULL)!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  if(pthread_create(&tid4,NULL,create_thread4,NULL)!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }    
  pthread_join(tid1,NULL);                          //wait for the thread to finish
  pthread_join(tid2,NULL);                            
  pthread_join(tid3,NULL);                           
  pthread_join(tid4,NULL);                            
  return 0;
}

void *create_thread1(void *arg)                                // threads function 
{                         
  printf("I am 1st Thread function:Hi\n");                      
  pthread_exit(0);                                             // To terminate thread function
}

void *create_thread2(void *arg)                                // thread function 
{                         
  printf("I am 2nd Thread function:Hi\n");                       
  pthread_exit(0);
}

void *create_thread3(void *arg)                                // thread function 
{                         
  printf("I am 3rd Thread function:Hi\n");                     
  pthread_exit(0);
}

void *create_thread4(void *arg)                                // thread function 
{                         
  printf("I am 4th Thread function:Hi\n");                        
  pthread_exit(0);
}
```
## 3.Print numbers from 1 to 10 concurrently?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 3.Develop a C program to create two threads that print numbers from 1 to 10 concurrently? *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<pthread.h>

void *create_thread(void *arg);

int main(void)
{
  pthread_t tid1,tid2;                                             // Thread ID
  if(pthread_create(&tid1,NULL,create_thread,"thread_1")!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  if(pthread_create(&tid2,NULL,create_thread,"thread_2")!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  pthread_join(tid1,NULL);                               // wait for the thread to finish
  pthread_join(tid2,NULL);                              // wait for the thread to finish
  printf("main thread id=%u\n",(unsigned int)pthread_self()); // get pthread id 
  printf("process id=%u\n",getpid());    // get process id
  return 0; 
}

void *create_thread(void *arg)                                // thread function 
{
  char *str=(void *)(arg);
  int i=0;
  for(i=0;i<=10;i++)
  {
    printf("%s:%d\n",str,i);
  }  
}

```
## 4.Calculates the factorial of a given number? 
```c

/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 4.Implement a C program to create a thread that calculates the factorial of a given number? *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
void *create_thread(void *arg);
int main(void)
{
  pthread_t tid;                   // Thread
  int number=0;
  if(pthread_create(&tid,NULL,create_thread,NULL)!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  pthread_join(tid,NULL);                          // wait for the thread to finish
  return 0; 
}

void *create_thread(void *arg)                                // thread function 
{
  int factorial=1,number=0,i;
  printf("Enter Number:");
  scanf("%d",&number);
  if(number<0)
  {
    printf("only 0< values\n");
    pthread_exit(0);
  }
  for(i=1;i<=number; i++)
  {
    factorial *= i;
  }
  printf("factorial=%d\n",factorial);
} 

```
## 5.Print their thread IDs?
```c
/* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *
 * 5.Write a C program to create two threads that print their thread IDs?  *
 * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * */

#include<stdio.h>
#include<unistd.h>
#include<pthread.h>

void *create_thread(void *arg);

int main(void)
{
  pthread_t tid1,tid2;                               // Thread
  if(pthread_create(&tid1,NULL,create_thread,"thread_1")!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  if(pthread_create(&tid2,NULL,create_thread,"thread_2")!=0)   // Create the thread
  {
    printf("thread creation is failed\n");
    return 0;
  }
  pthread_join(tid1,NULL);                          // wait for the thread to finish
  pthread_join(tid2,NULL);                          // wait for the thread to finish
  return 0; 
}

void *create_thread(void *arg)                                // thread function

{
  char *str=(char *)arg;
  printf("%s:%ld\n",str,pthread_self());
} 
```
