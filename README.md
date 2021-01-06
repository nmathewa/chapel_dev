# chapel_dev
# Data parallelism

## for loop for data parallelism

for all ===== > **coforall** in chapel

without coforall loop 
```cpp
const n = 1000000;
var A: [1..n] real;

for a in A do
  a += 1.0;

```

approx 9.8 seconds to run


with coforall loop
```cpp
const n = 1000000;
var A: [1..n] real;

coforall a in A do
  a += 1.0;

```

I got same 9.8 seconds to compute


> However, a coforall-loop would likely be overkill for this situation since it would create one million tasks, each of which is only responsible for incrementing a single element. For most hardware platforms, this would generate more concurrency than the system could execute in parallel. Moreover, the cost of creating, scheduling, and destroying the tasks would likely overshadow the small amount of computation that each one performs. Better would be to create a smaller number of tasks and assign a number of loop iterations to each, as a means of amortizing the task creation overheads while avoiding overwhelming the target system with too much concurrency.

```BASH
export CHPL_COMM=gasnet

```
the above setup give an error

