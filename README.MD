# Docker Devcontainer Toolchains for Raspberry Pi

VSCode's devcontainers make life a lot easier. Instead of having to set up the development environment every time, the environment resides in a docker container, which is spun up automatically. This way we can focus on coding and not on setting up the environment just right.

For remote work I have my environments running on a Raspberry Pi. Microsoft does not provide any devcontainers for the Raspberry, so I had to build them myself. Depending on the type of project, I use a different decvontainer. While all the containers are stored on the dockerhub, so that I don't have to build them from scratch every time, I keep the original Dockerfiles in this repository, so I can change them if needed.

All toolchains are ready to be used within a .devcontainer environment for VS Code, but can also be used stand-alone for testing purposes. Keep in mind that the container contents are destroyed when the container is stopped.

## ARM toolchain

Toolchain for ARM (linux and none-eabi) development. Available on docker hub (hub.docker.com) as **w1gx/rpi-arm-toolchain**

- gcc (11.3.0)
- cmake (3.22.1)
- gcc-arm-none-eabi (10.3.1)
- doxygen (1.9.1)
- mscgen (0.20)
- cpplint (1.5.5)
- ceedling (0.31.1) with Unity (2.5.4)
- python3 (3.10.6)
- git (2.34.1)

### Building the ARM toolchain

```bash
docker build -t rpi-arm-toolchain ./rpi-arm-toolchain
```

### Running the ARM toolchain

```bash
docker run -it rpi-arm-toolchain
```

## Risc-V toolchain

Toolchain for RISC-V development (riscv64). Contains not only the toolchain, but also a Qemu environment so that code can be tested without the need to connect to actual RISC-V hardware.  Available on docker hub (hub.docker.com) as **w1gx/rpi-riscv-toolchain**

- gcc (11.3.0)
- cmake (3.22.1)
- riscv64-unknown-linux-gnu-gcc (12.2.0)
- python3 (3.10.6)
- git (2.34.1)
- qemu-riscv64 (7.2.0)

### Building the RISCV toolchain

```bash
docker build -t rpi-riscv-toolchain ./rpi-riscv-toolchain
```

### Running the toolchain

```bash
docker run -it rpi-riscv-toolchain
```

### Emulating RISC-V

```bash
# start docker container
$ docker run -it rpi-riscv-toolchain
# create test.c
$ cat <<EOF > test.c
#include <stdio.h>
int main() {
    printf("This is a RISC-V test\n");
}
EOF
# cross-compile test.c
$ riscv64-unknown-linux-gnu-gcc -o test test.c
# show architecture of compiled 'test'
$ readelf -A test
# run test in qemu environment
$ qemu-riscv64 -L /opt/riscv/sysroot test 
# 
```

