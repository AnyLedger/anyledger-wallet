# anyledger-wallet

## Overview

🏦 AnyLedger Embedded Wallet for Internet of Things

C crypto libraries to safely manage, generate and store private keys, to generate blockchain addresses and to sign transactions. 

Additionally, it contains a RTOS app ([Zephyr](https://github.com/AnyLedger/zephyr)).
The current implementation uses Ethereum and HyperLedger, but the same architecture will be used for all the other chains. The current implementation supports [100+ hardware platforms](https://docs.zephyrproject.org/latest/boards/index.html) and is mainly bug tested on Nordic nRF52840.

Any IoT device flashing the embedded wallet can safely connect to a given blockchain, do transactions and interact with smart contracts. 

## Contributing

Have a look at our [contribution guidelines](https://github.com/AnyLedger/anyledger-wallet/blob/master/CONTRIBUTING.md).

## Compilation

### Dependencies

##### Linux (Ubuntu)
On a fresh Ubuntu:18.04 docker image. Adapt as appropriate to your distro.
```
apt-get update
apt-get install libgtest-dev g++ make cmake wget build-essential clang clang-tidy ninja-build git python3-pip protobuf-compiler python-protobuf -y

# Ubuntu's libgtest-dev is only the source. You have to compile them manually.
cd /usr/src/gtest; cmake .; cmake --build . --target install 

# Install Ethereum Solidity compiler
apt-get install software-properties-common -y && add-apt-repository ppa:ethereum/ethereum -y && apt-get update
apt-get install solc -y

# without this, you will get a Python 3 ASCII/UTF8 error on Ubuntu
export LC_ALL=C.UTF-8
export LANG=C.UTF-8
cd /
git clone https://github.com/AnyLedger/anyledger-wallet

alias build='rm -rf build; mkdir build; cd build; cmake -GNinja ../; ninja; cd ..'

cd anyledger-wallet; pip3 install --user -r requirements.txt; build
```

##### macOS

GTest:

```
git clone https://github.com/google/googletest 
cd googletest  
mkdir build  
cd build  
cmake ..  
make  
make install
```

Additional dependencies (installed using [Homebrew](https://brew.sh)):

`brew install gcc make cmake wget ninja nanopb-generator protobuf pkgconfig`

#### Python (this is required for ABI compiler)
`pip3 install --user -r requirements.txt`

#### solidity compiler
A working `solc` should be available in your `$PATH`. See the [Installation instructions](https://solidity.readthedocs.io/en/v0.5.7/installing-solidity.html).

### x86 build

This build is used to develop and test the wallet library on your desktop.

1) clone wallet repo
`git clone git@github.com:AnyLedger/anyledger-wallet.git && cd anyledger-wallet`

2) create build directory and generate build targets
`mkdir build && cd build && cmake -GNinja ../`

3) compile the library & run the tests
`ninja && ctest`

### zephyr build

Prerequisites: a working zephyr SDK (at least v0.9.5), see [here](https://docs.zephyrproject.org/latest/getting_started/getting_started.html) for instructions. 
Please make sure you can compile and flash at least blinky example before building the wallet.

1) clone wallet repo to the zephyr's `samples` directory
`mkdir samples/anyledger/ && git clone git@github.com:AnyLedger/anyledger-wallet.git`

2) create build directory and generate build targets
`cd samples/anyledger/anyledger-wallet/build && cmake -GNinja  -DBOARD=nrf52840_pca10056 -DBUILD_XCOMPILE=1  ../`

3) compile & flash the firmware
`ninja && ninja flash`

## Using AnyLedger wallet

To start AnyLedger wallet using a QEMU virtual machine, [see here](https://github.com/AnyLedger/anyledger-wallet/blob/master/BUILD-qemu.md).
