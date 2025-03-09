# edu-raspberry-os

> Add install target.

## Instructions

### Add target install

### Create heap and stack targets

```bash
cat > ./src/CMakeLists.txt << EOF
add_executable(heap heap.c)
add_executable(stack stack.c)

# Make sure mpack is found
find_package(mpack REQUIRED)

# Link the correct mpack target
target_link_libraries(heap PRIVATE mpack-static)
target_link_libraries(stack PRIVATE mpack-static)
EOF
```

### Add FetchContent for external packages

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

include(FetchContent)
FetchContent_Declare(
    mpack
    GIT_REPOSITORY https://github.com/ludocode/mpack.git
    GIT_TAG        v1.1
)
FetchContent_MakeAvailable(mpack)

add_subdirectory(src)

# Explicitly link mpack
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
        fprintf(stderr, "Heap allocation failed\\n");
        return 1;
    }

    printf("Heap memory allocated successfully\\n");

    free(ptr);
    printf("Heap memory freed\\n");

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

    printf("Stack memory allocated and used successfully\\n");
}

int main() {
    stackFunction();
    return 0;
}
EOF
```

## Test it

```bash
rm -rf build
cmake -B build
make -C build
sudo make -C build install

heap
stack
which heap
which stack
```
