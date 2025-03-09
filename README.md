# edu-raspberry-os

> Add install target.

## Instructions

### Add target install

### Create heap and stack targets

```bash
cat > ./src/CMakeLists.txt << EOF
add_executable(heap heap.c)
add_executable(stack stack.c)

install(TARGETS heap stack DESTINATION bin)
EOF
```

### Add FetchContent for external packages

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

add_subdirectory(src)

install(TARGETS heap stack DESTINATION bin)
EOF
```

### Heap

```bash
cat > ./src/heap.c << EOF
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr = malloc(sizeof(int) * 10);
    if (!ptr) {
        fprintf(stderr, "ERROR: Heap allocation failed\\n");
        return 1;
    }

    printf("INFO: Heap memory allocated successfully\\n");

    free(ptr);
    printf("INFO: Heap memory freed\\n");

    fprintf(stderr, "WARNING: Heap allocation test complete\\n");

    return 0;
}
EOF
```

### Stack

```bash
cat > ./src/stack.c << EOF
#include <stdio.h>

void stackFunction() {
    int arr[10]; // Stack allocation
    for (int i = 0; i < 10; i++) {
        arr[i] = i;
    }

    printf("INFO: Stack memory allocated and used successfully\\n");
    fprintf(stderr, "WARNING: Stack operation completed\\n");
}

int main() {
    stackFunction();
    return 0;
}
EOF
```

## Build and install it

```bash
rm -rf build
cmake -S . -B build
make -C build
sudo make -C build install
```

## Test it

```bash
heap
stack
which heap
which stack
```
