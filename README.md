# perf
A C89 standard compliant, single header, nostdlib (no C Standard Library) simple performance profiler.

For more information please look at the "perf.h" file or take a look at the "examples" or "tests" folder.

> [!WARNING]
> THIS PROJECT IS A WORK IN PROGRESS! ANYTHING CAN CHANGE AT ANY MOMENT WITHOUT ANY NOTICE! USE THIS PROJECT AT YOUR OWN RISK!

## Quick Start

Download or clone perf.h and include it in your project.

```C
#include "perf.h" /* Performance Profiler */

/* Just an example function that does some operations */
void test_function(double a, double b)
{
  int i;
  for (i = 0; i < 100; ++i)
  {
    a += a + b;
  }
}

int main() {

    /* Simply wrap your function or void block in a PERF_PROFILE call */
    PERF_PROFILE(test_function(5.0, 2.23));

    /* In this example we also profile the perf function calls themselv and give it a name */
    PERF_PROFILE_WITH_NAME(
        {
            PERF_PROFILE(test_function(1.0, 3.0));
            PERF_PROFILE_WITH_NAME(test_function(1.0, 3.0), "custom_name");
        },
        "profile_perf_calls");

    return 0;
}
```

The profiling results are then printed out like this:

```txt
perf_test.c:27 [perf]           41 cycles,     0.000001 ms, "test_function(5.0, 2.23)"
perf_test.c:31 [perf]           45 cycles,     0.000000 ms, "test_function(1.0, 3.0)"
perf_test.c:32 [perf]           69 cycles,     0.000003 ms, "custom_name"
perf_test.c:29 [perf]      5032741 cycles,     0.022399 ms, "profile_perf_calls"
```

### Disable Performance Profiling
If you placed the PERF_ calls in your code base but want to switch off the functionality without loosing any performance you can specify -DPERF_DISABLE or #define it like the following example.

```C
#define PERF_DISABLE
#include "perf.h"
```

## Run Example: nostdlib, freestsanding

In this repo you will find the "examples/perf_win32_nostdlib.c" with the corresponding "build.bat" file which
creates an executable only linked to "kernel32" and is not using the C standard library and executes the program afterwards.

## "nostdlib" Motivation & Purpose

nostdlib is a lightweight, minimalistic approach to C development that removes dependencies on the standard library. The motivation behind this project is to provide developers with greater control over their code by eliminating unnecessary overhead, reducing binary size, and enabling deployment in resource-constrained environments.

Many modern development environments rely heavily on the standard library, which, while convenient, introduces unnecessary bloat, security risks, and unpredictable dependencies. nostdlib aims to give developers fine-grained control over memory management, execution flow, and system calls by working directly with the underlying platform.

### Benefits

#### Minimal overhead
By removing the standard library, nostdlib significantly reduces runtime overhead, allowing for faster execution and smaller binary sizes.

#### Increased security
Standard libraries often include unnecessary functions that increase the attack surface of an application. nostdlib mitigates security risks by removing unused and potentially vulnerable components.

#### Reduced binary size
Without linking to the standard library, binaries are smaller, making them ideal for embedded systems, bootloaders, and operating systems where storage is limited.

#### Enhanced performance
Direct control over system calls and memory management leads to performance gains by eliminating abstraction layers imposed by standard libraries.

#### Better portability
By relying only on fundamental system interfaces, nostdlib allows for easier porting across different platforms without worrying about standard library availability.
