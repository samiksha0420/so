**FCFS**
class process:
    def __init__(self, pid, arrival_time, burst_time):
        self.pid=pid
        self.arrival_time=arrival_time
        self.burst_time=burst_time
        self.complition_time=0
        self.turnaround_time=0
        self.wait_time=0

def get_arrival_time(process):
    return process.arrival_time

def fcfs(pro):
    pro.sort(key=get_arrival_time)

    current_time=0
    for process in pro:
        if current_time < process.arrival_time:
            current_time = process.arrival_time
        process.complition_time = current_time + process.burst_time
        process.turnaround_time = process.complition_time - process.arrival_time
        process.wait_time = process.turnaround_time - process.burst_time
        current_time=process.complition_time

def display(pro):
    print("PID \t AT \t BT \t CT \t TAT \t WT")

    for p in pro:
        print(f"{p.pid} \t {p.arrival_time} \t {p.burst_time} \t {p.complition_time} \t {p.turnaround_time} \t {p.wait_time}")

# main funtion 
if __name__ == "__main__":
    
    process_list = [
        process("P1", 0, 5),
        process("P2", 1, 3),
        process("P3", 2, 8),
        process("P4", 3, 6)
    ]

    fcfs(process_list)
    display(process_list)


*******************************************
**np-sjf**
class process:
    def __init__(self, pid, arrival_time, burst_time):
        self.pid=pid
        self.arrival_time=arrival_time
        self.burst_time=burst_time
        self.complition_time=0
        self.turnaround_time=0
        self.wait_time=0

def get_arrival_time(process):
    return process.arrival_time

def fcfs(pro):
    pro.sort(key=get_arrival_time)

    current_time=0
    for process in pro:
        if current_time < process.arrival_time:
            current_time = process.arrival_time
        process.complition_time = current_time + process.burst_time
        process.turnaround_time = process.complition_time - process.arrival_time
        process.wait_time = process.turnaround_time - process.burst_time
        current_time=process.complition_time

def display(pro):
    print("PID \t AT \t BT \t CT \t TAT \t WT")

    for p in pro:
        print(f"{p.pid} \t {p.arrival_time} \t {p.burst_time} \t {p.complition_time} \t {p.turnaround_time} \t {p.wait_time}")

# main funtion 
if __name__ == "__main__":
    
    process_list = [
        process("P1", 0, 5),
        process("P2", 1, 3),
        process("P3", 2, 8),
        process("P4", 3, 6)
    ]

    fcfs(process_list)
    display(process_list)

************************************
**p-sjf**
class process:
    def __init__ (self, pid, at, bt):
        self.pid = pid
        self.at= at
        self.bt=bt
        self.rt=bt
        self.ct=0
        self.tat=0
        self.wt=0
        self.start_time=-1

def sjfp (process):
    n=len(process)
    current_time=0
    complete =0
    shortest = None

    while complete < n:
        ready_queue= [p for p in process if p.at<= current_time and p.rt>0]

        if ready_queue:
            shortest_job = min(ready_queue, key=lambda x: x.rt)

            if shortest_job.start_time == -1: #if first job enters
                shortest_job.start_time = current_time

            shortest_job.rt -= 1 #run for 1 unit time
            current_time += 1

            if shortest_job.rt == 0:
                shortest_job.ct = current_time
                shortest_job.tat = shortest_job.ct - shortest_job.at
                shortest_job.wt = shortest_job.tat - shortest_job.bt
                complete += 1


        else :
            current_time += 1

def display(process):
    print("pid \t at \t bt \t ct \t tat \t wt")

    for p in process:
        print(f"{p.pid} \t {p.at} \t {p.bt} \t {p.ct} \t {p.tat} \t {p.wt}" )


if __name__ == "__main__":
     process_list = [
        process("P1", 0, 8),
        process("P2", 1, 4),
        process("P3", 2, 9),
        process("P4", 3, 5)
    ]
     sjfp(process_list)
     display(process_list)



*************************

**ps**
class process:
    def __init__(self, pid, at, bt, priority):
        self.pid = pid
        self.at= at
        self.bt=bt 
        self.priority=priority
        self.ct=0
        self.tat=0
        self.wt=0 
        self.is_complete= False

def priority_shedule(process):

    n = len(process)
    current_time = 0
    complete = 0

    while complete < n:
        ready_queue = [p for p in process if p.at<=current_time and not p.is_complete]

        if ready_queue:
            high_priority = min(ready_queue, key=lambda x: x.priority)

            current_time += high_priority.bt
            high_priority.ct = current_time
            high_priority.tat = high_priority.ct - high_priority.at
            high_priority.wt = high_priority.tat - high_priority.bt
            complete += 1
            high_priority.is_complete = True

        else:
            current_time += 1
def display(process):
    print("pid \t at \t bt \t ct \t tat \t wt \t priority")

    for p in process:
        print(f"{p.pid} \t {p.at} \t {p.bt} \t {p.ct} \t {p.tat} \t {p.wt} \t {p.priority}" )


if __name__ == "__main__":
     process_list = [
        process("P1", 0, 6, 2),
        process("P2", 1, 8, 1),
        process("P3", 2, 7, 3),
        process("P4", 3, 3, 2)
    ]
     
     priority_shedule(process_list)
     display(process_list)


************************
     **rr****
     class process:
    def __init__ (self, pid, at, bt):
        self.pid = pid
        self.at= at
        self.bt=bt
        self.rt=bt
        self.ct=0
        self.tat=0
        self.wt=0

def rr(process, time_quantum):
    n = len(process)
    queue=[]
    time = 0
    complete=0
    process.sort(key=lambda x: x.at)
    visited =[False] * n

    while complete < n:
        for i in range(n):
            if process[i].at<time and not visited[i]:
                queue.append(process[i])

            if not queue:
                time+=1
                continue

            current_process = queue.pop(0)
            run_time = min(time_quantum, current_process.rt)
            time += run_time
            current_process.rt -= run_time

            for i in range(n):
                if (process[i].at>time-run_time and process[i].at<time and not visited[i]):
                    queue.append(process[i])
                    visited[i] = True

            if current_process.rt ==0:
                current_process.ct=time
                current_process.tat = time - current_process.at
                current_process.wt = current_process.tat -current_process.bt
                complete += 1
            
            else:
                queue.append(current_process)


def display(process):
    print("pid \t at \t bt \t ct \t tat \t wt")

    for p in process:
        print(f"{p.pid} \t {p.at} \t {p.bt} \t {p.ct} \t {p.tat} \t {p.wt}" )


if __name__ == "__main__":
    process_list = [
        process("P1", 0, 5),
        process("P2", 1, 4),
        process("P3", 2, 2),
        process("P4", 3, 1)
    ]
    time_quantum = 2

    rr(process_list, time_quantum)
    display(process_list)



**********************************
**prducer-consumer**

import threading
import time
import random

# Shared buffer
buffer = []
buffer_size = 5

# Semaphores
empty_slots = threading.Semaphore(buffer_size)  # initially all slots are empty
full_slots = threading.Semaphore(0)             # initially no filled slot

# Mutex lock
mutex = threading.Lock()

# Producer thread function
def producer():
    for i in range(10):
        item = random.randint(1, 100)
        empty_slots.acquire()         # wait if buffer is full
        mutex.acquire()              # acquire lock to access buffer
        buffer.append(item)
        print(f"Producer produced: {item}")
        mutex.release()              # release lock
        full_slots.release()        # signal that a new item is available
        time.sleep(random.random()) # simulate work

# Consumer thread function
def consumer():
    for i in range(10):
        full_slots.acquire()         # wait if buffer is empty
        mutex.acquire()              # acquire lock
        item = buffer.pop(0)
        print(f"Consumer consumed: {item}")
        mutex.release()              # release lock
        empty_slots.release()       # signal that a slot is now empty
        time.sleep(random.random()) # simulate work

# Main function
if __name__ == "__main__":
    # Create threads
    producer_thread = threading.Thread(target=producer)
    consumer_thread = threading.Thread(target=consumer)

    # Start threads
    producer_thread.start()
    consumer_thread.start()

    # Wait for threads to finish
    producer_thread.join()
    consumer_thread.join()

    print("Finished Producer-Consumer simulation.")



*************************
**thread-management**
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

#define MAX 10

int mat1[MAX][MAX], mat2[MAX][MAX], result[MAX][MAX];
int r1, c1, r2, c2;

// Structure to hold row and column info for each thread
typedef struct {
    int row;
    int col;
} Cell;

void* multiply(void* arg) {
    Cell* data = (Cell*) arg;
    int sum = 0;
    for (int i = 0; i < c1; i++) {
        sum += mat1[data->row][i] * mat2[i][data->col];
    }

    int* res = malloc(sizeof(int));
    *res = sum;
    pthread_exit((void*) res);  // Return result
}

int main() {
    printf("Enter rows and columns of matrix 1: ");
    scanf("%d %d", &r1, &c1);
    printf("Enter rows and columns of matrix 2: ");
    scanf("%d %d", &r2, &c2);

    if (c1 != r2) {
        printf("Matrix multiplication not possible\n");
        return 1;
    }

    printf("Enter elements of matrix 1:\n");
    for (int i = 0; i < r1; i++)
        for (int j = 0; j < c1; j++)
            scanf("%d", &mat1[i][j]);

    printf("Enter elements of matrix 2:\n");
    for (int i = 0; i < r2; i++)
        for (int j = 0; j < c2; j++)
            scanf("%d", &mat2[i][j]);

    pthread_t threads[r1 * c2];
    int thread_index = 0;

    // Create threads for each cell
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            Cell* data = (Cell*) malloc(sizeof(Cell));
            data->row = i;
            data->col = j;
            pthread_create(&threads[thread_index], NULL, multiply, data);
            thread_index++;
        }
    }

    // Join threads and collect results
    thread_index = 0;
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            void* ret_val;
            pthread_join(threads[thread_index], &ret_val);
            result[i][j] = *(int*)ret_val;
            free(ret_val); // Free dynamically allocated memory
            thread_index++;
        }
    }

    // Print result matrix
    printf("Resultant Matrix:\n");
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }

    return 0;
}



**************************
**thread cycle**
import threading
import time

# Define a thread class
class MyThread(threading.Thread):
    def run(self):
        print(f"Thread {self.name} is starting.")
        time.sleep(2)  # Simulate some work
        print(f"Thread {self.name} is running.")
        time.sleep(2)  # Simulate more work
        print(f"Thread {self.name} has finished.")

# Main program
if __name__ == "__main__":
    print("Main thread: creating a new thread.")

    t = MyThread()           # Create thread object
    t.start()                # Start the thread (calls run())
    print("Main thread: waiting for the thread to finish.")

    t.join()                 # Wait for thread to finish
    print("Main thread: thread has completed. Exiting program.")

