# DQT
## Overview
DQT (Deep Q-network Testing) is an automatic black-box Android GUI testing approach based on deep reinforcement learning and graph embedding. It leverages advanced graph embedding techniques for states and action encoding, employs a specially designed DQN for state-action evaluation, and conducts curiosity-driven exploration guided by a fine-grained dynamic reward function.

The artifact of DQT can be downloaded from [GoogleDriveShare](https://drive.google.com/drive/folders/1w0XQv8FooDnUDkMUCSKXn_ZtYnE8hy1v?usp=sharing).

<br/>

## Publication
More details about DQT can be achieved in the ICSE 2024 paper "[Deeply Reinforcing Android GUI Testing with Deep Reinforcement Learning](https://dl.acm.org/doi/10.1145/3597503.3623344)".
```
@inproceedings{DBLP:conf/icse/LanLLP00L24,
  author       = {Yuanhong Lan and
                  Yifei Lu and
                  Zhong Li and
                  Minxue Pan and
                  Wenhua Yang and
                  Tian Zhang and
                  Xuandong Li},
  title        = {Deeply Reinforcing Android {GUI} Testing with Deep Reinforcement Learning},
  booktitle    = {Proceedings of the 46th {IEEE/ACM} International Conference on Software
                  Engineering, {ICSE} 2024, Lisbon, Portugal, April 14-20, 2024},
  pages        = {71:1--71:13},
  publisher    = {{ACM}},
  year         = {2024},
  url          = {https://doi.org/10.1145/3597503.3623344},
  doi          = {10.1145/3597503.3623344},
  timestamp    = {Mon, 24 Jun 2024 15:20:25 +0200},
  biburl       = {https://dblp.org/rec/conf/icse/LanLLP00L24.bib},
  bibsource    = {dblp computer science bibliography, https://dblp.org}
}
```

<br/>

## File Structure
```
DQT
├── app  The directory for your APKs, an example MyExpenses-r554-debug.apk is provided.
├── main  The directory for the executable DQT.
├── result  The directory for storing runtime testing results.
└── venv  The Python virtual environment.
```

For directories `app` and `result`, specifying other paths for them in the config file is also feasible.<br />For directory `venv`, you need to build your own Python virtual environment.

<br/>

## Experimental Environment
Here is the experimental environment we have tested.
#### Operating System

- Ubuntu 18.04
- Also tested on Ubuntu 22.04
#### Python

- The project is based on Python 3.7.
   - except for module _system_event_trigger_ which only supports Python 2.7
- Make sure that Python 3.7 and Python 2.7 are all available in your Ubuntu
   - It is recommended to install Python 3.7 and Python 2.7 via apt command (e.g., use _ppa:deadsnakes/ppa_ source), thus `/usr/bin/python3.7` and `/usr/bin/python2.7` will be directly available.
- Build your own Python3.7 virtual environment for the project.
   - It is recommended to build it under `DQT/venv`.
   - You may use command `python3.7 -m venv venv`.
- You may use the provided Python2.7 for module _system_event_trigger_ directly.
   - While testing the tool on Ubuntu 22.04, this provided virtual environment works well.
   - You may build your own Python2.7 virtual environment to replace the provided one.
   - Anyway, make sure `DQT/main/system_event_trigger/venv/bin/python2.7` performs normally. You can check this by directly running it to enter the Python2.7 interactive mode.
- For these two venvs, in general terms, there is no need to install any extra packages, as the executable DQT already includes necessary python packages.
#### Deep Learning

- GPU Driver: 470.161.03 (to support CUDA)
- CUDA: 11.4 / 11.8
   - Remember to add it to the system PATH
   - Check by command: `nvcc -V`
- cuDNN: 8.2.4 (compatible with CUDA)
- Torch: 1.10.0+cu113 (has already been provided with the executable DQT)

_Note: Pay attention to the version compatibility between GPU Driver, CUDA, and cuDNN._
#### Android Environment

- Command Line Tool Used: ADB, AAPT, AAPT2
   - Make sure these tools are available on system variables to support command-line calls
   - It is recommended to add Android Supports such as _tools_, _platform-tools_, and _build-tools_ to the system path.
- You may check the availability of these tools by these commands: `adb version`, `aapt version`, `aapt2 version`

_Note: One convenient way to download these Android tools is via AndroidStudio._
#### Android Emulator

- Device: Google Pixel 2
- Resolution: 1080*1920
- Android Version: Android 9.0 (API Level 28)
- RAM: 4GB
- VM Heap: 2GB
- Internal Storage: 8GB
- SD Card: 1GB

<br/>

## Testing Config (config.yaml in `DQT/main/`)

- ALLOW_CLEAN: Allow the tool to clean app data during testing
   - Except for some special cases, this config is usually set to `true`.
   - Special cases need this config to be `false`, possible scenarios include: 
      - Logged-in apps (apps that have been logged in and you don't want to lose this status while testing)
      - Database conflicts (avoid duplicate database builds)
      - Plug-in missing (apps that have been equipped with plug-ins and you don't want to lose them while testing)
   - Among our 30 experimental apps, these 5 apps are recommended to set `false`: _Signal_ (login), _Tachiyomi_ (prevent plug-in loss), _K9Mail_ (login), _Conversations_ (login), _MoneyManagerEx_ (database problem)
   - Note that even if this is set to `false`, DQT will still clean app data at the very final phase of the test.
   - _**It is recommended to take a snapshot of the emulator to save the initial status before testing and start with the snapshot for every test**_
- APK_LOCATION: The absolute dir path of the APK.
- APK_NAME: The file name of the App Under Test (AUT). We have provided the app _MyExpenses_ as an example.
- DEVICE_ID: The device ID for testing (e.g., emulator-5554).
- RESULT_LOCATION: The absolute dir path for saving runtime testing results.
- TIME_LIMIT: The testing time in seconds.

<br/>

## Run DQT

1. Unzip DQT.zip and enter the directory `DQT`
2. Build the two Python virtual environments
3. Prepare an Android Device (e.g., an Android Emulator), use the command `adb devices` to check
4. Enter the Python3 virtual environment (e.g., `source ./venv/bin/activate`)
5. Edit the config file `DQT/main/config.yaml`
6. Run DQT via a command like `./main/main`

<br/>

## Reference Links

1. Python
   1. Python venv: [https://docs.python.org/3.7/tutorial/venv.html](https://docs.python.org/3.7/tutorial/venv.html)
2. Deep Learning
   1. GPU Driver and CUDA: [https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)
   2. CUDA and cuDNN: [https://developer.nvidia.com/rdp/cudnn-archive](https://developer.nvidia.com/rdp/cudnn-archive)
3. Android
   1. Logcat: [https://developer.android.com/studio/command-line/logcat](https://developer.android.com/studio/command-line/logcat)
   2. Emulator: [https://developer.android.com/studio/run/emulator](https://developer.android.com/studio/run/emulator)
   3. Emulator(cmd): [https://developer.android.com/studio/run/emulator-commandline](https://developer.android.com/studio/run/emulator-commandline)
   4. ADB: [https://developer.android.com/studio/command-line/adb](https://developer.android.com/studio/command-line/adb)
   5. AAPT2: [https://developer.android.com/studio/command-line/aapt2](https://developer.android.com/studio/command-line/aapt2)

