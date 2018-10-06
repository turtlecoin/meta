# SECTIONS AND DATA DEFINED HEREIN ARE PROVIDED AS AN EXAMPLE AND NEED TO BE UPDATED FOR EACH RELEASE AS NECESSARY AND/OR RELEVANT

![image](https://user-images.githubusercontent.com/34389545/35821974-62e0e25c-0a70-11e8-87dd-2cfffeb6ed47.png)

## Special Notes

### Network Upgrade

Upgrade to this release is **mandatory** by block 800,000. Please update your software before block 800,000 to ensure you remain sycned with the network.

### Static Mixin Change

The network will be changing from a static mixin of `7` to a static mixin of `3` at block 800,000. Please ensure all wallets, services, pools and other tools that send transactions are updated to support this by block 800,000.

### Backwards Compatibility

v0.8.x releases revert a change that made the v0.7.0 release incompatible with older versions of the wallet (zedwallet, turtle-service, etc). This has been resolved in the 0.8.x builds.

## Release Notes

### General Updates

* Enhanced Travis-CI & AppVeyor builds
  * Travis now auto builds AARCH64 builds on release
* ChaCha8 updates to make things easier to copy
* Added additional `make` targets to allow for building only certain sets of binaries
  * `make pool` builds `TurtleCoind` & `turtle-service`
  * `make solominer` builds `TurtleCoind`, `zedwallet`, & `miner`
  * `make cli` builds `TurtleCoind` & `zedwallet`
  * `make` builds all normal targets (`TurtleCoind`, `zedwallet`, `turtle-service`, & `miner`)
* Additional LWMA2 tweaks
  * Moved Difficulty algorithms to separate file for better manageability
* Updated linenoise to c++ version that gives better cross-platform support
* Cleaned up left over code that is no longer used
* Network Static Mixin count has been reduced from `7` to `3` as of block 800,000

### TurtleCoind Updates

* Removed linenoise support from the daemon that was linked to possible early exists of the daemon
* Daemon now supports the ability to reject communication with peers that are out of date. This release does **not** cut off any old peers yet; however, the next release will cut off support for older peers.
* Enhanced `status` command to provide percent of synchronization complete
* Enhanced `status` command to supply more accurate information in relation to the peer you are connected to
* Optimized checkpoint loading

### zedwallet Updates

* Massive update that allows for scanning from a specific height when importing from seed or keys.
  * Speeds up syncing a wallet; however, you must know which height to start scanning for transactions.
* Fixed overflow in `save_csv` command that would result in unexpected values on outgoing transactions
* Removed libboost references

### turtle-service Updates

* Massive update that allows for scanning from a specific height when importing from seed or keys.
  * Speeds up syncing a wallet; however, you must know which height to start scanning for transactions.
* Added *getFeeInfo* method to the RPC that is an alias for *feeinfo*
* Proper error reporting when a wallet file is not supplied

### miner Updates

* Nothing new to report

## How To Sync Quickly

- Download the latest checkpoints.csv here https://github.com/turtlecoin/checkpoints/raw/master/checkpoints.csv
- Place checkpoints.csv in the same folder as your TurtleCoind daemon
- Run TurtleCoind with checkpoints added like this: 
Linux, Apple `./TurtleCoind --load-checkpoints checkpoints.csv`
Windows `TurtleCoind.exe --load-checkpoints checkpoints.csv`

## How To Upgrade from an Older Version

### (Windows, Mac, Linux)

- run the old `TurtleCoind` to sync your chain fully
- with the old `TurtleCoind` fully synced, open `zedwallet`
- open your wallet
- export your SPEND and VIEW keys, this is important. `export_keys` If you are in windows, right click the titlebar of the CMD window and select EDIT then MARK so be able to copy and paste your keys.
- Unzip the new files into a NEW FOLDER outside of the old version folder
- Close the old version folder `TurtleCoind` and `zedwallet` by typing `exit` into each window
- Run the new `TurtleCoind`
- Run the new `zedwallet`
- Press **I** for IMPORT in `zedwallet`
- Use any filename for the wallet name, it does not have to be the same
- Use any password for wallet password, it does not have to be the same as the last
- Enter your `spend_key` and your `view_key` from step 4

## RPC API Changes

**Developers seeking to integrate `turtle-service` into their applications should note this crucial change from other CryptoNote coins.**

Mandatory authentication has been added to the JSON RPC API interfaces of `turtle-service`. To maintain back compatibility with old services, the `--rpc-legacy-security` flag has been introduced to allow password-less access to the RPC.

However, all new applications going forward should make use of the `--rpc-password` flag to enable authentication of JSON RPC queries.

To pass the password to the RPC interface, add an additional field `password` to the JSON object:

```
{
    "jsonrpc": "2.0",
    "method": "transfer",
    "params": {},
    "password": "<the password>"
}
```

## How To Compile

#### [new!] Raspberry Pi 3 B+
The following images are known to work. Please make sure you are running a 64-bit Pi release.

##### OS Distribution

- https://github.com/Crazyhead90/pi64/releases
- https://fedoraproject.org/wiki/Architectures/ARM/Raspberry_Pi#aarch64_supported_images_for_Raspberry_Pi_3
- https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-3

### Ubuntu 18.xx LTS

```bash
sudo apt-get update
sudo apt-get install -y build-essential python-dev gcc g++ cmake git libboost-all-dev
git clone -b master https://github.com/turtlecoin/turtlecoin
cd turtlecoin
mkdir build && cd $_
cmake .. && make
```

### Windows 10

#### Prerequisites

- Install [Visual Studio 2017 Community Edition](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15&page=inlineinstall)
- When installing Visual Studio, it is absolutely important you install C++ capabilities, and the vc++ v140 toolchain when selecting features. You will need this for cmake, MSBuild and other commands.
- Install [Boost 1.59.0](https://sourceforge.net/projects/boost/files/boost-binaries/1.59.0/), ensure to download the installer for  MSVC 14.

#### Building

- Use the start menu or similar to open 'x64 Native Tools Command Prompt for vs2017' command prompt.
- `cd <your_turtlecoin_directory>`
- `mkdir build`
- `cd build`
- `cmake -G "Visual Studio 14 Win64" .. -DBOOST_ROOT=D:/Boost/boost_1_59_0` (Or your boost installed dir.)
- `MSBuild TurtleCoin.sln /p:Configuration=Release`
- At this point, this will create a .sln file in the 'build' directory. Open this .sln in Visual Studio 2017 and click 'Build Solution' under the 'Build' Menu Item.
- If all went well, it will complete successfully, and you will find all your binaries in the `..\build\src\Debug` directory, or the `..\build\src\Release` directory if you built with release enabled.


### Apple

#### Prerequisites

- Install [cmake](https://cmake.org/). See [here](https://stackoverflow.com/questions/23849962/cmake-installer-for-mac-fails-to-create-usr-bin-symlinks) if you are unable call `cmake` from the terminal after installing.
- Install the [boost](http://www.boost.org/) libraries. Either compile boost manually or run `brew install boost`.
- Install XCode and Developer Tools.

#### Building

- `git clone -b master https://github.com/turtlecoin/turtlecoin`
- `cd turtlecoin`
- `mkdir build && cd $_`
- `cmake ..` or `cmake -DBOOST_ROOT=<path_to_boost_install> ..` when building
  from a specific boost install
- `make`

The binaries will be in `./src` after compilation is complete.


## Changelog

See the [TurtleCoin Release](https://github.com/turtlecoin/turtlecoin/releases) page for the full change history.

## Thanks
Cryptonote Developers, Bytecoin Developers, Forknote Project, TurtleCoin Community
