
# DQT2
## Overview
DQT2 (Deep Q-network Testing, second generation) is a black-box Android GUI testing framework designed for modern mobile apps that are large-scale, complex, and rapidly evolving. It extends DQT by enabling systematic testing knowledge sharing across both just-in-time testing and continuous regression testing.

DQT2 achieves this through two complementary and mutually reinforcing mechanisms. During testing, it performs just-in-time knowledge sharing by generalizing learned experience across similar GUI states and actions using graph-embedding–based representations and a tailored DQN guided by a fine-grained dynamic reward. Across test runs and app versions, it further enables persistent knowledge sharing by continuously evolving labeled app graphs that preserve and reuse accumulated testing knowledge.

The artifact of DQT2 can be downloaded from [GoogleDrive-DQT2](https://drive.google.com/drive/folders/1kTXkHKnQQsn_md_oV9Yqd46yVZndRYmf?usp=sharing).


<br/>


## File Structure
```plain
DQT2
├── app  The directory for your APKs, an example MyExpenses-r802-debug.apk is provided.
├── _internal  The directory for the executable DQT2.
├── result  The directory for storing runtime testing results.
├── DQT2  The main execution entry point.
├── example_config.yaml  A sample configuration file.
└── example_run_log.txt  A sample real runtime tool log.
```

For directories `app` and `result`, specifying other paths for them in the config file is also feasible.


<br/>


## Experimental Environment
Here is the experimental environment we have tested.

#### Operating System
+ Ubuntu 22.04

#### Python
+ The project is based on Python 3.11.
    - except for module _system_event_trigger_ which only supports Python 2.7
+ Make sure that Python 3.11 and Python 2.7 are all available in your Ubuntu
    - It is recommended to install Python 3.11 and Python 2.7 via apt command (e.g., use _ppa:deadsnakes/ppa_ source), thus `/usr/bin/python3.11` and `/usr/bin/python2.7` will be directly available.
+ If you want to enable _system_event_trigger_. You may use the provided Python2.7 for module _system_event_trigger_ directly.
    - While testing the tool on Ubuntu 22.04, this provided virtual environment works well.
    - You may build your own Python2.7 virtual environment to replace the provided one.
    - Anyway, make sure `DQT2/_internal/system_event_trigger/venv/bin/python2.7` performs normally. You can check this by directly running it to enter the Python2.7 interactive mode.


#### Deep Learning
+ GPU Driver: 535.183.01 (to support CUDA)
+ CUDA: 12.2
+ cuDNN: 9.3.0 (compatible with CUDA)
+ Torch: 2.4.0 (has already been provided with the executable DQT2)

_Note: Pay attention to the version compatibility between GPU Driver, CUDA, and cuDNN._

#### Android Environment
+ Command Line Tool Used: ADB, AAPT, AAPT2
    - Make sure these tools are available on system variables to support command-line calls
    - It is recommended to add Android Supports such as _tools_, _platform-tools_, and _build-tools_ to the system path.
+ You may check the availability of these tools by these commands: `adb version`, `aapt version`, `aapt2 version`

_Note: One convenient way to download these Android tools is via AndroidStudio._

#### Tested Android Emulator
**Emulator 1**
+ Device: Google Pixel 2
+ Resolution: 1080*1920
+ Android Version: Android 9.0 (API Level 28)
+ RAM: 4GB
+ VM Heap: 2GB
+ Internal Storage: 8GB
+ SD Card: 1GB

**Emulator 2**
+ Device: Google Pixel 8
+ Resolution: 1080*2400
+ Android Version: Android 14.0 (API Level 34)
+ RAM: 8GB
+ VM Heap: 1GB
+ Internal Storage: 8GB
+ SD Card: 1GB


<br/>



## Testing Config (config.yaml, `DQT2/_internal/config.yaml`)
**Testing Basic**
- DEVICE_ID: The device ID for testing (e.g., emulator-5554).
- TIME_LIMIT: The testing time in seconds.
- USE_LOCAL_APK: if USE_LOCAL_APK is false, DQT2 will directly test the already installed app in the device, the PACKAGE_NAME should be provided; else, DQT will install the app first and then start testing, the APK_ABSOLUTE_LOCATION_DIR and APK_NAME should be provided.
- APK_ABSOLUTE_LOCATION_DIR: The absolute dir path of the APK.
- APK_NAME: The file name of the App Under Test (AUT).
- PACKAGE_NAME: The package name of the AUT.

**Testing Advanced**
- NEED_SYSTEM_EVENT: Automatically inject normal system events or not.
- NEED_SYSTEM_EVENT_TRIGGER: Enable the _system_event_trigger_ module to inject additional system events or not.
- NEED_PERIODICAL_NETWORK_CONTROL: Need automatically network control or not.

**Testing Data**
- RESULT_ABSOLUTE_LOCATION_DIR: The absolute dir path for saving runtime testing results.
- RESULT_SUBDIR_VERBOSE: If true, the subdir of the test will be `xx/{apk_name}/{device_id}/{time}/`, else `xx/{apk_name}/`.
- NEED_LOGCAT: Enable auto logcat collection or not.
- NEED_SCREENSHOT: Enable step-wise screenshot or not.
- NEED_STATE_PERSISTENCE: Enable additional state persistence or not.
- NEED_STEP_WIDGET_IDENTIFIER: Enable step widget indentifier record or not.

**App Data Clean**
- UNINSTALL_PREVIOUS_UIAUTOMATOR: Try to uninstall precious UIAutomator before each test or not.
- ALLOW_PERIODIC_CLEAN: Allow DQT2 to clean app data periodically or not.
- ALLOW_CLEAN_AT_END: Allow DQT2 to clean app data at the last phrase of the test or not.

_**It is recommended to take a snapshot of the emulator to save the initial status before testing and start with the snapshot for every test, especially when you want to enable automatic data clean.**_

**Continuous Testing**
- ENABLE_APP_GRAPH: Enable runtime app graph construction or not.
- FORCE_BUILD_NEW_GRAPH: Force to build new app graph for each test or not.
- ENABLE_CONTINUOUS_TESTING: Enable continuous testing enhanced by app models or not.
- BACKUP_OLD_GRAPH_MODEL: Backup old graph model when starting a new test or not.
- ENABLE_PRETRAINING: Enable pretraining in continuous testing or not.

**NATE Integration**
- ENABLE_NATE: Enable NATE for network control or not.
- ENABLE_PURE_RANDOM_INJECTION: Use pure random injection instead of RL-based algorithm for NATE or not.

<br/>



## Run DQT
1. Unzip DQT2.zip and enter the directory `DQT2`.
2. Prepare an Android Device (e.g., an Android Emulator), use the command `adb devices` to check.
3. Edit the config file `./_internal/config.yaml`.
4. Run DQT2 via a command like `./DQT2`


<br/>


## Additional Note
_DQT2 has been implemented in large enterprises._

_At this stage, due to the restrictions of commercial contracts, the full source code cannot be published._

_We apologize for the inconvenience and appreciate your understanding._
