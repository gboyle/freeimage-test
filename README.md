# FreeImage Load Issue

Issue reproduction for using FreeImage via vcpkg with dynamic linking on Linux.

## Using FreeImage via vcpkg with dynamic linking on Linux fails to load libraw.so.23.

### Describe the bug
Trying to build and use *FreeImage* as a shared object in a C++ project under Linux. The build completes, but the resulting executable is unable to run as it fails to load *libraw.so.23*, which is present.

### Environment
- OS: Debian GNU/Linux 12 (bookworm)
- Compiler: c++ (Debian 12.2.0-14+deb12u1) 12.2.0
- using VCPKG_TARGET_TRIPLET="x64-linux-dynamic"

### To Reproduce
```bash
# install vcpkg under home (or update CMakePresets.json to reflect selected location)
cd
git clone --branch 2025.07.25 https://github.com/microsoft/vcpkg.git
cd vcpkg
./bootstrap-vcpkg.sh -disableMetrics

# clone
cd
git clone https://github.com/gboyle/freeimage-test.git

# configure
cd freeimage-test
cmake --preset repro

# build
cd build/
cmake --build .

# run
./freeimage-test
```

See [reproduction](https://github.com/gboyle/freeimage-test) for more.

### Expected behavior
The produced executable should be able to load *FreeImage* and its transitive dependencies.

### Failure logs

```bash
repro@vm:~/code/freeimage-test/build$ ./freeimage-test

./freeimage-test: error while loading shared libraries: libraw.so.23: cannot open shared object file: No such file or directory
```

See *details.txt* in [reproduction](https://github.com/gboyle/freeimage-test) for more (ldd, objdump, LD_DEBUG=libs, etc.).

### Additional context

- vcpkg does report *Cannot generate a safe runtime search path for target freeimage-test* but doesn't mention *libraw*
- using *LD_DEBUG=libs* indicates that *manual-link* is not searched (is it using the RUNPATH from libFreeImaged.so not the executable?)
- adding *manual-link* to *LD_LIBRARY_PATH* does work
- it's not entirely clear why *libraw* was moved to *manual-link*, it doesn't seem to export main
