# nanominer by nanopool
# version: 3.8
# Table of Contents
1. [Driver requirements](#driver-requirements)
1. [Reporting bugs and technical support](#reporting-bugs-and-technical-support)
1. [Setup](#setup)
1. [Log Files](#log-files)
1. [Remote Monitoring](#remote-monitoring)
1. [Automatic Restart Function](#automatic-restart-function)
1. [Parameters](#parameters)
1. [Configuration File](#configuration-file)
1. [Launching from command line](#launching-from-command-line)
1. [Examples of Configuration Files](#examples-of-configuration-files)

**nanominer** is a program product developed by nanopool to create structural cryptocurrency units based on the following algorithms:

|     Algo      |     Coin      | Dev Fee (once per 2 hours)  |     AMD     |    Nvidia   |  Intel Arc  |   CPU   |
|:-------------:|:-------------:|:---------------------------:|:-----------:|:-----------:|:-----------:|:-------:|
|  Ethash       |  ETHW & other |             1%              |   &check;   |   &check;   |   &check;   |         |
|  Etchash      |      ETC      |             1%              |   &check;   |   &check;   |   &check;   |         |
|  kHeavyhash   |      Kaspa    |             1%              |   &check;   |   &check;   |             |         |
|  Ubqhash      |      UBQ      |             1%              |   &check;   |   &check;   |   &check;   |         |
|  FiroPow      |      FIRO     |             1%              |   &check;   |   &check;   |             |         |
|  KawPow       |      RVN      |             2%              |   &check;   |   &check;   |             |         |
|  Octopus      |      CFX      |             2%              |   &check;   |   &check;   |             |         |
|  Autolykos2   |      ERG      |            2.5%             |   &check;   |   &check;   |             |         |
|  RandomX      |      XMR      |             2%              |             |             |             | &check; |
|  Verushash    |      VRSC     |             2%              |             |             |             | &check; |
|  Verthash     |      VTC      |             1%              |   &check;   |             |             |         |

**nanominer** also supports dual mining on the all Kaspa supported platforms (see config examples):
* ETH+KAS
* ETC+KAS
* ERG+KAS


**nanominer** also supports Zilliqa mining in the current configurations (see config examples):

| Configuration |  Merged (same pool) | Split (different pools)†|
|:-------------:|:-------------------:|:-----------------------:|
|    ETH+ZIL    |        &check;      |          &check;        |
|    ETC+ZIL    |        &check;      |          &check;        |
|    KAS+ZIL    |                     |          &check;        |
|    CFX+ZIL    |                     |          &check;        |
|    ERG+ZIL    |                     |          &check;        |
|    RVN+ZIL    |                     |          &check;        |
|    FIRO+ZIL   |                     |          &check;        |
When mining Zilliqa on a different pool, **nanominer** will use a placeholder `0xffffffffffffffffffffffffffffffffffffffff` ETH/ETC address to authorize on Zilliqa pool.
† Intel Arc does not support split Zilliqa mining at this moment (under construction).

**nanominer** also supports trial mining:
* ETH+KAS+ZIL
* ETC+KAS+ZIL
* ERG+KAS+ZIL

**nanominer** also supports quad mining:
* ETH+KAS+ZIL+XMR
* ETC+KAS+ZIL+XMR
* ERG+KAS+ZIL+XMR
* ETC+KAS+ZIL+VRSC
* ETH+KAS+ZIL+VRSC
* ERG+KAS+ZIL+VRSC


## Driver requirements

In order to work with Nvidia GPUs **nanominer** needs Nvidia driver **410.48 and newer on Linux** or **411.31 and newer on Windows**.
**nanominer cuda11** version needs Nvidia driver **455.23 and newer on Linux** or **456.38 and newer on Windows**.
Nvidia 30xx series of GPUs will only work with `cuda11` version of miner.

In order to begin mining Ethereum Classic with nanominer, ***it's enough to simply input your wallet*** in the configuration file.

Testing on **nanominer** demonstrated high performance working with Ethereum Classic, EthereumPOW, Ravencoin, Conflux, QuarkChain, Ubiq, Monero, Ergo, Firo and other currencies. Ethash, Etchash and KawPow implementations have some know-how optimizations for DAG size in memory which is critical for 3 GB and 4 GB GPUs. Independently of this, **nanominer** stands out with its high stability and simple setup.

## Reporting bugs and technical support
For reporting bugs, technical support, feature requests and community discussions feel free to use the following communication channels:
* Support email: support@nanominer.org
* GitHub issues tracker: https://github.com/nanopool/nanominer/issues
* Community chat in telegram (developers read it too): https://t.me/nanominer_en
* Our Discord channel https://discord.gg/JtKHCbm8Yg

## Setup
At launch **nanominer** reads the _config.ini_ setup file from the program's current directory. In order to
assign a specific name to the config file, it should be written as the first argument in the command
line. For example:
```
nanominer.exe config_etc.ini
```
When launching with the _-d_ command line option (e.g. `nanominer.exe -d`) the miner displays a list of the devices it detects, including their PCI addresses and their amount of memory. In order to use this function on Windows the program must be launched from the command prompt (cmd).

**nanominer** does not require any pools to be specified in the config file. If a pool (or list of pools) is not specified, **nanominer** will automatically use the pools on [nanopool.org](https://nanopool.org/) corresponding to the chosen cryptocurrency (except for coins not listed on Nanopool). QuarkChain public full nodes (fullnode.quarkchain.io and fullnode2.quarkchain.io) which are maintained by QuarkChain developers are used by default for QuarkChain.

When **nanominer** starts up it displays the main work information in the console log, including the program’s current version, the name of the rig, the number and type of graphics cards installed and the program’s current settings.
## Log Files
The event log function on **nanominer** is automatically activated each time the program starts up. The log files that are created contain all the information displayed on the console while the miner is running. By default, the log files are saved in the logs folder of the program's current directory. Deactivating event logging, as well as assigning a random catalogue for recording log files, can be done by using the corresponding configuration parameters (see the examples in the _Parameters_ section of this file).

## Remote Monitoring
**nanominer** has web interface for getting rig statistics, discovering other instances of **nanominer** in the local network and managing them. You can edit miners' config via web as well as restart the miners. In order to perform these actions on a running instance of **nanominer**, its config must contain a password for web interface (see the _webPassword_ option).
By default **nanominer** starts a HTTP server on port 9090, which can be found on http://127.0.0.1:9090. In the program's config file, the port can be configured and the API function can be deactived with the _webPort_ option (or it can be set to 0 to disable web interface).
[BoringAPI](https://github.com/nanopool/BoringAPI) for getting rig statistics is supported aswell, which can be found on http://127.0.0.1:9090/stats.

**nanominer** also supports the network API program EthMan for rig monitoring. By default it opens port 3333 in “read-only” mode without the ability to restart the miner or rig through the network. In the program's config file, the port can be configured and the API function can be deactived with the _mport_ function. The config file also lets you set a password for monitoring with the _ethmanPassword_ option.

## Automatic Restart Function
With default settings, **nanominer** will automatically restart if it encounters critical errors in the GPU or lag. (These errors usually arise due to hardware problems or overclocking the GPU.) The automatic restart function can be deactivated using the _watchdog_ parameter.

Likewise, the _minHashrate_ (minimum hashrate) parameter allows the user to set the value of the minimum hashrate which, if exceeded, will cause the miner to restart. This function uses the average hashrate over the last ten minutes, as displayed in blue in the console log. If the average hashrate over 10 minutes is lower than the set value, the miner will restart. With default settings the minimum hashrate is not set.

Another function on **nanominer** that improves the miner's automatic functioning is handled by the _restarts_ parameter.It sets the number of times the miner restarts before rebooting the worker (rig). By default the miner will only restart itself.

More detailed information on using these functions can be found in the _Parameters_ section of this file.

## Parameters
The settings for **nanominer** can be found in the configuration file with the *.ini extension (_config.ini_ by default). Config file can contain common params and algorithm params (in sections with corresponding algorithm names). Section names can be defined as “Ethash”, “Etchash”, “KawPow”, “Octopus”, “Ubqhash”, “RandomX”, “Verushash”, “Autolykos” or “Zilliqa”. Configuration file must be in the following format:
```ini
commonparameter1=commonvalue1
commonparameter2=commonvalue2
commonparameterX=commonvalueX

[AlgoName1]
devices=0,1
wallet = wallet1
algoparameter1=algovalue1_1
algoparameter2=algovalue1_2
algoparameterY=algovalue1_Y

[AlgoName2]
devices=2,3
wallet = wallet2
algoparameter1=algovalue2_1
algoparameter2=algovalue2_2
algoparameterZ=algovalue2_Z

[AlgoName3]
devices=4,5
wallet = wallet3
```
More config examples can be found below.

This file permits the presence of empty lines and comments. Comment lines should begin with a ";" (semicolon). The parameters and values are not case-sensitive, which means it makes no difference to the program whether you type _ETH_, _eth_ or _Eth_, or whether you type _wallet_ or _Wallet_. If an incorrect value is set for a parameter, the default value will be used instead (note that this rule does not apply to the _wallet_ parameter).

What follows is a list of the parameters that can be set on **nanominer**.
### wallet
Mandatory parameter.
This is the user's wallet, where funds will be deposited.
### coin
Optional parameter.
This chooses the default coin for the pool. The default pool is [nanopool.org](https://nanopool.org/).
The coin parameter accepts one of the following values: ETHW (or EthereumPOW), ETC (or Ethereum Classic), RVN (or Raven), CFX (or Conflux), QKC (or QuarkChain), UBQ (or Ubiq), XMR (or Monero), VRSC (or Verus), ERG (or Ergo), VTC (or Vertcoin), FIRO, ZIL (or Zilliqa). When a coin is specified and equals one of the values mentioned above, **nanominer** automatically tries to determine the pool necessary for it to function if none have been provided in a separate parameter. If a coin is specified but **nanominer** cannot recognize it, then the name of the coin is used only for logging. If a coin is not specified, **nanominer** will use the default coin for the corresponding algorithm. Moreover, if [nanopool.org](https://nanopool.org/) is specified in the configuration file for Ethereum Classic, EthereumPOW, Ergo or Monero, **nanominer** will determine the coin from the pool's settings.

*Important*: when using **nanominer** to mine EthereumPOW on the default pool, it is necessary to define the coin (coin=ETHW). In that case the pools will be determined automatically.

If the pools are clearly defined with the aid of the _pool1, pool2, ..._, parameters, then **nanominer** will function according to the tasks it receives from those pools.
### rigName
Optional parameter. Can be specified in common parameter section instead of the algorithm section to be applied for all algorithms at once.
This is the name of the rig (computer/worker). It will be displayed in the pool's statistics. If this parameter is not set, the program will generate a unique name and provide it to the pool. To disable rigname completely just set it to empty string with
```ini
rigName=
```
### email
Optional parameter. Can be specified in common parameter section instead of the algorithm section to be applied for all algorithms at once.
This is the user’s e-mail address. It is provided to the pool where the rig will be operating. The pool can use it when sending out service notifications.
### pool1, pool2, ...
Optional parameter.
This defines the set of mining pools used. Values must be given in the format url:port (e.g. `pool1=etc-eu1.nanopool.org:19999`). The parameters should be defined in ascending, sequential order, from pool1 to poolN (for example: pool1, pool2, pool3). If the pool list is provided, the best pool will be chosen from the order of the pool list. If a `sortPools=true` option is specified, the best pool will be chosen by the connection speed. If the pool (or list of pools) is not defined, **nanominer** will automatically use the pools on [nanopool.org](https://nanopool.org/) that correspond to the chosen cryptocurrency. For QuarkChain public full nodes are used if no pools are defined. For Ubiq [Ubiqpool.io](https://ubiqpool.io) pools are used if no pools are defined.

### protocol
Optional parameter.
Can be used to set the pool protocol to _stratum_. If not specified, **nanominer** will try to detect the pool protocol automatically.

### rigPassword
Optional parameter.
The password for the rig (or worker). It may be necessary when working with pools that require registration and setting a rig password.
### watchdog
Optional parameter.
This parameter manages the miner's restart function when running into critical GPU errors or lag. It accepts the values _true_ or _false_. By default, _true_ – automatic restart – is activated.
### restarts
Optional parameter.
This parameter sets the number of times the miner will restart before rebooting the rig. In case of GPU problems like hardware errors or lag, or in case of hashrate degradation (if the _minhashrate_ option is used), **nanominer** will restart. However, certain errors cannot be fixed by restarting the program. In such cases it is necessary to reboot the rig. To reboot, the miner loads the _reboot.bat_ script from the current directory if running on Windows or _reboot.sh_ if on Linux:
```
reboot
```
**The reboot.sh file on Linux must be given execute permissions in order for it to work.**
The typical content of the _reboot.bat_ script for Windows:
```
shutdown /r /t 5 /f
```
The script must be written by the user.
To run reboot script instead of restarting miner every time a critical error occurs, just set `restarts=0`
### minHashrate
Optional parameter.
This is the minimum acceptable hashrate. This function keeps track of the rig's total hashrate and compares it with this parameter. If five minutes after the miner is launched the set minimum is not reached, **nanominer** will automatically restart. Likewise, the miner will restart if for any reason the average hashrate over a ten-minute period falls below the set value. This value can be set with an optional modifier letter that represents a thousand for kilohash or a million for megahash per second. For example, setting the value to 100 megahashes per second can be written as 100M, 100.0M, 100m, 100000k, 100000K or 100000000. If this parameter is not defined, the miner will not restart (with the exception of the situations described in the _watchdog_ section). Restarts caused by this option count towards the _restarts_ parameter.
### maxRejectedShares
Optional paramter.
Can be used to set the maximum amount of rejected shares before restarting miner process/rebooting the rig. Restarts caused by this option count towards the _restarts_ parameter.
Option is disabled by default.
### devices
Optional paramter.
These are the graphics cards that will be used by the miner. If you do not want to launch the miner on all available GPUs but only on some of them, their numbers can be provided in the _devices_ parameter separated by a comma or space. **nanominer** numbers the GPUs starting from zero in ascending order of their PCI addresses. You can see a list of available GPUs and the order in which they're in by launching **nanominer** with the _-d_ command line option:
```
nanominer -d
```
For example, if there are four GPUs in the system (0, 1, 2, 3) and all but the second-to-last one (indexed as 2) must be set to mine, then the devices option must be set in the following manner:
```ini
devices=0,1,3
```
The order of devices determines the order of displayed hashrate. For example, if it is set as
```ini
devices=3,1,0
``` 
then the hashrate line will first display GPU3, then GPU1 and finally GPU0.

### checkForUpdates
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _true_). If this parameter is set to _false_ then **nanominer** stops checking for the newest release version on every startup.

### autoUpdate
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ and checking for updates is enabled, then **nanominer** will update itself on every startup, provided there is a newer version available.

### coreClocks, memClocks
Optional parameters.
Can be used to overclock/underclock NVIDIA GPU's. Absolute (e.g. 4200) as well as relative (e.g. +200, -150) values in MHz are accepted. Parameter values must be separated by a comma or space (first value is for GPU0, second is for GPU1, and so on). For example, if it is set as
```ini
coreClocks=+200,-150
memClocks=+300,3900
```
then GPU0 will be overclocked by 200 MHz of core and 300 MHz of memory, whereas GPU1 core clock will be underclocked by 150 MHz, and its memory clock set to 3900 MHz.
You can also apply same settings for each GPU by defining only one core and memory clock value, for example:
```ini
coreClocks=+200
memClocks=+300
```

### powerLimits
Optional parameter.
Can be used to set Nvidia cards power limits from -50 to 50. For example, -20 means 80% power limit, 10 means 110% power limit. Parameter values must be separated by a comma or space (first value is for GPU0, second is for GPU1, and so on). You can also apply same settings for each GPU by defining only one power limit value.

### memTweak
Optional parameter.
Can be set to modify AMD GPU timings on the fly for Ethash/Etchash/Ubqhash algorithms. The following AMD ASICs are currently supported: gfx900, gfx901, gfx906, gfx907, Baffin, Ellesmere, gfx804, Hawaii, Tahiti, Pitcairn, Tonga.

Miner must be launched using admin/root privileges in order to change timings.

Default memory tweak value is 1 which means slightly improving memory timings. Zero value means timings are left as is without modifications. Parameter values must be separated by a comma or space (first value is for GPU0, second is for GPU1, and so on). Supported memory tweak value range is from 0 to 10 (0 means disabling timings modification, 1 is the least intense, 10 is the most intense), for example:
```ini
memTweak=9,8,10
```
It is recommended to begin from lower values and increase them if the miner works stably.

You can also apply same settings for each GPU by defining only one memory tweak value:
```ini
memTweak=10
```

### fanSpeed
Optional parameter.
Used to set the GPU fan speed to a specific percentage from 30% to 100%. If below 30, automatically sets to 30. If the value is incorrect, i.e. negative or non-numeric value, sets to the last used value.

### epoch
Optional parameter.
Ethash algorithm specific option to check miner behaviour on different Ethash epochs.

### zilEpoch
Optional parameter.
Sets the epoch of Zilliqa DAG to store in GPU memory (default is 0).

### noLog
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then no log files will be recorded onto the hard drive.

### noColor
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then the console output won't contain any colors.

### logPath
Optional parameter.
This parameter can either be used to set the name of the folder in which log files will be created (e.g. `logPath=logfolder/`), or to specify a path to single file, which will be used for all logs (e.g. `logPath=logs/log.txt`, `logPath=/var/log/nanominer/log.txt`, `logPath=C:\logs\log.txt`). Both relative and absolute paths work.
Default value for this parameter is _logs/_.

### webPassword
Optional parameter.
Password for web interface. There is no password by default (web interface is read-only).

### webPort
Optional parameter.
Port for web interface. The default port is 9090. Zero value disables web interface.

### mport
Optional parameter.
This is the network port for remote monitoring and program management through EthMan or other programs that use a similar API protocol format.
The program supports all API functions, including restarting the miner and rig(s).
You can block miner management through API (in which case the miner will only display the statistics and won't respond to any commands). To enable this function, a "minus" (-) sign must be written before the port number.
And you can completely deactivate remote monitoring. To do this, the port number must be set to "0" (zero).
Default value: -3333 (This means that the miner blocks management through API and displays statistics on port 3333).

### ethmanPassword
Optional parameter.
Your password for monitoring with EthMan and other utilities that support the same network API.

### useSSL
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _true_). If this parameter is set to _true_ then miner always tries to use SSL pool connection first and fallbacks to unencrypted connection if SSL connection failed. If this parameter is set to _false_ then miner doesn't try using SSL for pool connection.

### shardId
Optional parameter.
Can be used to set a shard ID for QuarkChain solo mining. This parameter should be specified in hex, e.g. 0x1, 0x10001, 0x10002, 0x50001, etc. For root chain shard ID `null` must be specified. For more information on shards, visit [this](https://github.com/QuarkChain/pyquarkchain/wiki/Address,-Shard-Key,-Chain-Id,-Shard-Id) and [this](https://github.com/quarkChain/pyquarkchain/releases/latest) link. Default shard ID is 0x1. Shard ID is passed to QuarkChain node "as is" so all current and future Ethash shards are supported.

### farmRecheck
Optional parameter.
The interval (in milliseconds) between polling the node for new jobs in solo mining mode for QuarkChain. Default value is 200. 

### cpuThreads
Optional parameter for CPU mining.
Specifies the number of concurrent CPU threads to use for mining. All threads are used by default.

### sortPools
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then the best pool will be chosen by least ping (not by the pool list).

### countDevShares
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_ for QuarkChain solo mining and _true_ for other coins). If this parameter is set to _true_ then shares accepted or rejected by pool during fee time will be included in miner statistics. Otherwise only shares during user mining are included to miner statistics.

### validateShares
Optional parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then shares of ethash algorithms family on AMD GPUs are validated by CPU. Also in this case share difficulty is shown for AMD GPUs.

### sendHashrate
Optional parameter for Ethash, Etchash and Ubqhash algorithms. This parameter accepts the values _true_ or _false_. The default value is _true_ (if JSON-RPC pool protocol is used).

### silence
Optional parameter. This parameter accepts the values from 0 to 3. Control the amount of information displayed in the logs and on the screen.
0: All the information is logged. Default behaviour.
1: Hide frequent job messages.
2: Also hide shares messages.
3: Also hide hashrate prints.

### dagSer
Optional parameter for Ethash, Etchash, FiroPow and KawPow algorithms.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then the DAG will be generated sequentially on each GPU. Otherwise all the GPUs generate DAG at the same time.

### lhr
Optional parameter for Ethash and Etchash algorithms. Can be used to set desired percentage of full unlocked hashrate. Valid range is from 50 to 100. Also can be set to _off_ (-1) or _auto_ (0). Use _off_ for non-LHR cards and _auto_ for automatic LHR card detection and tuning. Default is _auto_. This parameter can be set for each GPU separately. In this case, order must correspond to the order of GPUs, specified in _devices_ parameter. For example:
```ini
devices=0,2,3
lhr=71.5,off,0
```
means 71.5% for GPU0, _off_ for GPU2 and _auto_ for GPU3.

## Configuration File
The minimum configuration file for Ethereum Classic may contain only a wallet:
```ini
wallet=<wallet>
```
**nanominer** will automatically use Ethereum Classic pools.

To work with EthereumPOW, the coin must be specified:
```ini
wallet=<wallet>
coin=ETHW
```
In this case **nanominer** will use pools corresponding to EthereumPOW.

## IMPORTANT!
For coins that are not supported by [nanopool.org](https://nanopool.org/), **you must** specify a **wallet** and pools (**pool1...**).

## Launching from command line
It is possible to run nanominer from the command line with the desired arguments. At least one algorithm and wallet are required to be passed. All common config parameters in the command line must be specified before the first "algo" parameter. Here are some examples of command lines for launching Ethereum Classic and Monero:

Windows:
```
nanominer.exe -algo etchash -wallet YOUR_ETC_WALLET -coin eth -rigName YOUR_ETC_WORKER -email YOUR_EMAIL -algo randomx -wallet YOUR_XMR_WALLET -coin xmr -rigName YOUR_XMR_WORKER -email YOUR_EMAIL 
```
Linux:
```
./nanominer -algo etchash -wallet YOUR_ETC_WALLET -coin eth -rigName YOUR_ETC_WORKER -email YOUR_EMAIL -algo randomx -wallet YOUR_XMR_WALLET -coin xmr -rigName YOUR_XMR_WORKER -email YOUR_EMAIL 
```

If no algo specified, etchash is used by default. So the following commands will launch the etchash algorithm:

Windows:
```
nanominer.exe -wallet YOUR_WALLET
```
Linux:
```
./nanominer -wallet YOUR_WALLET
```

The helper scripts folder with all of its contents is still there, for those who use it.

## Examples of Configuration Files
Example of configuration file for quad mining Ergo + Kaspa + Zilliqa + Monero
```ini
[autolykos]
wallet = 9he6BZYMN8FMKxYKsqPvQJ6fbNar4bWuhJsR9JJt4x9Z6fiqSo1
; nanopool pools by default

[zil]
wallet = zil1rpxnv479xy9c2jlgry3wy3869rnt4rjvjwjtuv
zilEpoch = 0 ; number of DAG epoch for caching
pool1 = eu.ezil.me:4444

[heavyhash]
wallet = kaspa:qr36zdxs0dn3n0h799jhdl02qks5742lxjgmfsfj9xmlca7n4l6mw0s0n48nx
rigname = test_speed
pool1 = pool.woolypooly.com:3112


[RandomX]
coin = xmr
wallet = 46TgqBPmxFYiEhWfwGMRWWaqyrardCVB2JtZCKyAmatGeuPWMsNAJmFU3cCiTSn16XT2nw5XMXcJVidVxR46F3i57N5K7NN
```

Example of a configuration file for Kaspa:
```ini
wallet = kaspa:qr36zdxs0dn3n0h799jhdl02qks5742lxjgmfsfj9xmlca7n4l6mw0s0n48nx
rigname = test_speed
pool1 = pool.woolypooly.com:3112
```

Example of a configuration file for Ethereum Classic and Monero:
```ini
[Etchash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = etc-eu1.nanopool.org:19999
pool2 = etc-eu2.nanopool.org:19999
pool3 = etc-us-east1.nanopool.org:19999
pool4 = etc-us-west1.nanopool.org:19999
pool5 = etc-asia1.nanopool.org:19999
pool6 = etc-jp1.nanopool.org:19999
pool7 = etc-au1.nanopool.org:19999
[RandomX]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = xmr-eu1.nanopool.org:14433
pool2 = xmr-eu2.nanopool.org:14433
pool3 = xmr-us-east1.nanopool.org:14433
pool4 = xmr-us-west1.nanopool.org:14433
pool5 = xmr-asia1.nanopool.org:14433
```
Example of a configuration file for split Ethereum Classic and Zilliqa:
```ini
[Etchash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
; nanopool by default
[Zilliqa]
wallet = zilxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
pool1 = eu.ezil.me:4444
pool2 = us-west.ezil.me:4444
pool3 = asia.ezil.me:4444
```

Example of a configuration file for merged Ethereum Classic and Zilliqa:
```ini
[Etchash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff.zilxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
zilEpoch = 0
pool1 = eu.ezil.me:4444
pool2 = us-west.ezil.me:4444
pool3 = asia.ezil.me:4444
```
Example of a configuration file for Ethereum Classic:
```ini
[Etchash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = etc-eu1.nanopool.org:19999
pool2 = etc-eu2.nanopool.org:19999
pool3 = etc-us-east1.nanopool.org:19999
pool4 = etc-us-west1.nanopool.org:19999
pool5 = etc-asia1.nanopool.org:19999
pool6 = etc-jp1.nanopool.org:19999
pool7 = etc-au1.nanopool.org:19999
```
Example of an equivalent file for Ethereum Classic:
```ini
[Etchash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
```
Example of a minimum file for Ethereum Classic:
```ini
[Etchash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
```
Example of a configuration file for split EthereumPOW and Zilliqa:
```ini
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
; nanopool by default
[Zilliqa]
wallet = zilxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
pool1 = eu.ezil.me:4444
pool2 = us-west.ezil.me:4444
pool3 = asia.ezil.me:4444
```
Example of a configuration file for EthereumPOW:
```ini
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
coin=ETHW
rigName = rig1
email = someemail@org
pool1 = ethw-eu1.nanopool.org:15433
pool2 = ethw-eu2.nanopool.org:15433
pool3 = ethw-us-east1.nanopool.org:15433
pool4 = ethw-us-west1.nanopool.org:15433
pool5 = ethw-asia1.nanopool.org:15433
pool6 = ethw-jp1.nanopool.org:15433
pool7 = ethw-au1.nanopool.org:15433
```
Example of an equivalent file for EthereumPOW:
```ini
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
coin=ETHW
rigName = rig1
email = someemail@org
```
Example of a minimum file for EthereumPOW:
```ini
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
coin=ETHW
```
Example of a complete configuration file for solo QuarkChain mining:
```ini
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
shardId=0x30001
farmRecheck=200
coin=Qkc
pool1=localhost:38391
protocol=getwork
```
Example of a minimum file for solo QuarkChain mining:
```ini
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
coin=Qkc
pool1=localhost:38391
shardId=0x50001
```
Example of a file for solo QuarkChain mining on root shard:
```ini
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
coin=Qkc
pool1=localhost:38391
shardId=null
```
Example of a minimum file for QuarkChain mining using public nodes:
```ini
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
coin=Qkc
shardId=0x30001
```

Example of a configuration file for Ubiq:
```ini
[Ubqhash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
coin=Ubq
rigName = rig1
email = someemail@org
pool1 = us.ubiqpool.io:8008
pool2 = eu.ubiqpool.io:8008
```
Example of a minimum file for Ubiq:
```ini
coin=UBQ
wallet=0xffffffffffffffffffffffffffffffffffffffff
```
Example of a complete file for Monero:
```ini
[RandomX]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = xmr-eu1.nanopool.org:14433
pool2 = xmr-eu2.nanopool.org:14433
pool3 = xmr-us-east1.nanopool.org:14433
pool4 = xmr-us-west1.nanopool.org:14433
pool5 = xmr-asia1.nanopool.org:14433
```
Example of an equivalent file for Monero:
```ini
[RandomX]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
```
Example of a minimum file for Monero:
```ini
[RandomX]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```

Example of a configuration file for Ravencoin:
```ini
[Kawpow]
wallet = Rrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrr
coin=Rvn
rigName = rig1
email = someemail@org
pool1 = rvn-eu1.nanopool.org:12433
pool2 = rvn-eu2.nanopool.org:12433
pool3 = rvn-us-east1.nanopool.org:12433
pool4 = rvn-us-west1.nanopool.org:12433
pool5 = rvn-asia1.nanopool.org:12433
pool6 = rvn-jp1.nanopool.org:12433
pool7 = rvn-au1.nanopool.org:12433

```
Example of a minimum file for Ravencoin:
```ini
wallet=Rrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrrr
```

Example of a complete file for Conflux:
```ini
[Octopus]
wallet = 0x1fffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1=cfx-eu1.nanopool.org:17433
pool2=cfx-eu2.nanopool.org:17433
pool3=cfx-us-east1.nanopool.org:17433
pool4=cfx-us-west1.nanopool.org:17433
pool5=cfx-asia1.nanopool.org:17433
pool6=cfx-jp1.nanopool.org:17433
pool7=cfx-au1.nanopool.org:17433
```

Example of an equivalent file for Conflux:
```ini
[Octopus]
wallet = 0x1fffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
```
Example of a minimum file for Conflux:
```ini
wallet = 0x1fffffffffffffffffffffffffffffffffffffff
coin = CFX
```
Example of a configuration file for Conflux and Zilliqa:
```ini
[Octopus]
wallet = 0x1fffffffffffffffffffffffffffffffffffffff
[Zilliqa]
wallet = zilxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
pool1 = eu.ezil.me:4444
pool2 = us-west.ezil.me:4444
pool3 = asia.ezil.me:4444
```

Example of a complete file for VerusCoin:
```ini
[Verushash]
wallet = REoPcdGXthL5yeTCrJtrQv5xhYTknbFbec
coin = VRSC
rigName = speed_test
rigPassword=d=4
cpuThreads=4
pool1 = na.luckpool.net:3956
```

Example of a minimum file for VerusCoin:
```ini
coin = VRSC
wallet = REoPcdGXthL5yeTCrJtrQv5xhYTknbFbec
pool1 = na.luckpool.net:3956
```

Example of a complete file for Ergo:
```ini
[autolykos]
wallet = 9he6BZYMN8FMKxYKsqPvQJ6fbNar4bWuhJsR9JJt4x9Z6fiqSo1
rigName = rig1
email = someemail@org
pool1 = ergo-eu1.nanopool.org:11433
pool2 = ergo-us-east1.nanopool.org:11433
pool3 = ergo-us-west1.nanopool.org:11433
pool4 = ergo-eu2.nanopool.org:11433
pool5 = ergo-asia1.nanopool.org:11433
pool6 = ergo-jp1.nanopool.org:11433
pool7 = ergo-au1.nanopool.org:11433

```
Example of an equivalent file for Ergo:
```ini
[autolykos]
wallet = 9he6BZYMN8FMKxYKsqPvQJ6fbNar4bWuhJsR9JJt4x9Z6fiqSo1
rigName = rig1
email = someemail@org
```
Example of a minimum file for Ergo:
```ini
coin=ergo
wallet = 9he6BZYMN8FMKxYKsqPvQJ6fbNar4bWuhJsR9JJt4x9Z6fiqSo1
```

Example of a configuration file for Firo:
```ini
[FiroPow]
wallet = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
coin=Firo
rigName = rig1
email = someemail@org
pool1 = firo-eu1.picopool.org:22222

```
Example of a minimum file for Firo:
```ini
wallet=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

Example of dual mining Kaspa + Zilliqa
```ini
[heavyhash]
silence = 1 ; hide frequent job messages
wallet = kaspa:qr36zdxs0dn3n0h799jhdl02qks5742lxjgmfsfj9xmlca7n4l6mw0s0n48nx
rigname = test_speed
pool1 = pool.woolypooly.com:3112

[zil]
wallet = zil1rpxnv479xy9c2jlgry3wy3869rnt4rjvjwjtuv
zilEpoch = 0 ; number of DAG epoch for caching
pool1 = eu.ezil.me:4444
```

Example of configuration file for mining Ethereum Classic, Ergo, Ubiq and Monero on same 8 GPUs rig using separate devices:
```ini
rigName = rig1
[Etchash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
devices = 0,1
[Autolykos]
wallet = 9he6BZYMN8FMKxYKsqPvQJ6fbNar4bWuhJsR9JJt4x9Z6fiqSo1
devices = 5
[Ubqhash]
wallet = 0x1111111111111111111111111111111111111111
pool1 = eu.ubiqpool.io:8008
devices = 2,3,4,6,7
[RandomX]
wallet=fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```

