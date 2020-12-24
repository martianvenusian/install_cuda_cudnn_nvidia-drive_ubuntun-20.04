# install_cuda_cudnn_nvidia-drive_ubuntun-20.04
These guidelines are about how to install cuda cudnn nvidia-drive on Ubuntu 20.04

### 1. Install update and upgrade your system:

```
$ sudo apt-get update
$ sudo apt-get upgrade
```


### 2. Before you start
Before you start please check version of libraries your are going to install.
For example:
 - TensorFlow 2.2.0
 - Python 3.5-3.8
 - GCC 7.3.1
 - CMake >= 3.12: https://cmake.org/download/ 
 - cuDNN library >= v7.6
 - CUDA toolkit >= v10.1
 
For tensorflow installation check here: https://www.tensorflow.org/install/source#gpu

### 3. Installing GCC on Ubuntu 20.04
CUDA 10.1 requires gcc v7.3.1.
Ubuntu 20.04 repositories provide GCC version 9.3.0
Verify gcc version by following command:

    `$ gcc --version`

If gcc is not installed then need to install gcc and g++.
Run the following command as root. The command installs a lot of packages, including gcc, g++ and make

    `$ sudo apt install build-essential`

OR

Install the desired GCC and G++ versions by typing:

    `$ sudo apt-get install gcc-7 g++-7 gcc-8 g++-8 gcc-9 g++-9 gcc-10 g++-10`

The commands below configures alternative for each version and associate a priority with it. The default version is the one with the highest priority, in our case that is gcc-10
    ```
    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100 --slave /usr/bin/g++ g++ /usr/bin/g++-10 --slave /usr/bin/gcov gcov /usr/bin/gcov-10
    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90 --slave /usr/bin/g++ g++ /usr/bin/g++-9 --slave /usr/bin/gcov gcov /usr/bin/gcov-9
    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-8 80 --slave /usr/bin/g++ g++ /usr/bin/g++-8 --slave /usr/bin/gcov gcov /usr/bin/gcov-8
    $ sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 70 --slave /usr/bin/g++ g++ /usr/bin/g++-7 --slave /usr/bin/gcov gcov /usr/bin/gcov-7
    ```

Later if you want to change the default version use the update-alternatives command:

    `sudo update-alternatives --config gcc`

```
    There are 4 choices for the alternative gcc (providing /usr/bin/gcc).

    Selection    Path             Priority   Status
    ------------------------------------------------------------
    0            /usr/bin/gcc-10   100       auto mode
    1            /usr/bin/gcc-10   100       manual mode
    2            /usr/bin/gcc-7    70        manual mode
    3            /usr/bin/gcc-8    80        manual mode
    * 4            /usr/bin/gcc-9    90        manual mode

    Press <enter> to keep the current choice[*], or type selection number: 2
    update-alternatives: using /usr/bin/gcc-7 to provide /usr/bin/gcc (gcc) in manual mode
```

Verify gcc version again:

    `$ gcc --version`

    ```
    gcc (Ubuntu 7.5.0-6ubuntu2) 7.5.0
    Copyright (C) 2017 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
    ```

### 4. Install latest NVIDIA drivers (GPU only).

Let’s go ahead and add the NVIDIA PPA repository to Ubuntu’s Aptitude package manager:

    ` $ sudo add-apt-repository ppa:graphics-drivers/ppa`
    ` $ sudo apt-get update`
    
Check nvidia drivers:

    `$ nvidia-smi`

    ```
    Command 'nvidia-smi' not found, but can be installed with:
    sudo apt install nvidia-340               # version 340.108-0ubuntu2, or
    sudo apt install nvidia-utils-435         # version 435.21-0ubuntu7
    sudo apt install nvidia-utils-390         # version 390.138-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-440         # version 440.100-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-450         # version 450.80.02-0ubuntu0.20.04.2
    sudo apt install nvidia-utils-450-server  # version 450.80.02-0ubuntu0.20.04.3
    sudo apt install nvidia-utils-418-server  # version 418.152.00-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-440-server  # version 440.95.01-0ubuntu0.20.04.1
    sudo apt install nvidia-utils-455         # version 455.38-0ubuntu0.20.04.1
    ```

Now we can very conveniently install our NVIDIA drivers

    `$ sudo apt install nvidia-driver-xxx`

    

Go ahead and reboot so that the drivers will be activated as your machine starts:

    `$ sudo reboot now`

You’ll want to verify that NVIDIA drivers have been successfully installed:

    `$ nvidia-smi`
    
    ```
    Thu Dec 24 17:02:34 2020       
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 455.45.01    Driver Version: 455.45.01    CUDA Version: 11.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  GeForce RTX 208...  Off  | 00000000:01:00.0  On |                  N/A |
    |  0%   51C    P8    19W / 250W |    857MiB /  7982MiB |     11%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
    |   1  GeForce RTX 208...  Off  | 00000000:03:00.0 Off |                  N/A |
    |  0%   48C    P8    12W / 250W |     10MiB /  7982MiB |      0%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
                                                                                
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |    0   N/A  N/A       991      G   /usr/lib/xorg/Xorg                231MiB |
    |    0   N/A  N/A      1721      G   /usr/lib/xorg/Xorg                308MiB |
    |    0   N/A  N/A      1888      G   /usr/bin/gnome-shell              195MiB |
    |    0   N/A  N/A      2724      G   ...AAAAAAAAA= --shared-files      107MiB |
    |    1   N/A  N/A       991      G   /usr/lib/xorg/Xorg                  4MiB |
    |    1   N/A  N/A      1721      G   /usr/lib/xorg/Xorg                  4MiB |
    |    1   N/A  N/A      1888      G   /usr/bin/gnome-shell                0MiB |
    |    1   N/A  N/A      2724      G   ...AAAAAAAAA= --shared-files        0MiB |
    +-----------------------------------------------------------------------------+
    ```


### 5. Install CUDA Toolkit and cuDNN (GPU only)

You can install CUDA 10.1 in Ubuntu 20.04 by running:

    `$ sudo apt install nvidia-cuda-toolkit`

After installing CUDA 10.1, run:
    `$ nvcc -V`
    
    ```
    nvcc: NVIDIA (R) Cuda compiler driver
    Copyright (c) 2005-2019 NVIDIA Corporation
    Built on Sun_Jul_28_19:07:16_PDT_2019
    Cuda compilation tools, release 10.1, V10.1.24
    ```


### 6. Install cuDNN (CUDA Deep Learning Neural Network library) (GPU only)

For this step, you will need to create an account on the NVIDIA website + download cuDNN.

Here’s the link: https://developer.nvidia.com/cudnn

When you’re logged in and on that page, go ahead and make the following selections:

- “Download cuDNN”
- Login and check “I agree to the terms of service fo the cuDNN Software License Agreement”
- “Archived cuDNN Releases” https://developer.nvidia.com/rdp/cudnn-archive
- “cuDNN v7.6.5 for CUDA 10.1”
- “cuDNN Library for Linux”

The following commands will install cuDNN in the proper locations on your Ubuntu 20.04 system:

    ```
    $ cd ~/Downloads/
    $ tar -xvzf cudnn-10.1-linux-x64-v7.6.5.32.tgz
    ```

Next, copy the extracted files to the CUDA installation folder:

    ```
    $ sudo cp cuda/include/cudnn.h /usr/lib/cuda/include/
    $ sudo cp cuda/lib64/libcudnn* /usr/lib/cuda/lib64/
    ```

Set the file permissions of cuDNN:

    ```
    $ sudo chmod a+r /usr/lib/cuda/include/cudnn.h /usr/lib/cuda/lib64/libcudnn*
    $ cd ~
    ```

Above, we have:

- Extracted the cuDNN 10.1 v7.6.5 file in our home directory.
- Copied the lib64/ directory and all of its contents to the path shown.
- Copied the include/ folder as well to the path shown.
- Take care with these commands as they can be a pain point later if cuDNN isn’t where it needs to be.

### 7. Export CUDA environment variables
We need to append them to ~/.bashrc file by running:

    `$ echo 'export LD_LIBRARY_PATH=/usr/lib/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc`
    `$ echo 'export LD_LIBRARY_PATH=/usr/lib/cuda/include:$LD_LIBRARY_PATH' >> ~/.bashrc`

Load the exported environment variables by running:

    `$ source ~/.bashrc`


### 8. Check our installation once more

Verify gcc version:

    `$ gcc --version`

Verify nvidia-driver version

    `$ nvidia-smi`

    ```
        Thu Dec 24 17:41:55 2020       
    +-----------------------------------------------------------------------------+
    | NVIDIA-SMI 455.45.01    Driver Version: 455.45.01    CUDA Version: 11.1     |
    |-------------------------------+----------------------+----------------------+
    | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
    | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
    |                               |                      |               MIG M. |
    |===============================+======================+======================|
    |   0  GeForce RTX 208...  Off  | 00000000:01:00.0  On |                  N/A |
    |  0%   51C    P8    18W / 250W |    779MiB /  7982MiB |      5%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
    |   1  GeForce RTX 208...  Off  | 00000000:03:00.0 Off |                  N/A |
    |  0%   37C    P8    12W / 250W |     10MiB /  7982MiB |      0%      Default |
    |                               |                      |                  N/A |
    +-------------------------------+----------------------+----------------------+
                                                                                
    +-----------------------------------------------------------------------------+
    | Processes:                                                                  |
    |  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
    |        ID   ID                                                   Usage      |
    |=============================================================================|
    |    0   N/A  N/A       991      G   /usr/lib/xorg/Xorg                231MiB |
    |    0   N/A  N/A      1721      G   /usr/lib/xorg/Xorg                330MiB |
    |    0   N/A  N/A      1888      G   /usr/bin/gnome-shell              101MiB |
    |    0   N/A  N/A      2724      G   ...AAAAAAAAA= --shared-files       98MiB |
    |    0   N/A  N/A      8984      G   /usr/lib/firefox/firefox            2MiB |
    |    1   N/A  N/A       991      G   /usr/lib/xorg/Xorg                  4MiB |
    |    1   N/A  N/A      1721      G   /usr/lib/xorg/Xorg                  4MiB |
    |    1   N/A  N/A      1888      G   /usr/bin/gnome-shell                0MiB |
    |    1   N/A  N/A      2724      G   ...AAAAAAAAA= --shared-files        0MiB |
    |    1   N/A  N/A      8984      G   /usr/lib/firefox/firefox            0MiB |
    +-----------------------------------------------------------------------------+
    ```
 
Verify cuda version

    `$ nvcc --version`
    
    ```
    nvcc: NVIDIA (R) Cuda compiler driver
    Copyright (c) 2005-2019 NVIDIA Corporation
    Built on Sun_Jul_28_19:07:16_PDT_2019
    Cuda compilation tools, release 10.1, V10.1.243
    ```
    OR
    
    `$ cat /usr/lib/cuda/version.txt`
    ```
    CUDA Version 10.1.243

    ```

Verify cudnn version:    

    `$ cat /usr/lib/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2`
        
    ```
    #define CUDNN_MAJOR 7
    #define CUDNN_MINOR 6
    #define CUDNN_PATCHLEVEL 5
    --
    #define CUDNN_VERSION (CUDNN_MAJOR * 1000 + CUDNN_MINOR * 100 + CUDNN_PATCHLEVEL)

    #include "driver_types.h"
    ```

I think the CREATOR


References:
https://towardsdatascience.com/installing-tensorflow-gpu-in-ubuntu-20-04-4ee3ca4cb75d
https://medium.com/analytics-vidhya/installing-tensorflow-with-cuda-cudnn-gpu-support-on-ubuntu-20-04-f6f67745750a
https://medium.com/@sb.jaduniv/how-to-install-opencv-4-2-0-with-cuda-10-1-on-ubuntu-20-04-lts-focal-fossa-bdc034109df3