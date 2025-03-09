# edu-raspberry-os

## Instructions


### CMakeLists.txt

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

include(FetchContent)

# Fetch pigpio (GPIO control library)
FetchContent_Declare(
    pigpio
    GIT_REPOSITORY https://github.com/joan2937/pigpio.git
    GIT_TAG        master
)
FetchContent_MakeAvailable(pigpio)

add_subdirectory(src)

install(TARGETS heap stack blink DESTINATION bin)

# Custom clean target
add_custom_target(clean-all
    COMMAND rm -rf build bin
    COMMENT "Removing all build artifacts and installed binaries"
)
EOF
```

### ./src/CmakeLists.txt

```bash
cat > src/CMakeLists.txt << EOF
add_executable(heap heap.c)
add_executable(stack stack.c)
add_executable(blink blink.c)

# Link pigpio
target_link_libraries(blink PRIVATE pigpio)
EOF
```

### ./src/blink.c

```bash
cat > src/blink.c << EOF
#include <stdio.h>
#include <pigpio.h>

#define LED_PIN 17  // GPIO 17

int main() {
    if (gpioInitialise() < 0) {
        fprintf(stderr, "ERROR: pigpio initialization failed!\\n");
        return 1;
    }

    gpioSetMode(LED_PIN, PI_OUTPUT);
    printf("INFO: Blinking LED on GPIO 17\\n");

    for (int i = 0; i < 10; i++) {
        gpioWrite(LED_PIN, 1);
        printf("INFO: LED ON\\n");
        gpioDelay(500000);  // 500ms

        gpioWrite(LED_PIN, 0);
        printf("INFO: LED OFF\\n");
        gpioDelay(500000);  // 500ms
    }

    gpioTerminate();
    printf("INFO: Blink test complete\\n");
    return 0;
}
EOF
```

## Build it

```bash
rm -rf build
cmake -S . -B build
make -C build
sudo make -C build install
```

## Test it

```bash
sudo blink
```

## Restart it

```bash
git reset --hard
git clean -df
```
