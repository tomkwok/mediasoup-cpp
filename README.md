# Mediasoup server with CMake

[tomkwok/mediasoup-cpp](https://github.com/tomkwok/mediasoup-cpp) is a fork of [barry-ran/mediasoup-cpp](https://github.com/barry-ran/mediasoup-cpp), which is a fork of [versatica/mediasoup](https://github.com/versatica/mediasoup) to switch build system from `node-gyp` to `cmake`. This would be ideal for compiling on a single-board computer such as Raspberry Pi, but I found that some work is needed to complete the set-up.

This fork of mine brings the following changes:

- Fixed building on Linux and macOS: `PATH_MAX` constant, case-sensitive header file name, `-std=c++14` in `CMAKE_CXX_FLAGS` for use of `std::initializer_list`
- Added detailed documentation for building and usage in `README.md`
- Updated dependency OpenSSL version
- Backport commits from upstream [versatica/mediasoup](https://github.com/versatica/mediasoup)
- (Todo) Re-write remaining Node.js components in C++ including Transport
- (Todo) Add parsing of command line arguments to demo executable and pass them to workers spawned
- (Todo) Move dependencies in `deps/` to git submodules for more convenient updates from upstream

## Building

Configure the number of processes for `make` as suitable to speed up compilation.

```
mkdir -p build/
cd build/
cmake ..
make -j3
```

## Usage

After building, a binary named `mediasoup-worker` can be found in directory `build/`. The worker binary may be used with a demo server implemented with [mediasoup-go](https://github.com/jiyeyuran/mediasoup-go) with the environment variable `MEDIASOUP_WORKER_BIN` set to the path of compiled `mediasoup-worker`. Hence, we avoided having to use a Node.js build system to compile the worker written in C++, and also avoided having to use the Node.js implementation of the endpoint server.

A binary named `mediasoup-demo` also found in directory `build/` is useful for debugging only as it cannot be used to launch a server instance for the demo application. The endpoint server re-write in C++ found in [barry-ran/mediasoup-cpp](https://github.com/barry-ran/mediasoup-cpp) is incomplete.

## Web app

The demo web app can be found in [mediasoup-demo](https://github.com/versatica/mediasoup-demo) in directory `app/`. It can be built using Node.js with the following commands in directory `app/`.

```
npm install
gulp dist
```

The built web app found in `server/public/` can then be taken out and hosted on other machines such as a Raspberry Pi with any web server such as `nginx`. Note that leading `/` can be removed from paths in `index.html` if the web app is not hosted at root `/`.
