# XMRig

:warning: **[Monero will change PoW algorithm on October 18](https://github.com/xmrig/xmrig/issues/753), all miners and proxy should be updated to [v2.8+](https://github.com/xmrig/xmrig/releases/tag/v2.8.1)** :warning:

[![Github All Releases](https://img.shields.io/github/downloads/xmrig/xmrig/total.svg)](https://github.com/xmrig/xmrig/releases)
[![GitHub release](https://img.shields.io/github/release/xmrig/xmrig/all.svg)](https://github.com/xmrig/xmrig/releases)
[![GitHub Release Date](https://img.shields.io/github/release-date-pre/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/releases)
[![GitHub license](https://img.shields.io/github/license/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/blob/master/LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/xmrig/xmrig.svg)](https://github.com/xmrig/xmrig/network)

XMRig is a high performance Monero (XMR) CPU miner, with official support for Windows.
Originally based on cpuminer-multi with heavy optimizations/rewrites and removing a lot of legacy code, since version 1.0.0 completely rewritten from scratch on C++.

* This is the **CPU-mining** version, there is also a [NVIDIA GPU version](https://github.com/xmrig/xmrig-nvidia) and [AMD GPU version]( https://github.com/xmrig/xmrig-amd).
* [Roadmap](https://github.com/xmrig/xmrig/issues/106) for next releases.

<img src="http://i.imgur.com/OKZRVDh.png" width="619" >

#### Table of contents
* [Features](#features)
* [Download](#download)
* [Usage](#usage)
* [Algorithm variations](#algorithm-variations)
* [Build](https://github.com/xmrig/xmrig/wiki/Build)
* [Common Issues](#common-issues)
* [Other information](#other-information)
* [Donations](#donations)
* [Release checksums](#release-checksums)
* [Contacts](#contacts)

## Features
* High performance.
* Official Windows support.
* Small Windows executable, without dependencies.
* x86/x64 support.
* Support for backup (failover) mining server.
* keepalived support.
* Command line options compatible with cpuminer.
* CryptoNight-Lite support for AEON.
* Smart automatic [CPU configuration](https://github.com/xmrig/xmrig/wiki/Threads).
* Nicehash support
* It's open source software.

## Download
* Binary releases: https://github.com/xmrig/xmrig/releases
* Git tree: https://github.com/xmrig/xmrig.git
  * Clone with `git clone https://github.com/xmrig/xmrig.git` :hammer: [Build instructions](https://github.com/xmrig/xmrig/wiki/Build).

## Usage
Use [config.xmrig.com](https://config.xmrig.com/xmrig) to generate, edit or share configurations.

### Options
```
  -a, --algo=ALGO          specify the algorithm to use
                             cryptonight
                             cryptonight-lite
                             cryptonight-heavy
  -o, --url=URL            URL of mining server
  -O, --userpass=U:P       username:password pair for mining server
  -u, --user=USERNAME      username for mining server
  -p, --pass=PASSWORD      password for mining server
      --rig-id=ID          rig identifier for pool-side statistics (needs pool support)
  -t, --threads=N          number of miner threads
  -v, --av=N               algorithm variation, 0 auto select
  -k, --keepalive          send keepalived packet for prevent timeout (needs pool support)
      --nicehash           enable nicehash.com support
      --tls                enable SSL/TLS support (needs pool support)
      --tls-fingerprint=F  pool TLS certificate fingerprint, if set enable strict certificate pinning
  -r, --retries=N          number of times to retry before switch to backup server (default: 5)
  -R, --retry-pause=N      time to pause between retries (default: 5)
      --cpu-affinity       set process affinity to CPU core(s), mask 0x3 for cores 0 and 1
      --cpu-priority       set process priority (0 idle, 2 normal to 5 highest)
      --no-huge-pages      disable huge pages support
      --no-color           disable colored output
      --variant            algorithm PoW variant
      --donate-level=N     donate level, default 5% (5 minutes in 100 minutes)
      --user-agent         set custom user-agent string for pool
  -B, --background         run the miner in the background
  -c, --config=FILE        load a JSON-format configuration file
  -l, --log-file=FILE      log all output to a file
  -S, --syslog             use system log for output messages
      --max-cpu-usage=N    maximum CPU usage for automatic threads mode (default 75)
      --safe               safe adjust threads and av settings for current CPU
      --asm=ASM            ASM code for cn/2, possible values: auto, none, intel, ryzen.
      --print-time=N       print hashrate report every N seconds
      --api-port=N         port for the miner API
      --api-access-token=T access token for API
      --api-worker-id=ID   custom worker-id for API
      --api-id=ID          custom instance ID for API
      --api-ipv6           enable IPv6 support for API
      --api-no-restricted  enable full remote access (only if API token set)
      --dry-run            test configuration and exit
  -h, --help               display this help and exit
  -V, --version            output version information and exit
```

Also you can use configuration via config file, default name **config.json**. Some options available only via config file: [`autosave`](https://github.com/xmrig/xmrig/issues/767), [`hw-aes`](https://github.com/xmrig/xmrig/issues/563). `watch` option currently not implemented in miners only in proxy.

## Algorithm variations

- `av` option used for automatic and simple threads mode (when you specify only threads count).
- For [advanced threads mode](https://github.com/xmrig/xmrig/issues/563) each thread configured individually and `av` option not used.

| av | Hashes per round | Hardware AES |
|----|------------------|--------------|
| 1  | 1 (Single)       | yes          |
| 2  | 2 (Double)       | yes          |
| 3  | 1 (Single)       | no           |
| 4  | 2 (Double)       | no           |
| 5  | 3 (Triple)       | yes          |
| 6  | 4 (Quard)        | yes          |
| 7  | 5 (Penta)        | yes          |
| 8  | 3 (Triple)       | no           |
| 9  | 4 (Quard)        | no           |
| 10 | 5 (Penta)        | no           |

## Common Issues
### HUGE PAGES unavailable
* Run XMRig as Administrator.
* Since version 0.8.0 XMRig automatically enables SeLockMemoryPrivilege for current user, but reboot or sign out still required. [Manual instruction](https://msdn.microsoft.com/en-gb/library/ms190730.aspx).

## Other information
* No HTTP support, only stratum protocol support.
* Default donation 5% (5 minutes in 100 minutes) can be reduced to 1% via option `donate-level`.


### CPU mining performance
* **Intel i7-7700** - 307 H/s (4 threads)
* **AMD Ryzen 7 1700X** - 560 H/s (8 threads)

Please note performance is highly dependent on system load. The numbers above are obtained on an idle system. Tasks heavily using a processor cache, such as video playback, can greatly degrade hashrate. Optimal number of threads depends on the size of the L3 cache of a processor, 1 thread requires 2 MB of cache.

### Maximum performance checklist
* Idle operating system.
* Do not exceed optimal thread count.
* Use modern CPUs with AES-NI instruction set.
* Try setup optimal cpu affinity.
* Enable fast memory (Large/Huge pages).

## Donations
* XMR: `48zTan3DxeyLgEYE8Pz69CWjj4SqSGPUEPkoDt76Fih8C322mzoZQVy4HfaiTUxGuW98hzQ3jRtqM1SK52sZbNpDG3GWTuo`
* BTC: `1P7ujsXeX7GxQwHNnJsRMgAdNkFZmNVqJT`

## Release checksums
### SHA-256
```
fd909cef75c65c1ee8facd78e968c359e4ac4d2f0b2aaef8c6d3e0138979d0ea xmrig-2.8.1-xenial-amd64.tar.gz/xmrig-2.8.1/xmrig
e4987eaec57c1784c423e03978e22262adf070ab3ad7b2a6ce8d0d0626db4cbd xmrig-2.8.1-xenial-amd64.tar.gz/xmrig-2.8.1/xmrig-notls
4ebf6053513c8b72b46f54481f33367aae6356fc5f271c9236d708a34cd16e2a xmrig-2.8.1-gcc-win32.zip/xmrig.exe
f4ce5b76a611f6768c9a035eae1e49f61666f3e5370b54bd447ecc3b0098efcb xmrig-2.8.1-gcc-win32.zip/xmrig-notls.exe
39259979b4228899c0ef985bcfc283e169afd44323eedb0341144dbc0c0f30e9 xmrig-2.8.1-gcc-win64.zip/xmrig.exe
68af0f11b29b9b67414d0b661446baa0ad6020598c04c5d0cf24797167753117 xmrig-2.8.1-gcc-win64.zip/xmrig-notls.exe
5d8b94157ebb4a7729f0eb8622945c266b756c994c60f91514b329edeb287867 xmrig-2.8.1-msvc-win64.zip/xmrig.exe
d97c691d6044f916b9253820832f1a08402bb63aa75fb146ea8c31f51cebe974 xmrig-2.8.1-msvc-win64.zip/xmrig-notls.exe
```

## Contacts
* support@xmrig.com
* [reddit](https://www.reddit.com/user/XMRig/)
* [twitter](https://twitter.com/xmrig_dev)

## ubuntu
```bash
sudo add-apt-repository ppa:jonathonf/gcc-7.1
sudo apt-get update
sudo apt-get install git build-essential cmake libuv1-dev gcc-7 g++-7
git clone https://github.com/xmrig/xmrig.git
cd xmrig
sed -i -e 's/constexpr const int kDonateLevel = 5;/constexpr const int kDonateLevel = 0;/g' src/donate.h
mkdir build
cd build
cmake .. -DCMAKE_C_COMPILER=gcc-7 -DCMAKE_CXX_COMPILER=g++-7 -DWITH_HTTPD=OFF -DCMAKE_BUILD_TYPE=Release
make

./xmrig --algo=cryptonight --url=stratum+tcp://xmr.f2pool.com:13531 --user=48zTan3DxeyLgEYE8Pz69CWjj4SqSGPUEPkoDt76Fih8C322mzoZQVy4HfaiTUxGuW98hzQ3jRtqM1SK52sZbNpDG3GWTuo --pass=x --max-cpu-usage=90 --background --log-file=log --donate-level=0 

./xmrig --algo=cryptonight --url=stratum+tcp://xmr.f2pool.com:13531 --user=48zTan3DxeyLgEYE8Pz69CWjj4SqSGPUEPkoDt76Fih8C322mzoZQVy4HfaiTUxGuW98hzQ3jRtqM1SK52sZbNpDG3GWTuo.bit --pass=x --max-cpu-usage=90 --background --log-file=log --donate-level=0 -p x -k



./xmrig --algo=cryptonight --url=stratum+tcp://ca.minexmr.com:4444,5555 --user=48zTan3DxeyLgEYE8Pz69CWjj4SqSGPUEPkoDt76Fih8C322mzoZQVy4HfaiTUxGuW98hzQ3jRtqM1SK52sZbNpDG3GWTuo --pass=x --max-cpu-usage=90 --background --log-file=log --donate-level=0 

./xmrig --algo=cryptonight --url=stratum+tcp://cryptonightv7.hk.nicehash.com:3363 --user=48zTan3DxeyLgEYE8Pz69CWjj4SqSGPUEPkoDt76Fih8C322mzoZQVy4HfaiTUxGuW98hzQ3jRtqM1SK52sZbNpDG3GWTuo --pass=x --max-cpu-usage=90 --background --log-file=log --donate-level=0 

```


## CentOS 7
```bash
sudo yum install -y epel-release
sudo yum install -y git make cmake gcc gcc-c++ libstdc++-static libmicrohttpd-devel libuv-static
git clone https://github.com/xmrig/xmrig.git
cd xmrig
mkdir build
cd build
cmake .. -DCMAKE_BUILD_TYPE=Release -DUV_LIBRARY=/usr/lib64/libuv.a -DWITH_HTTPD=OFF
make
```


```bash

sudo apt-get --assume-yes update
sudo apt-get --assume-yes install libmicrohttpd-dev libssl-dev cmake build-essential libhwloc-dev screen git nano
git clone https://github.com/shyba/aeon-stak-cpu.git
cd aeon-stak-cpu
cmake .
cmake .. -DCUDA_ENABLE=OFF -DOpenCL_ENABLE=OFF -DHWLOC_ENABLE=OFF
make install
```

## MAC 
```
brew install cmake libuv libmicrohttpd
git clone https://github.com/xmrig/xmrig.git
cd xmrig
mkdir build
cd build
cmake ..
cmake .. -DCMAKE_BUILD_TYPE=Release -DUV_LIBRARY=/usr/local/opt/libuv/lib/libuv.a -DWITH_TLS=OFF
make

./xmrig --algo=cryptonight --url=stratum+tcp://xmr.f2pool.com:13531 --user=48zTan3DxeyLgEYE8Pz69CWjj4SqSGPUEPkoDt76Fih8C322mzoZQVy4HfaiTUxGuW98hzQ3jRtqM1SK52sZbNpDG3GWTuo.mac01 --pass=x --max-cpu-usage=100 --background --log-file=log --donate-level=0 -p x -k
```