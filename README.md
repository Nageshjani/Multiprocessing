## Creating processes using Process class

```python
from multiprocessing import Process

def my_function(name):
    print("Hello", name)

if __name__ == '__main__':
    p = Process(target=my_function, args=('John',))
    p.start()

```





## Starting and stopping processes

```python
from multiprocessing import Process
import time

def my_function():
    print("Process started")
    time.sleep(5)
    print("Process stopped")

if __name__ == '__main__':
    p = Process(target=my_function)
    p.start()
    time.sleep(2)
    p.terminate()

```

## Passing Arguments

```python
from multiprocessing import Process

def my_function(name):
    print("Hello", name)

if __name__ == '__main__':
    p = Process(target=my_function, args=('John',))
    p.start()
   
```


## Sharing data between processes

```python
from multiprocessing import Process, Value

def my_function(counter):
    for i in range(5):
        counter.value += 1
    print("Counter: ", counter.value)

if __name__ == '__main__':
    counter = Value('i', 0)
    p1 = Process(target=my_function, args=(counter,))
    p2 = Process(target=my_function, args=(counter,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()


```



## Inter-process communication

```python
from multiprocessing import Process, Pipe

def send_messages(conn):
    conn.send("Hello from sender process!")
    conn.close()

def receive_messages(conn):
    print(conn.recv())
    conn.close()

if __name__ == '__main__':
    sender_conn, receiver_conn = Pipe()
    p1 = Process(target=send_messages, args=(sender_conn,))
    p2 = Process(target=receive_messages, args=(receiver_conn,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()


```


## Shared Memory

```python
from multiprocessing import Process, Value

def add_one(num):
    num.value += 1

if __name__ == '__main__':
    num = Value('i', 0)
    p1 = Process(target=add_one, args=(num,))
    p2 = Process(target=add_one, args=(num,))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
    print(num.value)

```


## Lock

```python
from multiprocessing import Process, Lock

def add_one(num, lock):
    lock.acquire()
    num.value += 1
    lock.release()

if __name__ == '__main__':
    num = Value('i', 0)
    lock = Lock()
    p1 = Process(target=add_one, args=(num, lock))
    p2 = Process(target=add_one, args=(num, lock))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
    print(num.value)

```


## Condition

```python
from multiprocessing import Process, Condition

def producer(items, condition):
    for i in range(5):
        condition.acquire()
        items.append(i)
        print(f"Produced: {i}")
        condition.notify()
        condition.release()

def consumer(items, condition):
    for i in range(5):
        condition.acquire()
        while not items:
            condition.wait()
        item = items.pop(0)
        print(f"Consumed: {item}")
        condition.release()

if __name__ == '__main__':
    items = []
    condition = Condition()
    p1 = Process(target=producer, args=(items, condition))
    p2 = Process(target=consumer, args=(items, condition))
    p1.start()
    p2.start()
    p1.join()
    p2.join()
```


## Parallelizing tasks using multiple processes

```python
from multiprocessing import Process
import time

def task(number):
    time.sleep(2)
    print(f"Task {number} completed")

if __name__ == '__main__':
    processes = []
    for i in range(4):
        p = Process(target=task, args=(i,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

```


## Pool Processes

```python
from multiprocessing import Pool
import time

def task(number):
    time.sleep(2)
    return f"Task {number} completed"

if __name__ == '__main__':
    pool = Pool(processes=4)
    results = []
    for i in range(4):
        result = pool.apply_async(task, args=(i,))
        results.append(result)

    for result in results:
        print(result.get())

```


# Map Reduced

```python
from multiprocessing import Pool

def square(number):
    return number ** 2

if __name__ == '__main__':
    numbers = [1, 2, 3, 4, 5]
    pool = Pool(processes=4)
    results = pool.map(square, numbers)
    print(results)

```

## I/O Operations

```python
from multiprocessing import Process, Queue
import time

def producer(queue):
    for i in range(5):
        queue.put(i)
        time.sleep(1)

def consumer(queue):
    while True:
        item = queue.get()
        print(f"Consumed: {item}")
        time.sleep(1)

if __name__ == '__main__':
    queue = Queue()
    p1 = Process(target=producer, args=(queue,))
    p2 = Process(target=consumer, args=(queue,))
    p1.start()
    p2.start()
    p1.join()
    p2.terminate()

```



## Process pools and thread pools


```python
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor
import time

def task(number):
    time.sleep(2)
    return f"Task {number} completed"

if __name__ == '__main__':
    with ProcessPoolExecutor(max_workers=4) as executor:
        results = [executor.submit(task, i) for i in range(4)]
        for result in results:
            print(result.result())

    with ThreadPoolExecutor(max_workers=4) as executor:
        results = [executor.submit(task, i) for i in range(4)]
        for result in results:
            print(result.result())

```

## Asyncio

```python

import asyncio
from concurrent.futures import ProcessPoolExecutor

async def compute(number):
    loop = asyncio.get_running_loop()
    with ProcessPoolExecutor() as executor:
        return await loop.run_in_executor(executor, task, number)

async def main():
    tasks = [asyncio.create_task(compute(i)) for i in range(4)]
    results = await asyncio.gather(*tasks)
    print(results)

if __name__ == '__main__':
    asyncio.run(main())

```


## Multiprocessing and distributed computing

```python
from multiprocessing.managers import BaseManager

class Calculator:
    def add(self, x, y):
        return x + y

if __name__ == '__main__':
    BaseManager.register('Calculator', Calculator)
    manager = BaseManager(address=('localhost', 5000), authkey=b'password')
    manager.start()
    calculator = manager.Calculator()
    print(calculator.add(2, 3))

```


## Performance tuning with multiprocessing

```python
from multiprocessing import Process, Value

def add_one(num):
    num.value += 1

if __name__ == '__main__':
    num = Value('i', 0)
    processes = []
    for i in range(4):
        p = Process(target=add

```