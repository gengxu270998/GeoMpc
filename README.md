# GeoMPC

This repository contains a proof-of-concept implementation for Geometric Perspective Protocols.

## Repo Directory Description

- `SCI/` A fork of CryptFlow2's SCI library and contains implementation of Geometric Perspective Protocols.
- `pretrained/` Pretrained neural networks and inputs.

## Compilation

Follow the README.md in SCI folder to compile the library. The test and network binaries will be created in `SCI/build/bin/`. 

```
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX=./install .. -DBUILD_TESTS=ON -DBUILD_NETWORKS=ON
cmake --build . --target install --parallel
```

We successfully completed the compilation on Ubuntu 20.04 with Intel Core i9-9900K.

## Running Tests & Networks

Run the unit tests of Geometric Perspective Protocols as follows:

```
./geometric_perspective_protocols-OT r=1 [port=port] & ./geometric_perspective_protocols-OT r=2 [port=port]
```

To run secure inference on networks:

```
./densenet121-OT-GEOM r=1 ell=32 k=12 [port=port] < ../../../pretrained/densenet121_model_scale12.inp // Server
./densenet121-OT-GEOM r=2 ell=32 k=12 [port=port] < ../../../pretrained/densenet121_input_scale12_pred739.inp // Client
```

- replace `densenet121-OT-GEOM` by `densenet121-OT` to execute the CryptFlow2's inference.
- replace `densnet121` by `resnet50` to run inference on ResNet50 model.
