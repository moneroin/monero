## Monero Infinity

Recently Monero released new software version 0.14. That means Monero will hard fork on Mar 9th 2019 at block height 1788000. I, together with some independent developers who share different opinions to Monero’s future, have decided to stay in the current version which is v0.13.0.4.

This repo is initialy a fork from [original monero v0.13.0.4](https://github.com/monero-project/monero) and will be maintained here from now on.

## Development resources

- Web: [moneroin.com](https://)
- Forum: 
- Mail: 
- GitHub: 
- IRC: 

## Introduction

Monero Infinity is a private, secure, untraceable, decentralised digital currency. You are your bank, you control your funds, and nobody can trace your transfers unless you allow them to do so.

**Privacy:** Monero Infinity uses a cryptographically sound system to allow you to send and receive funds without your transactions being easily revealed on the blockchain (the ledger of transactions that everyone has). This ensures that your purchases, receipts, and all transfers remain absolutely private by default.

**Security:** Using the power of a distributed peer-to-peer consensus network, every transaction on the network is cryptographically secured. Individual wallets have a 25 word mnemonic seed that is only displayed once, and can be written down to backup the wallet. Wallet files are encrypted with a passphrase to ensure they are useless if stolen.

**Untraceability:** By taking advantage of ring signatures, a special property of a certain type of cryptography, Monero Infinity is able to ensure that transactions are not only untraceable, but have an optional measure of ambiguity that ensures that transactions cannot easily be tied back to an individual user or computer.

## About this project

This is the core implementation of Monero Infinity. It is open source and completely free to use without restrictions, except for those specified in the license agreement below. There are no restrictions on anyone creating an alternative implementation of Monero Infinity that uses the protocol and network in a compatible manner.

As with many development projects, the repository on Github is considered to be the "staging" area for the latest changes. Before changes are merged into that branch on the main repository, they are tested by individual developers in their own branches, submitted as a pull request, and then subsequently tested by contributors who focus on testing and code reviews. That having been said, the repository should be carefully considered before using it in a production environment, unless there is a patch in the repository for a particular show-stopping issue you are experiencing. It is generally a better idea to use a tagged release for stability.

## License

See [LICENSE](LICENSE).

## Scheduled software upgrades

To be announced.

## Compiling Monero Infinity from source

### Dependencies

The following table summarizes the tools and libraries required to build. A
few of the libraries are also included in this repository (marked as
"Vendored"). By default, the build uses the library installed on the system,
and ignores the vendored sources. However, if no library is found installed on
the system, then the vendored source will be built and used. The vendored
sources are also used for statically-linked builds because distribution
packages often include only shared library binaries (`.so`) but not static
library archives (`.a`).

| Dep          | Min. version  | Vendored | Debian/Ubuntu pkg  | Optional | Purpose        |
| ------------ | ------------- | -------- | ------------------ | -------- | -------------- |
| GCC          | 4.7.3         | NO       | `build-essential`  | NO       |                |
| CMake        | 3.5           | NO       | `cmake`            | NO       |                |
| pkg-config   | any           | NO       | `pkg-config`       | NO       |                |
| Boost        | 1.58          | NO       | `libboost-all-dev` | NO       | C++ libraries  |
| OpenSSL      | basically any | NO       | `libssl-dev`       | NO       | sha256 sum     |
| libzmq       | 3.0.0         | NO       | `libzmq3-dev`      | NO       | ZeroMQ library |
| OpenPGM      | ?             | NO       | `libpgm-dev`       | NO       | For ZeroMQ     |
| libunbound   | 1.4.16        | YES      | `libunbound-dev`   | NO       | DNS resolver   |
| libsodium    | ?             | NO       | `libsodium-dev`    | NO       | cryptography   |
| libunwind    | any           | NO       | `libunwind8-dev`   | YES      | Stack traces   |
| liblzma      | any           | NO       | `liblzma-dev`      | YES      | For libunwind  |
| libreadline  | 6.3.0         | NO       | `libreadline6-dev` | YES      | Input editing  |
| ldns         | 1.6.17        | NO       | `libldns-dev`      | YES      | SSL toolkit    |
| expat        | 1.1           | NO       | `libexpat1-dev`    | YES      | XML parsing    |
| GTest        | 1.5           | YES      | `libgtest-dev`^    | YES      | Test suite     |
| Doxygen      | any           | NO       | `doxygen`          | YES      | Documentation  |
| Graphviz     | any           | NO       | `graphviz`         | YES      | Documentation  |


[^] On Debian/Ubuntu `libgtest-dev` only includes sources and headers. You must
build the library binary manually. This can be done with the following command ```sudo apt-get install libgtest-dev && cd /usr/src/gtest && sudo cmake . && sudo make && sudo mv libg* /usr/lib/ ```

Debian / Ubuntu one liner for all dependencies  
``` sudo apt update && sudo apt install build-essential cmake pkg-config libboost-all-dev libssl-dev libzmq3-dev libunbound-dev libsodium-dev libunwind8-dev liblzma-dev libreadline6-dev libldns-dev libexpat1-dev doxygen graphviz libpgm-dev```

### Cloning the repository

Clone recursively to pull-in needed submodule(s):

`$ git clone --recursive https://github.com/moneroin/monero`

If you already have a repo cloned, initialize and update:

`$ cd monero && git submodule init && git submodule update`

### Build instructions

Monero Infinity uses the CMake build system and a top-level [Makefile](Makefile) that
invokes cmake commands as needed.

#### On Linux and OS X

* Install the dependencies
* Change to the root of the source code directory, change to the most recent release branch, and build:

        cd monero
        git checkout release-v1.0
        make

    *Optional*: If your machine has several cores and enough memory, enable
    parallel build by running `make -j<number of threads>` instead of `make`. For
    this to be worthwhile, the machine should have one core and about 2GB of RAM
    available per thread.

    *Note*: If cmake can not find zmq.hpp file on OS X, installing `zmq.hpp` from
    https://github.com/zeromq/cppzmq to `/usr/local/include` should fix that error.
    
    *Note*: The instructions above will compile the most stable release of the
    Monero Infinity software. If you would like to use and test the most recent software,
    use ```git checkout master```. The master branch may contain updates that are
    both unstable and incompatible with release software, though testing is always 
    encouraged. 

* The resulting executables can be found in `build/release/bin`

* Add `PATH="$PATH:$HOME/monero/build/release/bin"` to `.profile`

* Run Monero Infinity with `moneroinfinityd --detach`

* **Optional**: build and run the test suite to verify the binaries:

        make release-test

    *NOTE*: `core_tests` test may take a few hours to complete.

* **Optional**: to build binaries suitable for debugging:

         make debug

* **Optional**: to build statically-linked binaries:

         make release-static

Dependencies need to be built with -fPIC. Static libraries usually aren't, so you may have to build them yourself with -fPIC. Refer to their documentation for how to build them.

* **Optional**: build documentation in `doc/html` (omit `HAVE_DOT=YES` if `graphviz` is not installed):

        HAVE_DOT=YES doxygen Doxyfile

#### On Windows:

Binaries for Windows are built on Windows using the MinGW toolchain within
[MSYS2 environment](https://www.msys2.org). The MSYS2 environment emulates a
POSIX system. The toolchain runs within the environment and *cross-compiles*
binaries that can run outside of the environment as a regular Windows
application.

**Preparing the build environment**

* Download and install the [MSYS2 installer](https://www.msys2.org), either the 64-bit or the 32-bit package, depending on your system.
* Open the MSYS shell via the `MSYS2 Shell` shortcut
* Update packages using pacman:  

        pacman -Syuu  

* Exit the MSYS shell using Alt+F4  
* Edit the properties for the `MSYS2 Shell` shortcut changing "msys2_shell.bat" to "msys2_shell.cmd -mingw64" for 64-bit builds or "msys2_shell.cmd -mingw32" for 32-bit builds
* Restart MSYS shell via modified shortcut and update packages again using pacman:  

        pacman -Syuu  


* Install dependencies:

    To build for 64-bit Windows:

        pacman -S mingw-w64-x86_64-toolchain make mingw-w64-x86_64-cmake mingw-w64-x86_64-boost mingw-w64-x86_64-openssl mingw-w64-x86_64-zeromq mingw-w64-x86_64-libsodium mingw-w64-x86_64-hidapi

    To build for 32-bit Windows:
 
        pacman -S mingw-w64-i686-toolchain make mingw-w64-i686-cmake mingw-w64-i686-boost mingw-w64-i686-openssl mingw-w64-i686-zeromq mingw-w64-i686-libsodium mingw-w64-i686-hidapi

* Open the MingW shell via `MinGW-w64-Win64 Shell` shortcut on 64-bit Windows
  or `MinGW-w64-Win64 Shell` shortcut on 32-bit Windows. Note that if you are
  running 64-bit Windows, you will have both 64-bit and 32-bit MinGW shells.

**Cloning**

* To git clone, run:

        git clone --recursive https://github.com/moneroin/monero.git

**Building**

* Change to the cloned directory, run:
	
        cd monero

* If you would like a specific [version/tag](https://github.com/moneroin/monero/tags), do a git checkout for that version. eg. 'v1.0.0.0'. If you dont care about the version and just want binaries from master, skip this step:
	
        git checkout v1.0.0.0

* If you are on a 64-bit system, run:

        make release-static-win64

* If you are on a 32-bit system, run:

        make release-static-win32

* The resulting executables can be found in `build/release/bin`

* **Optional**: to build Windows binaries suitable for debugging on a 64-bit system, run:

        make debug-static-win64
	
* **Optional**: to build Windows binaries suitable for debugging on a 32-bit system, run:

        make debug-static-win32

* The resulting executables can be found in `build/debug/bin`

### Building portable statically linked binaries (Cross Compiling)

By default, in either dynamically or statically linked builds, binaries target the specific host processor on which the build happens and are not portable to other processors. Portable binaries can be built using the following targets:

* ```make release-static-linux-x86_64``` builds binaries on Linux on x86_64 portable across POSIX systems on x86_64 processors
* ```make release-static-linux-i686``` builds binaries on Linux on x86_64 or i686 portable across POSIX systems on i686 processors
* ```make release-static-win64``` builds binaries on 64-bit Windows portable across 64-bit Windows systems
* ```make release-static-win32``` builds binaries on 64-bit or 32-bit Windows portable across 32-bit Windows systems

## Running moneroinfinityd

The build places the binary in `bin/` sub-directory within the build directory
from which cmake was invoked (repository root by default). To run in
foreground:

    ./bin/moneroinfinityd

To list all available options, run `./bin/moneroinfinityd --help`.  Options can be
specified either on the command line or in a configuration file passed by the
`--config-file` argument.  To specify an option in the configuration file, add
a line with the syntax `argumentname=value`, where `argumentname` is the name
of the argument without the leading dashes, for example `log-level=1`.

To run in background:

    ./bin/moneroinfinityd --log-file moneroinfinityd.log --detach

To make the original [monero-v0.13.0.4](https://github.com/monero-project/monero/releases/tag/v0.13.0.4) still work on monero infinity net, we have set up several monero infinity nodes to still accept CNv2 proofs instead of CNv4 proofs.

To quickly sync from these nodes, please use `--add-peer` with monerod-v0.13.0.4.

| Server          | Port  | Region        |
|-----------------|-------|---------------|
| 149.129.54.152  | 18080 | Signpore      |
| 39.105.13.148   | 18080 | China         |
| 212.64.58.71    | 18080 | China         |
| 39.106.52.162   | 18080 | China         |
| 162.250.188.193 | 18080 | Canada        |
| 172.245.246.20  | 18080 | United States |

## Internationalization

See [README.i18n.md](README.i18n.md).

## Debugging

This section contains general instructions for debugging failed installs or problems encountered with Monero Infinity. First ensure you are running the latest version built from the Github repo.

### Obtaining stack traces and core dumps on Unix systems

We generally use the tool `gdb` (GNU debugger) to provide stack trace functionality, and `ulimit` to provide core dumps in builds which crash or segfault.

* To use gdb in order to obtain a stack trace for a build that has stalled:

Run the build.

Once it stalls, enter the following command:

```
gdb /path/to/moneroinfinityd `pidof moneroinfinityd` 
```

Type `thread apply all bt` within gdb in order to obtain the stack trace

* If however the core dumps or segfaults:

Enter `ulimit -c unlimited` on the command line to enable unlimited filesizes for core dumps

Enter `echo core | sudo tee /proc/sys/kernel/core_pattern` to stop cores from being hijacked by other tools

Run the build.

When it terminates with an output along the lines of "Segmentation fault (core dumped)", there should be a core dump file in the same directory as moneroinfinityd. It may be named just `core`, or `core.xxxx` with numbers appended.

You can now analyse this core dump with `gdb` as follows:

`gdb /path/to/moneroinfinityd /path/to/dumpfile`

Print the stack trace with `bt`

* To run moneroinfinity within gdb:

Type `gdb /path/to/moneroinfinityd`

Pass command-line options with `--args` followed by the relevant arguments

Type `run` to run moneroinfinityd

### Analysing memory corruption

There are two tools available:

* ASAN

Configure Monero Infinity with the -D SANITIZE=ON cmake flag, eg:

    cd build/debug && cmake -D SANITIZE=ON -D CMAKE_BUILD_TYPE=Debug ../..

You can then run the monero infinity tools normally. Performance will typically halve.

* valgrind

Install valgrind and run as `valgrind /path/to/moneroinfinityd`. It will be very slow.

### LMDB

Instructions for debugging suspected blockchain corruption as per @HYC

There is an `mdb_stat` command in the LMDB source that can print statistics about the database but it's not routinely built. This can be built with the following command:

`cd ~/monero/external/db_drivers/liblmdb && make`

The output of `mdb_stat -ea <path to blockchain dir>` will indicate inconsistencies in the blocks, block_heights and block_info table.

The output of `mdb_dump -s blocks <path to blockchain dir>` and `mdb_dump -s block_info <path to blockchain dir>` is useful for indicating whether blocks and block_info contain the same keys.

These records are dumped as hex data, where the first line is the key and the second line is the data.
