Support email: support@miner.finom.io  
Our Discord channel https://discord.gg/eyWkqG3

# nanominer by nanopool
# version: 1.0
**nanominer** is a program product developed by nanopool to create structural cryptocurrency units on the framework of the Ethash, Ubqhash, CryptoNight (v6, v7, v8) and RandomHash algorithms. The present version of **nanominer** was made to work with every cryptocurrency based on these algorithms, including Ethereum, Ethereum Classic, Ubiq, Monero, PascalCoin and many others. This version of **nanominer** runs on Windows or Linux with AMD or Nvidia graphics cards (for Ethash and CryptoNight algorithms). The RandomHash algorithm is supported only on CPU.

In order to begin mining Ethereum with nanominer, ***it's enough to simply input your wallet*** in the configuration file.

Testing on **nanominer** demonstrated high performance working with Ethereum, Ethereum Classic, Ubiq, Monero, PascalCoin and other currencies. As a result of the research carried out, it was found that **nanominer** performs on par with, and sometimes better than, competing program products. Independently of this, **nanominer** stands out with its high stability and simple setup.
## Payment
Payment for the use of **nanominer** takes the form of a commission from mining to its wallets. The commission is:
- 1% of total mining time for any GPU algorithm;
- 3% for RandomHash on CPU in case there is at least one GPU algorithm launched in parallel;
- 5% for RandomHash on CPU in case there are no GPU algorithms launched in parallel.

## Setup
At launch **nanominer** reads the _config.ini_ setup file from the program's current directory. In order to
assign a specific name to the config file, it should be written as the first argument in the command
line. For example:
```
nanominer.exe config_etc.ini
```
When launching with the _-d_ command line option (e.g. `nanominer.exe -d`) the miner displays a list of the devices it detects, including their PCI addresses and their amount of memory. In order to use this function on Windows the program must be launched from the command prompt (cmd).

**nanominer** does not require any pools to be specified in the config file. If a pool (or list of pools) is not specified, **nanominer** will automatically use the pools on [nanopool.org](https://nanopool.org/) corresponding to the chosen cryptocurrency (except for Ubiq).

When **nanominer** starts up it displays the main work information in the console log, including the program’s current version, the name of the rig, the number and type of graphics cards installed and the program’s current settings.
## Log Files
The event log function on **nanominer** is automatically activated each time the program starts up. The log files that are created contain all the information displayed on the console while the miner is running. By default, the log files are saved in the logs folder of the program's current directory. Deactivating event logging, as well as assigning a random catalogue for recording log files, can be done by using the corresponding configuration parameters (see the examples in the _Parameters_ section of this file).

## Remote Monitoring
**nanominer** supports BoringAPI for getting rig statistics. By default it starts a BoringAPI HTTP server on port 9090, which can be found on http://127.0.0.1:9090/stats. In the program's config file, the port can be configured and the API function can be deactived with the _port_ function.

**nanominer** also supports the network API program EthMan for rig monitoring. By default it opens port 3333 in “read-only” mode without the ability to restart the miner or rig through the network. In the program's config file, the port can be configured and the API function can be deactived with the _mport_ function. The config file also lets you set a password for monitoring with the _ethmanPassword_ option.

## Automatic Restart Function
With default settings, **nanominer** will automatically restart if it encounters critical errors in the GPU or lag. (These errors usually arise due to hardware problems or overclocking the GPU.) The automatic restart function can be deactivated using the _watchdog_ parameter.

Likewise, the _minHashrate_ (minimum hashrate) parameter allows the user to set the value of the minimum hashrate which, if exceeded, will cause the miner to restart. This function uses the average hashrate over the last ten minutes, as displayed in blue in the console log. If the average hashrate over 10 minutes is lower than the set value, the miner will restart. With default settings the minimum hashrate is not set.

Another function on **nanominer** that improves the miner's automatic functioning is handled by the _restarts_ parameter.It sets the number of times the miner restarts before rebooting the worker (rig). By default the miner will only restart itself.

More detailed information on using these functions can be found in the _Parameters_ section of this file.

## Parameters
The settings for **nanominer** can be found in the configuration file with the *.ini extension (_config.ini_ by default). Config file can contain common params and algorithm params (in sections with corresponding algorithm names). Section names can be defined as “Ethash”, “Ubqhash”, “CryptoNightv8”, “CryptoNightv7”, “CryptoNight” or “RandomHash”. Configuration file must be in the following format:
```
commonparameter1=commonvalue1
commonparameter2=commonvalue2
commonparameterX=commonvalueX
...

[AlgoName1]
devices=0,1
wallet = wallet1
algoparameter1=algovalue1_1
algoparameter2=algovalue1_2
algoparameterY=algovalue1_Y
...

[AlgoName2]
devices=2,3
wallet = wallet2
algoparameter1=algovalue2_1
algoparameter2=algovalue2_2
algoparameterZ=algovalue2_Z

[AlgoName3]
devices=4,5
wallet = wallet3
...
```
More config examples can be found below.

This file permits the presence of empty lines and comments. Comment lines should begin with a ";" (semicolon). The parameters and values are not case-sensitive, which means it makes no difference to the program whether you type _ETH_, _eth_ or _Eth_, or whether you type _wallet_ or _Wallet_. If an incorrect value is set for a parameter, the default value will be used instead (note that this rule does not apply to the _wallet_ parameter).

What follows is a list of the parameters that can be set on **nanominer**.
### wallet
Mandatory parameter.
This is the user's wallet, where funds will be deposited.
### paymentId
Optional algorithm parameter, can be defined for wallets created on an exchange where the user has a personal
payment number in addition to their wallet.
### coin
Optional algorithm parameter.
This chooses the default coin for the pool. The default pool is [nanopool.org](https://nanopool.org/).
The coin parameter accepts one of three values: ETH (or Ethereum), ETC (or Ethereum Classic) and XMR (or Monero). When a coin is specified and equals one of the values mentioned above, **nanominer** automatically tries to determine the pool necessary for it to function if none have been provided in a separate parameter. If a coin is specified but **nanominer** cannot recognize it, then the name of the coin is used only for logging. If a coin is not specified, **nanominer** will use the default coin for the corresponding algorithm (Ethereum or Monero). Moreover, if [nanopool.org](https://nanopool.org/) is specified in the configuration file for Ethereum, Ethereum Classic or Monero, **nanominer** will determine the coin from the pool's settings.

*Important*: when using **nanominer** to mine Ethereum Classic on the default pool, it is necessary to define the coin (coin=ETC). In that case the pools will be determined automatically.

If the pools are clearly defined with the aid of the _pool1, pool2, ..._, parameters, then **nanominer** will function according to the tasks it receives from those pools.
### rigName
Optional algorithm parameter. Can be specified in common parameter section instead of the algorithm section to be applied for all algorithms at once.
This is the name of the rig (computer/worker). It will be displayed in the pool's statistics. If this parameter is not set, the program will generate a unique name and provide it to the pool.
### email
Optional algorithm parameter. Can be specified in common parameter section instead of the algorithm section to be applied for all algorithms at once.
This is the user’s e-mail address. It is provided to the pool where the rig will be operating. The pool can use it when sending out service notifications.
### pool1, pool2, ...
Optional algorithm parameter.
This defines the set of mining pools used. Values must be given in the format url:port (e.g. `pool1=eth-eu1.nanopool.org:9999`). The parameters should be defined in ascending, sequential order, from pool1 to poolN (for example: pool1, pool2, pool3). The pool will be chosen automatically from the list in accordance with the maximum connection speed. If the pool (or list of pools) is not defined, **nanominer** will automatically use the pools on [nanopool.org](https://nanopool.org/) that correspond to the chosen cryptocurrency.

### protocol
Optional algorithm parameter.
Can be used to set the pool protocol to _stratum_. If not specified, **nanominer** will try to detect the pool protocol automatically.

### rigPassword
Optional algorithm parameter.
The password for the rig (or worker). It may be necessary when working with pools that require registration and setting a rig password.
### watchdog
Optional common parameter.
This parameter manages the miner's restart function when running into critical GPU errors or lag. It accepts the values _true_ or _false_. By default, _true_ – automatic restart - is activated.
### minHashrate
Optional algorithm parameter.
This is the minimum acceptable hashrate. This function keeps track of the rig's total hashrate and compares it with this parameter. If five minutes after the miner is launched the set minimum is not reached, **nanominer** will automatically restart. Likewise, the miner will restart if for any reason the average hashrate over a ten-minute period falls below the set value. This value can be set with an optional modifier letter that represents a thousand for kilohash or a million for megahash per second. For example, setting the value to 100 megahashes per second can be written as 100M, 100.0M, 100m, 100000k, 100000K or 100000000. If this parameter is not defined, the miner will not restart (with the exception of the situations described in the _watchdog_ section).
### devices
Optional paramter.
These are the graphics cards that will be used by the miner. If you do not want to launch the miner on all available GPUs but only on some of them, their numbers can be provided in the _devices_ parameter separated by a comma. **nanominer** numbers the GPUs starting from zero in ascending order of their PCI addresses. You can see a list of available GPUs and the order in which they're in by launching **nanominer** with the _-d_ command line option:
```
nanominer -d
```
For example, if there are four GPUs in the system (0, 1, 2, 3) and all but the second-to-last one (indexed as 2) must be set to mine, then the devices option must be set in the following manner:
```
devices=0,1,3
```
The order of devices determines the order of displayed hashrate. For example, if it is set as
```
devices=3,1,0
``` 
then the hashrate line will first display GPU3, then GPU1 and finally GPU0.

### restarts
Optional common parameter.
This parameter sets the number of times the miner will restart before rebooting the rig. In case of GPU problems like hardware errors or lag, or in case of hashrate degradation (if the _minhashrate_ option is used), **nanominer** will restart. However, certain errors cannot be fixed by restarting the program. In such cases it is necessary to reboot the rig. To reboot, the miner loads the _reboot.bat_ script from the current directory if running on Windows or _reboot.sh_ if on Linux:
```
reboot
```
The typical content of the _reboot.bat_ script for Windows:
```
shutdown /r /t 5 /f
```
The script must be written by the user.

### noLog
Optional common parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then no log files will be recorded onto the hard drive.

### logPath
Optional common parameter.
This parameter can either be used to set the name of the folder in which log files will be created (e.g. `logPath=logfolder/`), or to specify a path to single file, which will be used for all logs (e.g. `logPath=logs/log.txt`, `logPath=/var/log/nanominer/log.txt`, `logPath=C:\logs\log.txt`). Both relative and absolute paths work.
Default value for this parameter is _logs/_.

### port
Optional common parameter.
Port for BoringAPI monitoring server, which can be used for sending remote HTTP requests (e.g. http://127.0.0.1:9090/stats) for getting the mining statistics. The default port is 9090.

### mport
Optional common parameter.
This is the network port for remote monitoring and program management through EthMan or other programs that use a similar API protocol format.
The program supports all API functions, including restarting the miner and rig(s).
You can block miner management through API (in which case the miner will only display the statistics and won't respond to any commands). To enable this function, a "minus" (-) sign must be written before the port number.
And you can completely deactivate remote monitoring. To do this, the port number must be set to "0" (zero).
Default value: -3333 (This means that the miner blocks management through API and displays statistics on port 3333).

### ethmanPassword
Optional common parameter.
Your password for monitoring with EthMan and other utilities that support the same network API.

### cpuThreads
Optional algorithm parameter for CPU mining.
Specifies the number of concurrent CPU threads to use for mining. All threads are used by default.

### sortPools
Optional algorithm parameter.
This parameter accepts the values _true_ or _false_ (the default is _false_). If this parameter is set to _true_ then the best pool will be chosen by least ping (not by the pool list).

## Configuration File
The minimum configuration file for Ethereum may contain only a wallet:
```
wallet=<wallet>
```
**nanominer** will automatically use Ethereum pools.

To work with Ethereum Classic, the coin must be specified:
```
wallet=<wallet>
coin=ETC
```
In this case **nanominer** will use pools corresponding to Ethereum Classic.

## IMPORTANT!
For coins that run on the Ethash algorithm but are not supported by [nanopool.org](https://nanopool.org/), **you must** specify a **wallet** and pools (**pool1...**). In this case **nanominer** will function as if it were working with Ethereum,  but the results of its operations will be determined by tasks from the corresponding pools specified in the configuration file.

## Examples of Configuration Files
Example of a configuration file for Ethereum and PascalCoin:
```
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = eth-eu1.nanopool.org:9999
pool2 = eth-eu2.nanopool.org:9999
pool3 = eth-us-east1.nanopool.org:9999
pool4 = eth-us-west1.nanopool.org:9999
pool5 = eth-asia1.nanopool.org:9999
pool6 = eth-jp1.nanopool.org:9999
pool7 = eth-au1.nanopool.org:9999
[RandomHash]
wallet = 123456-77
paymentId = ffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = pasc-eu1.nanopool.org:15556
pool2 = pasc-eu2.nanopool.org:15556
pool3 = pasc-us-east1.nanopool.org:15556
pool4 = pasc-us-west1.nanopool.org:15556
pool5 = pasc-asia1.nanopool.org:15556
```
Example of a configuration file for Ethereum:
```
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = eth-eu1.nanopool.org:9999
pool2 = eth-eu2.nanopool.org:9999
pool3 = eth-us-east1.nanopool.org:9999
pool4 = eth-us-west1.nanopool.org:9999
pool5 = eth-asia1.nanopool.org:9999
pool6 = eth-jp1.nanopool.org:9999
pool7 = eth-au1.nanopool.org:9999
```
Example of an equivalent file for Ethereum:
```
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
```
Example of a minimum file for Ethereum:
```
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
```
Example of a configuration file for Ethereum Classic:
```
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
coin=Etc
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
```
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
coin=Etc
rigName = rig1
email = someemail@org
```
Example of a minimum file for Ethereum Classic:
```
[Ethash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
coin=Etc
```
Example of a configuration file for Ubiq:
```
[Ubqhash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
coin=Ubq
rigName = rig1
email = someemail@org
pool1 = ubiq-eu.maxhash.org:8008
pool2 = ubiq-us.maxhash.org:8008
pool3 = ubiq-as.maxhash.org:8008
```
Example of a minimum file for Ubiq:
```
[Ubqhash]
wallet=0xffffffffffffffffffffffffffffffffffffffff
pool1 = ubiq-eu.maxhash.org:8008
```
Example of a complete file for Monero:
```
[CryptoNightv8]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
paymentId = ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = xmr-eu1.nanopool.org:14433
pool2 = xmr-eu2.nanopool.org:14433
pool3 = xmr-us-east1.nanopool.org:14433
pool4 = xmr-us-west1.nanopool.org:14433
pool5 = xmr-asia1.nanopool.org:14433
```
Example of an equivalent file for Monero:
```
[CryptoNightv8]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
paymentId = ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
rigName = rig1
email = someemail@org
```
Example of a minimum file for Monero:
```
[CryptoNightv8]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
```
Example of a complete file for PascalCoin:
```
[RandomHash]
wallet = 123456-77
paymentId = ffffffffffffffff
rigName = rig1
email = someemail@org
pool1 = pasc-eu1.nanopool.org:15556
pool2 = pasc-eu2.nanopool.org:15556
pool3 = pasc-us-east1.nanopool.org:15556
pool4 = pasc-us-west1.nanopool.org:15556
pool5 = pasc-asia1.nanopool.org:15556
```
Example of an equivalent file for PascalCoin:
```
[RandomHash]
wallet = 123456-77
paymentId = ffffffffffffffff
rigName = rig1
email = someemail@org
```
Example of a minimum file for PascalCoin:
```
[RandomHash]
wallet = 123456-77
```


To mine PascalCoin in a solo mode please provide ip and port of Pascal Coin Wallet software. The wallet number filled in config does not matter in such case. Block payload would be "Miner Name" set up in Pascal Coin Wallet followed by nanominer version. Example of a file for solo mining PascalCoin using local wallet software:
```
wallet = 0
pool1 = 127.0.0.1:4009
```

Example of configuration file for mining Ethereum, Monero, Ubiq and PascalCoin on same 8 GPUs rig using separate devices:

```
rigName = rig1
[Ethash]
wallet = 0xffffffffffffffffffffffffffffffffffffffff
devices = 0,1
[CryptoNightv8]
wallet = fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
paymentId = ffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff
devices = 5
[Ubqhash]
wallet = 0x1111111111111111111111111111111111111111
pool1 = ubiq-eu.maxhash.org:8008
devices = 2,3,4,6,7
[RandomHash]
wallet=123456-77
```
