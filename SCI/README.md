# Secure and Correct Inference (SCI) Library

## Introduction
This directory contains the code for the Secure and Correct Inference (SCI) library from ["CrypTFlow2: Practical 2-Party Secure Inference"](https://eprint.iacr.org/2020/1002) and ["SIRNN: A Math Library for Secure RNN Inference"](https://eprint.iacr.org/2021/459).

## Required Packages
 - g++ (version >= 8)
 - cmake
 - make
 - libgmp-dev
 - libssl-dev  
 - SEAL 3.3.2
 - Eigen 3.3

SEAL can be installed as follows:

```
mkdir -p extern && cd extern
git clone --branch 3.3.2 https://github.com/microsoft/SEAL.git
cd SEAL/native/src
cmake .
make
sudo make install
```
We have noticed that compiling this version of the SEAL library can result in issues under certain environments. 
```
/root/gengxu270998-GeoMpc-ba00052/SCI/extern/SEAL/native/src/seal/util/locks.h:17:33: error: ‘unique_lock’ in namespace ‘std’ does not name a template type
   17 |         using WriterLock = std::unique_lock<std::shared_mutex>;
      |                                 ^~~~~~~~~~~
/root/gengxu270998-GeoMpc-ba00052/SCI/extern/SEAL/native/src/seal/util/locks.h:10:1: note: ‘std::unique_lock’ is defined in header ‘<mutex>’; did you forget to ‘#include <mutex>’?
    9 | #include <shared_mutex>
  +++ |+#include <mutex>
   10 |
/root/gengxu270998-GeoMpc-ba00052/SCI/extern/SEAL/native/src/seal/util/locks.h:29:35: error: ‘WriterLock’ does not name a type
   29 |             SEAL_NODISCARD inline WriterLock acquire_write()
      |                                   ^~~~~~~~~~
/root/gengxu270998-GeoMpc-ba00052/SCI/extern/SEAL/native/src/seal/util/locks.h:39:35: error: ‘WriterLock’ does not name a type
   39 |             SEAL_NODISCARD inline WriterLock try_acquire_write() noexcept
      |                                   ^~~~~~~~~~
```
We are not entirely sure what is causing this problem, but it seems can be resolved by adding 
```
#include <mutex>
```
to the locks.h file.

Eigen can be installed as follows:

```
mkdir -p extern && cd extern
git clone https://github.com/libigl/eigen.git
cd eigen
mkdir build && cd build
cmake ..
sudo make install
```

The other packages can be installed directly using `sudo apt-get install <package>` on Linux.

## Compilation

To compile the library:

```
mkdir build && cd build
sudo cmake -DCMAKE_INSTALL_PREFIX=./install .. -DBUILD_TESTS=ON -DBUILD_NETWORKS=ON
sudo cmake --build . --target install --parallel
```

## Running Tests & Networks

On successful compilation, the test and network binaries will be created in `build/bin/`.

Run the tests as follows to make sure everything works as intended:

`./<test> r=1 [port=port] & ./<test> r=2 [port=port]`

To run secure inference on networks:

```
./<network> r=1 [port=port] < <model_file> // Server
./<network> r=2 [ip=server_address] [port=port] < <image_file> // Client
```

# Acknowledgements

This library includes code from the following external repositories:

 - [emp-toolkit/emp-tool](https://github.com/emp-toolkit/emp-tool/tree/c44566f40690d2f499aba4660f80223dc238eb03/emp-tool) for cryptographic tools and network I/O.
 - [emp-toolkit/emp-ot](https://github.com/emp-toolkit/emp-ot/tree/0f4a1e41a25cf1a034b5796752fde903a241f482/emp-ot) for Naor-Pinkas (base) OT and IKNP OT extension implementation.
 - [mc2-project/delphi](https://github.com/mc2-project/delphi/tree/de77cd7b896a2314fec205a8f67b257df46dd75c/rust/protocols-sys/c++/src/lib) for implementation of [Gazelle's](https://eprint.iacr.org/2018/073.pdf) algorithms for convolution and fully connected layers, which was majorly modified for better efficiency. 
 - [homenc/HElib](https://github.com/homenc/HElib/blob/6397b23e64c32fd6eab76bd7a08b95d8399503f4/src/NumbTh.h) for command-line argument parsing.
