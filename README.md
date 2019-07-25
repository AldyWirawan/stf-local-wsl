# Background
A lot of people want to deploy stf on his windows machine but it's quite troublesome because STF not support windows at all. Using docker will be quite problematic due to adb devices not easily synced over. Some people recommend WSL (windows subsystem for linux) but there's no official guide for it. This guide aims to cover the installation of stf local on WSL

I still recommend a proper installation on linux machine, just use windows WSL for POC only.

# Prerequisites
Install WSL using this guide: https://docs.microsoft.com/en-us/windows/wsl/install-win10 (choose ubuntu)

# Concept
We will use some of the STF requirements in Windows:
1. rethinkDB
2. adb

The rest of the STF requirements will be installed in WSL

# Steps

## On Windows

### RethinkDB
This one is fairly straightforward
1. download rethinkdb.exe (choose windows official package from https://www.rethinkdb.com/docs/install/)
2. put in some folder
3. open powershell, and run ./rethinkdb.exe
4. to specify the interface port, in case default 8080 is used, run ./rethinkdb.exe --http-port <desired port>
  
### ADB
Install adb in your windows, easiest way is by installing android studio. Make sure adb command is working on command prompt

## On Linux using WSL console

### Linux pkg requirement
run these command to install required package:
~~~
sudo apt-get update
sudo apt-get install nodejs npm graphicmagick yasm libtool pkg-config build-essential autoconf automake unzip g++ protobuf-compiler
~~~

### ZeroMQ
need to download the package, unzip, and use make to install ZeroMQ. Use these command:
~~~
wget https://github.com/zeromq/libzmq/releases/download/v4.3.2/zeromq-4.3.2.tar.gz
tar -zxvf zeromq-4.3.2.tar.gz
cd zeromq-4.3.2
./autogen.sh && ./configure && make -j 4 && sudo make check && sudo make install && sudo ldconfig
~~~

### STF
need to install STF via npm. install jpeg turbo first as pre-requisites of installing STF:
~~~
sudo npm install --save jpeg-turbo
sudo npm install -g stf
~~~

# Running STF

Several things to note before running stf:
1. make sure rethinkDB is on
2. make sure adb daemon is on (try adb devices in windows powershell)

After that, on WSL console, run:
~~~
stf local
~~~

you can add public ip to make it accessible by other network, just add --public-ip <desired ip> option
