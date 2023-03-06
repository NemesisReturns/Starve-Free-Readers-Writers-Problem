# Starve-Free-Readers-Writers-Problem
Here I present a starve free solution of readers writers problem. I have taken pseudocodes and definitions used in slides in class.
### The classical solution of the problem is(As given in slides):
Reader Process :
```cpp
wait(mutex);
readcount++;
if (readcount== 1) wait(wrt);
signal(mutex);
.
.
.
reading is performed
.
.
.
wait(mutex);
readcount--;
if (readcount== 0) signal(wrt);
signal(mutex):
```
Writer Process :
```cpp
wait(wrt);
.
.
.
writing is performed
.
.
.
signal(wrt);
```
In the above code, mutex and wrt are semaphores with initial values as 1 and 1 respectively and readcount is the variable for counting the current number of readers reading the file with initial value as 0. <br />
### The psedocode for semaphores is given below(As given in slides) :
```cpp
typedef struct{
  int value;
  struct process *list;
} semaphore;

wait(semaphore *S){
  S->value--;
  if(S->value < 0){
    add this process to S->list;
    block();
  }
}

signal(semaphore *S){
  S->value++;
  if(S->value <= 0){
    remove a process P from S->list;
    wakeup(P);
  }
}
```

### Starve free Solution for Readers Writers Problem :

```cpp
wait(que);
wait(mutex);
readcount++;
if (readcount== 1) wait(wrt);
signal(mutex);
signal(que);
.
.
.
reading is performed
.
.
.
wait(mutex);
readcount--;
if (readcount== 0) signal(wrt);
signal(mutex):
```
Writer Process :
```cpp
wait(que);
wait(wrt);
.
.
.
writing is performed
.
.
.
signal(wrt);
signal(que);
```
Explanation : <br />
This solution is similar to the previous solution, the only difference is that is uses an extra semaphore que with an initial value of 1. Due to this semaphore, all the processes are arranged in a FIFO queue and hence during reading if any writer comes, it is stored in the queue. Hence, it follows FCFS scheduling as the queue is FIFO. Hence, there is no starving for readers or writers in this case.

