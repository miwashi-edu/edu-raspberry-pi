# edu-raspberry-os

> Add install target.

## Instructions

### Add target install

### Replace hello target with hello1 and hello2 using same cpp

```bash
cat > ./src/CMakeLists.txt << EOF
add_executable(hello1 main.cpp)
add_executable(hello2 main.cpp)
EOF
```

```bash
cat > CMakeLists.txt << EOF
cmake_minimum_required(VERSION 3.16)
project(myproject LANGUAGES CXX)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY \${CMAKE_SOURCE_DIR}/bin)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_subdirectory(src)

install(TARGETS hello1 hello2 DESTINATION bin) # Added install target
EOF
```

## Test it

> We need sudo as we copy files to /usr/local/bin
> The target is chained, first it runs target build, then target install
> If build failes, install is not run

```bash
sudo make -C build install
hello1
hello2
which hello1
which hello2
```
