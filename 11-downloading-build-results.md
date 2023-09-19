---
title: Downloading remote build results
order: 9
---

# Downloading remote build results

With tipi `v0.0.51` you can download build results directly onto your machine without downloading the full build directory.

Assuming we have a structure like this:
```sh
.
├── src
│  ├── string_to_json.cpp
│  └── stringstream_mayham.cpp
```

We can now build for `linux-cxx17` the sources remotely like this:
```sh
tipi -t linux-cxx17 build .
```

## Download a single file

Finally we can download the complete binary like this:
```sh
tipi -t linux-cxx17 download "./build/linux-cxx17/bin/src/string_to_json"
```

## Download multiple files with a wildcard

Finally we can download the complete binaries like this:
```sh
tipi -t linux-cxx17 download "./build/linux-cxx17/bin/src/string*"
```
Note that you have to take care of shell expansion. If your local shell is able to find files that match the `*` pattern it will pass just these paths to tipi.
Adding quotes solves this problem.

This will create a structure like this on your machine:
```sh
./build
├── 24ba3b7
│  └── bin
│     └── src
│        ├── string_to_json
│        └── stringstream_mayham
└── linux-cxx17 -> /usr/local/share/.tipi/vC.w/1da57b4-stringstream_mayham.b/24ba3b7
```

## Downloading the full build tree
To always synchronize the full build tree it's possible to build the application by appending **`--sync-build`** to the command line : 

`tipi -t linux-cxx17 build . --sync-build`

This can be a very big download following the build folder size. 