# Threading vs Multiprocessing vs Asyncio (Python)

## Comparison Table

| Feature | Threading | Multiprocessing | Asyncio |
|--------|--------|--------|--------|
| Concurrency Type | Multiple threads in one process | Multiple processes | Multiple coroutines in one thread |
| Memory | Shared memory | Separate memory | Shared memory |
| True Parallelism | No (due to GIL for CPU work) | Yes | No |
| Best For | Network I/O, file I/O, database I/O | CPU-bound tasks | Massive I/O concurrency |
| Communication Cost | Low | High (IPC required) | Very Low |

---

## 1. Fundamental Differences

### 1.1 Threading

Threading creates multiple execution paths (threads) inside the same process.

```text
Process
│
├── Thread 1
├── Thread 2
└── Thread 3
```

All threads:
- Share memory
- Share variables
- Share resources

### 1.2 Multiprocessing

Multiprocessing creates completely separate processes.

Each process:
- Has its own Python interpreter
- Has its own memory
- Has its own GIL

This allows true parallel execution on multiple CPU cores.

### 1.3 Asyncio

Asyncio does not create threads or processes. Instead it uses:

- One thread
- One event loop
- Multiple coroutines

---

## 2. When to Use What?

### 2.1 Threading → I/O Bound Tasks

Use threading when tasks spend most of their time waiting.

Examples:
- API calls
- HTTP requests
- Database queries
- Reading files
- Waiting on sockets

```python
requests.get(...)
```

### 2.2 Multiprocessing → CPU Bound Tasks

Use multiprocessing when tasks consume CPU heavily.

Examples:
- Machine Learning preprocessing
- Data transformations
- Image processing
- Video encoding
- Mathematical simulations

```python
for i in range(1_000_000_000):
    total += i
```

### 2.3 Asyncio → Large Scale I/O

Use asyncio when handling thousands of I/O operations.

Examples:
- AI agents
- FastAPI applications
- Web scrapers
- Chat servers
- LLM orchestration

```python
await http_request()
```

---

## 3. How Threading Works Despite the GIL

### What is GIL?

GIL = Global Interpreter Lock.

Only one thread can execute Python bytecode at a time.

```text
Thread 1 -> Running Python
Thread 2 -> Waiting

<Switch>

Thread 2 -> Running Python
Thread 1 -> Waiting
```

### Then Why Use Threading?

Because I/O operations release the GIL.

```python
response = requests.get(url)
```

While waiting for the network response:
- Thread 1 waits.
- The GIL is released.
- Thread 2 can execute.

---

## 4. Disadvantages of Multiprocessing

### 4.1 High Memory Usage

Each process has separate memory.

> Four processes ≈ Four Python interpreters.

### 4.2 Data Copying Overhead

Processes cannot directly share memory.

### 4.3 Slower Startup

Creating a process is slower than creating a thread.

### 4.4 IPC Complexity

Inter-process communication requires mechanisms such as:
- Queue
- Pipe
- SharedMemory
- Manager

---

## 5. Threading Example (Downloading Multiple URLs)

```python
from concurrent.futures import ThreadPoolExecutor
import requests

urls = [
    "https://google.com",
    "https://github.com",
    "https://python.org"
]

def fetch(url):
    response = requests.get(url)
    return f"{url}: {response.status_code}"

with ThreadPoolExecutor(max_workers=3) as executor:
    results = executor.map(fetch, urls)

for result in results:
    print(result)
```

Execution:

```text
Thread 1 -> Google
Thread 2 -> Github
Thread 3 -> Python.org
```

All requests wait simultaneously.

---

## 6. Multiprocessing Example (CPU Intensive Task)

```python
from concurrent.futures import ProcessPoolExecutor

def square_sum(n):
    total = 0
    for i in range(n):
        total += i * i
    return total

numbers = [
    10_000_000,
    20_000_000,
    30_000_000
]

with ProcessPoolExecutor() as executor:
    results = executor.map(square_sum, numbers)

for result in results:
    print(result)
```

Execution:

```text
CPU Core 1 -> Task 1
CPU Core 2 -> Task 2
CPU Core 3 -> Task 3
```

True parallelism.

---

## 7. How Asyncio Works on a Single Thread

**Key Idea:** Tasks voluntarily give up control.

```python
await asyncio.sleep(3)
```

Means:
- Pause me.
- Run someone else.
- Wake me later.

```python
import asyncio

async def task1():
    print("Task 1 Started")
    await asyncio.sleep(3)
    print("Task 1 Finished")

async def task2():
    print("Task 2 Started")
    await asyncio.sleep(2)
    print("Task 2 Finished")

async def main():
    await asyncio.gather(task1(), task2())

asyncio.run(main())
```

Execution:

```text
0s Task 1 Started
0s Task 2 Started
2s Task 2 Finished
3s Task 1 Finished
```

---

## 8. Asyncio HTTP Request Example

```python
import asyncio
import aiohttp

urls = [
    "https://httpbin.org/delay/2",
    "https://httpbin.org/delay/2",
    "https://httpbin.org/delay/2"
]

async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()

async def main():
    async with aiohttp.ClientSession() as session:
        tasks = [fetch(session, url) for url in urls]
        results = await asyncio.gather(*tasks)  # Unpacks the coroutines.
        print(f"Received {len(results)} responses")

asyncio.run(main())
```

Execution:

```text
0s Request 1 sent
0s Request 2 sent
0s Request 3 sent

2s Response 1 received
2s Response 2 received
2s Response 3 received
```

The event loop sends all requests immediately and switches between coroutines while waiting for network responses.
