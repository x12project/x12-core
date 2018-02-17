# X12

Copyright (c) 2014-2017, The X12 Project

## Development Resources

- GitHub: [https://github.com/kingrobert/x12-wallet](https://github.com/kingrobert/x12-wallet)


## Build

| Operating System      | Processor | Status |
| --------------------- | -------- |--------|
| Ubuntu 16.04          |  i686    | [![Ubuntu 16.04 i686]
| Ubuntu 16.04          |  amd64   | [![Ubuntu 16.04 amd64]
| Ubuntu 16.04          |  armv7   | [![Ubuntu 16.04 armv7]
| OSX 10.10             |  amd64   | [![OSX 10.10 amd64]
| OSX 10.11             |  amd64   | [![OSX 10.11 amd64]
| OSX 10.12             |  amd64   | [![OSX 10.12 amd64]
| Windows (MSYS2/MinGW) |  i686    | [![Windows (MSYS2/MinGW) i686]
| Windows (MSYS2/MinGW) |  amd64   | [![Windows (MSYS2/MinGW) amd64]


## Introduction

X12 is a private, secure, untraceable, decentralised digital currency. You are your bank, you control your funds, and nobody can trace your transfers unless you allow them to do so.

**Privacy:** X12 uses a cryptographically sound system to allow you to send and receive funds without your transactions being easily revealed on the blockchain (the ledger of transactions that everyone has). This ensures that your purchases, receipts, and all transfers remain absolutely private by default.

**Security:** Using the power of a distributed peer-to-peer consensus network, every transaction on the network is cryptographically secured. Individual wallets have a 25 word mnemonic seed that is only displayed once, and can be written down to backup the wallet. Wallet files are encrypted with a passphrase to ensure they are useless if stolen.

**Untraceability:** By taking advantage of ring signatures, a special property of a certain type of cryptography, X12 is able to ensure that transactions are not only untraceable, but have an optional measure of ambiguity that ensures that transactions cannot easily be tied back to an individual user or computer.

## About this Project

This is the core implementation of X12. 

## License

See [LICENSE](LICENSE).

## Compiling X12 from Source

### Dependencies

The following table summarizes the tools and libraries required to build.  A
few of the libraries are also included in this repository (marked as
"Vendored"). By default, the build uses the library installed on the system,
and ignores the vendored sources. However, if no library is found installed on
the system, then the vendored source will be built and used. The vendored
sources are also used for statically-linked builds because distribution
packages often include only shared library binaries (`.so`) but not static
library archives (`.a`).

| Dep            | Min. Version  | Vendored | Debian/Ubuntu Pkg  | Arch Pkg       | Optional | Purpose        |
| -------------- | ------------- | ---------| ------------------ | -------------- | -------- | -------------- |
| GCC            | 4.7.3         | NO       | `build-essential`  | `base-devel`   | NO       |                |
| CMake          | 3.0.0         | NO       | `cmake`            | `cmake`        | NO       |                |
| pkg-config     | any           | NO       | `pkg-config`       | `base-devel`   | NO       |                |
| Boost          | 1.58          | NO       | `libboost-all-dev` | `boost`        | NO       | C++ libraries  |
| OpenSSL        | basically any | NO       | `libssl-dev`       | `openssl`      | NO       | sha256 sum     |
| libunbound     | 1.4.16        | YES      | `libunbound-dev`   | `unbound`      | NO       | DNS resolver   |
| libminiupnpc   | 2.0           | YES      | `libminiupnpc-dev` | `miniupnpc`    | YES      | NAT punching   |
| libunwind      | any           | NO       | `libunwind8-dev`   | `libunwind`    | YES      | Stack traces   |
| liblzma        | any           | NO       | `liblzma-dev`      | `xz`           | YES      | For libunwind  |
| ldns           | 1.6.17        | NO       | `libldns-dev`      | `ldns`         | YES      | SSL toolkit    |
| expat          | 1.1           | NO       | `libexpat1-dev`    | `expat`        | YES      | XML parsing    |
| GTest          | 1.5           | YES      | `libgtest-dev`^    | `gtest`        | YES      | Test suite     |
| Doxygen        | any           | NO       | `doxygen`          | `doxygen`      | YES      | Documentation  |
| Graphviz       | any           | NO       | `graphviz`         | `graphviz`     | YES      | Documentation  |

[^] On Debian/Ubuntu `libgtest-dev` only includes sources and headers. You must
build the library binary manually. This can be done with the following command ```sudo apt-get install libgtest-dev && cd /usr/src/gtest && sudo cmake . && sudo make && sudo mv libg* /usr/lib/ ```

### Build instructions

X12 uses the CMake build system and a top-level [Makefile](Makefile) that
invokes cmake commands as needed.

#### On Linux and OS X

* Install the dependencies
* Change to the root of the source code directory and build:

        cd x12-core
        make

    *Optional*: If your machine has several cores and enough memory, enable
    parallel build by running `make -j<number of threads>` instead of `make`. For
    this to be worthwhile, the machine should have one core and about 2GB of RAM
    available per thread.

* The resulting executables can be found in `build/release/bin`

* Add `PATH="$PATH:$HOME/x12-core/build/release/bin"` to `.profile`

* Run X12 with `x12coind --detach`

* **Optional**: build and run the test suite to verify the binaries:

        make release-test

    *NOTE*: `coretests` test may take a few hours to complete.

* **Optional**: to build binaries suitable for debugging:

         make debug

* **Optional**: to build statically-linked binaries:

         make release-static

* **Optional**: build documentation in `doc/html` (omit `HAVE_DOT=YES` if `graphviz` is not installed):

        HAVE_DOT=YES doxygen Doxyfile


#### On Windows:

Binaries for Windows are built on Windows using the MinGW toolchain within
[MSYS2 environment](http://msys2.github.io). The MSYS2 environment emulates a
POSIX system. The toolchain runs within the environment and *cross-compiles*
binaries that can run outside of the environment as a regular Windows
application.

**Preparing the Build Environment**

* Download and install the [MSYS2 installer](http://msys2.github.io), either the 64-bit or the 32-bit package, depending on your system.
* Open the MSYS shell via the `MSYS2 Shell` shortcut
* Update packages using pacman:  

        pacman -Syuu  

* Exit the MSYS shell using Alt+F4  
* Edit the properties for the `MSYS2 Shell` shortcut changing "msys2_shell.bat" to "msys2_shell.cmd -mingw64" for 64-bit builds or "msys2_shell.cmd -mingw32" for 32-bit builds
* Restart MSYS shell via modified shortcut and update packages again using pacman:  

        pacman -Syuu  


* Install dependencies:

    To build for 64-bit Windows:

        pacman -S mingw-w64-x86_64-toolchain make mingw-w64-x86_64-cmake mingw-w64-x86_64-boost

    To build for 32-bit Windows:
 
        pacman -S mingw-w64-i686-toolchain make mingw-w64-i686-cmake mingw-w64-i686-boost

* Open the MingW shell via `MinGW-w64-Win64 Shell` shortcut on 64-bit Windows
  or `MinGW-w64-Win64 Shell` shortcut on 32-bit Windows. Note that if you are
  running 64-bit Windows, you will have both 64-bit and 32-bit MinGW shells.

**Building**

* If you are on a 64-bit system, run:

        make release-static-win64

* If you are on a 32-bit system, run:

        make release-static-win32

* The resulting executables can be found in `build/release/bin`


## Running x12coind

The build places the binary in `bin/` sub-directory within the build directory
from which cmake was invoked (repository root by default). To run in
foreground:

    ./bin/x12coind

To list all available options, run `./bin/x12coind --help`.  Options can be
specified either on the command line or in a configuration file passed by the
`--config-file` argument.  To specify an option in the configuration file, add
a line with the syntax `argumentname=value`, where `argumentname` is the name
of the argument without the leading dashes, for example `log-level=1`.

To run in background:

    ./bin/x12coind --log-file x12coind.log --detach

To run as a systemd service, copy
[x12coind.service](utils/systemd/x12coind.service) to `/etc/systemd/system/` and
[x12coind.conf](utils/conf/x12coind.conf) to `/etc/`. The [example
service](utils/systemd/x12coind.service) assumes that the user `x12` exists
and its home is the data directory specified in the [example
config](utils/conf/x12coind.conf).

If you're on Mac, you may need to add the `--max-concurrency 1` option to
x12coin-wallet-cli, and possibly x12coind, if you get crashes refreshing.

## Internationalization

See [README.i18n](README.i18n).

## Using Tor

While X12 isn't made to integrate with Tor, it can be used wrapped with torsocks, if you add --p2p-bind-ip 127.0.0.1 to the x12coind command line. You also want to set DNS requests to go over TCP, so they'll be routed through Tor, by setting DNS_PUBLIC=tcp. You may also disable IGD (UPnP port forwarding negotiation), which is pointless with Tor. To allow local connections from the wallet, you might have to add TORSOCKS_ALLOW_INBOUND=1, some OSes need it and some don't. Example:

`DNS_PUBLIC=tcp torsocks x12coind --p2p-bind-ip 127.0.0.1 --no-igd`

or:

`DNS_PUBLIC=tcp TORSOCKS_ALLOW_INBOUND=1 torsocks x12coind --p2p-bind-ip 127.0.0.1 --no-igd`

TAILS ships with a very restrictive set of firewall rules. Therefore, you need to add a rule to allow this connection too, in addition to telling torsocks to allow inbound connections. Full example:

`sudo iptables -I OUTPUT 2 -p tcp -d 127.0.0.1 -m tcp --dport 18781 -j ACCEPT`

`DNS_PUBLIC=tcp torsocks ./x12coind --p2p-bind-ip 127.0.0.1 --no-igd --rpc-bind-ip 127.0.0.1 --data-dir /home/amnesia/Persistent/your/directory/to/the/blockchain`

`./x12coin-wallet-cli`

## Using readline

While x12coind and x12coin-wallet-cli do not use readline directly, most of the functionality can be obtained by running them via rlwrap. This allows command recall, edit capabilities, etc. It does not give autocompletion without an extra completion file, however. To use rlwrap, simply prepend `rlwrap` to the command line, eg:

`rlwrap bin/x12coin-wallet-cli --wallet-file /path/to/wallet`

Note: rlwrap will save things like your seed and private keys, if you supply them on prompt. You may want to not use rlwrap when you use simplewallet to restore from seed, etc.

# Debugging

This section contains general instructions for debugging failed installs or problems encountered with X12. First ensure you are running the latest version built from the github repo.

## Obtaining Stack Traces and Core Dumps on Unix Systems

We generally use the tool `gdb` (GNU debugger) to provide stack trace functionality, and `ulimit` to provide core dumps in builds which crash or segfault.

* To use gdb in order to obtain a stack trace for a build that has stalled:

Run the build.

Once it stalls, enter the following command:

```
gdb /path/to/x12coind `pidof x12coind` 
```

Type `thread apply all bt` within gdb in order to obtain the stack trace

* If however the core dumps or segfaults:

Enter `ulimit -c unlimited` on the command line to enable unlimited filesizes for core dumps

Run the build.

When it terminates with an output along the lines of "Segmentation fault (core dumped)", there should be a core dump file in the same directory as x12coind.

You can now analyse this core dump with `gdb` as follows:

`gdb /path/to/x12coind /path/to/dumpfile`

Print the stack trace with `bt`

* To run x12 within gdb:

Type `gdb /path/to/x12coind`

Pass command-line options with `--args` followed by the relevant arguments

Type `run` to run x12coind

## Analysing Memory Corruption

We use the tool `valgrind` for this.

Run with `valgrind /path/to/x12coind`. It will be slow.

## LMDB

Instructions for debugging suspected blockchain corruption as per @HYC

There is an `mdb_stat` command in the LMDB source that can print statistics about the database but it's not routinely built. This can be built with the following command:

`cd ~/x12/external/db_drivers/liblmdb && make`

The output of `mdb_stat -ea <path to blockchain dir>` will indicate inconsistencies in the blocks, block_heights and block_info table.

The output of `mdb_dump -s blocks <path to blockchain dir>` and `mdb_dump -s block_info <path to blockchain dir>` is useful for indicating whether blocks and block_info contain the same keys.

These records are dumped as hex data, where the first line is the key and the second line is the data.
