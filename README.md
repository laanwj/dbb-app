[![License](http://img.shields.io/:License-MIT-yellow.svg)](LICENSE)
[![Build Status](https://travis-ci.org/digitalbitbox/dbb-app.svg?branch=master)](https://travis-ci.org/digitalbitbox/dbb-app)

## DBB-APP
A QT based application for the [Digital Bitbox](https://digitalbitbox.com) hardware wallet. The application support managing your dbb device (create new wallet, backup, set 2FA key, etc.). It also supports co-signing together with a [Bitpay Copay Wallet](http://copay.io).


## dbb-app
The dbb-app Qt app will be built if qt5 is available on your system or if `--with-gui=qt5` has been set. 

## dbb-cli
This package includes a cli tool "dbb-cli" which can be used to direcly talk with your digital bitbox device.


**Examples:**

* `dbb-cli erase`
* `dbb-cli -newpassword=0000 password`
* `dbb-cli -newpassword=test -password=0000 password`
* `dbb-cli -password=test led`
* `dbb-cli -password=test seed`
* `dbb-cli -keypath=m/44/0 xpub`

Available commands with possible arguments (* = mandatory):

```
  erase 
  password -*newpassword
  led 
  seed -source (default: create), -decrypt (default: no), -salt
  backuplist 
  backuperase 
  backup -encrypt (default: no), -filename (default: backup.dat)
  sign -type (default: meta), -meta, -*^hashes-keypaths, -*^keypath

    Example
    =======
    dbb-cli --password=0000 sign -hashes-keypaths='[{"hash": "f6f4a3633eda92eef9dd96858dec2f5ea4dfebb67adac879c964194eb3b97d79", "keypath":"m/44/0"}]' -keypath='[{"pubkey":"0270526bf580ddb20ad18aad62b306d4beb3b09fae9a70b2b9a93349b653ef7fe9", "keypath":"m/44"}]' -meta=34982a264657cdf2635051bd778c99e73ce5eb2e8c7f9d32b8aaa7e547c7fd90

    (this will sign the given hash(s) with the privatekey specified with keypath and checks, if pubkey at given keypath is equal/valid

  xpub -*keypath
  random -mode (default: true)
  info 
  lock 
  verifypass -operation (default: create)
  aes -type (default: encrypt), -*data
  bootloaderunlock 
  bootloaderlock 
  firmware -filename
  decryptbackup -filename
```

## Build Instructions
### Dependencies:

dbb-cli and dbb-app depend on libbtc (https://github.com/libbtc/libbtc) for key generation, signing, hashing, crypto, etc.
Libbtc is included as git subtree and will be compiled during the normal build process.

- libbtc (included as git subtree) (https://github.com/libbtc/libbtc)
- hidapi (https://github.com/signal11/hidapi)
- libcurl
- libavahi (for mDNS; linux only, libavahi-compat-libdnssd-dev)
- qt5 (if UI enabled)

OSX:

    brew install hidapi libevent qt5

#### Linux (Ubuntu 15.04):

    sudo apt-get install build-essential libtool autotools-dev autoconf pkg-config git
    sudo apt-get install libcurl4-openssl-dev libudev-dev libqrencode-dev libhidapi-dev
    sudo apt-get install libqt5gui5 libqt5core5a libqt5dbus5 qttools5-dev qttools5-dev-tools libqt5websockets5-dev libavahi-compat-libdnssd-dev


#### Linux (Fedora 21/22):

    sudo yum groupinstall "C Development Tests and Libraries"
    sudo yum install git hidapi-devel
    sudo yum install mesa-libGL-devel qt5-qttools-devel



if `libhidapi` is not available, compile it yourself

    sudo apt-get install libudev-dev libusb-1.0-0-dev lib
    git clone git://github.com/signal11/hidapi.git
    ./bootstrap
    ./configure
    make
    sudo make install



### Basic build steps:

    ./autogen.sh
    ./configure --enable-debug --with-gui=qt5
    make
    sudo make install
