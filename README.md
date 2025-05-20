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



     **rr**
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



