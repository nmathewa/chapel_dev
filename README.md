# chapel_dev

## 1. Chapel installation 

Below is my system specific installation of chapel using only host communication layer (GASNET is used)
> CPU: AMD RYZEN 5 2500U quad core 8 thread 
> arch : x86_64
> memory : 8 GB (6.9)

### Prerequesties

- Based on this [documentation](https://chapel-lang.org/docs/usingchapel/QUICKSTART.html)
-  For debian based systems 
    ```bash
    sudo apt-get install gcc g++ m4 perl python python-dev python-setuptools bash make mawk git pkg-config
    ```
- Download chapel tar file from official GIT repo --- [link](https://github.com/chapel-lang/chapel/releases/download/1.23.0/chapel-1.23.0.tar.gz)
- Extract the tar file ``` tar xzf chapel-1.23.0.tar.gz```
- The complete list of enviornment varibles are listed in this [link](https://chapel-lang.org/docs/usingchapel/chplenv.html)
- set the chapel home directory ```export CHPL_HOME=~/chapel-1.23.0```
- set chapel path dir ```export PATH="$PATH":"$CHPL_HOME/bin/$CHPL_BIN_SUBDIR" ```
- set man path for chapel ```export export MANPATH="$MANPATH":"$CHPL_HOME"/man```
- set host platform (host is the principle system) ```export CHPL_HOST_PLATFORM=`$CHPL_HOME/util/chplenv/chpl_platform.py````
- set system arch ```export CHPL_HOST_ARCH=x86_64```
- set the communication layer as GASNET ```export CHPL_COMM=gasnet```

### Installing chapel

- using GNU make go to chapel directory

```bash
     cd $CHPL_HOME
     ./configure
     make
     sudo make install
```
- Test if the build is successful by
```bash
   chpl --help
```

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

