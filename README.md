ccminer with mtp support
========================
djm34 2017-2018

donation addresses:

	BTC: 1NENYmxwZGHsKFmyjTc5WferTn5VTFb7Ze

	XZC: aChWVb8CpgajadpLmiwDZvZaKizQgHxfh5

Based on Christian Buchner's &amp; Christian H.'s CUDA project and tpruvot@github.


Building on windows
-------------------

Required: msvc2015 and cuda 9.x (cuda 9.2 prefered)
Dependencies for windows are included in compat directory, using a different version of msvc will most likely require to recompile those libraries.

In order to build ccminer, choose "Release" and "x64" (this version won't work with win32)
Then click "generate"

Building on Linux (tested on Ubuntu 16.04)
------------------------------------------

A developpement environnement is required together with curl, jansson and openssl


	* sudo apt-get update && sudo apt-get -y dist-upgrade
	* sudo apt-get -y install gcc g++ build-essential automake linux-headers-$(uname -r) git gawk libcurl4-openssl-dev libjansson-dev xorg libc++-dev libgmp-dev python-dev

	* Installing CUDA 9.2 and compatible drivers from nvidia website and not from ubuntu package is usually easier
	
	* Compiling ccminner:

	./autogen.sh
	./configure
	./make

Error list:
-------------------
1.libssl-dev error.

	"checking for SSL_library_init in -lssl... no
	configure: error: OpenSSL library required"


	Solution:(Copy from wiki)
	There are two ways to initialize the OpenSSL library, and they depend on the version of the library you are using. If you are using OpenSSL 1.0.2 or below, then you would use SSL_library_init. If you are using OpenSSL 1.1.0 or above, then you would use OPENSSL_init_ssl. 
	
	apt-get install libssl1.0-dev

2.Cuda install error.
	"dpkg: error processing archive /tmp/apt-dpkg-install-PypkDn/104-nvidia-396_396.26-0ubuntu1_amd64.deb (--unpack):
	 trying to overwrite '/usr/lib/x86_64-linux-gnu/libGLX_indirect.so.0', which is also in package libglx-mesa0:amd64 18.0.5-0ubuntu0~18.04.1"

	`sudo dpkg -i cuda-repo-ubuntu1710-9-2-local_9.2.88-1_amd64.deb`
	`sudo apt-key add /var/cuda-repo-<version>/7fa2af80.pub`
	`sudo apt-get update`
	`sudo apt-get install cuda`

	cd /var/cuda-repo-9-2-local
	dpkg -i --force-overwrite nvidia-396_396.26-0ubuntu1_amd64.deb
	rerun apt-get install cuda
	
3.Nvcc command not found.

	* find / -name nvcc
	* /usr/local/cuda-9.2/bin/nvcc
	* add the path to /etc/profile
	* source /etc/profile



About source code dependencies for windows
------------------------------------------

This project requires some libraries to be built :

- OpenSSL (prebuilt for win)

- Curl (prebuilt for win)

- pthreads (prebuilt for win)

The tree now contains recent prebuilt openssl and curl .lib for both x86 and x64 platforms (windows).

To rebuild them, you need to clone this repository and its submodules :
    git clone https://github.com/peters/curl-for-windows.git compat/curl-for-windows


Running ccminer with mtp and requirement
----------------------------------------

mtp requires 4Gb of vram, hence cards with less than 4.5Gb of vram won't work.
while running, ccminer will also use around 5.5Gb of ram. 
For the moment, ccminer, support only one vga per instance, to run of several gpus, it is then required to run one ccminer instance by gpu.

Instruction to mine on zcoin wallet (example)

command line structure

ccminer -a mtp -o  http://127.0.0.1:rpcport  -u rpcuser -p rpcpassword --coinbase-addr zcoin-address  --device card-number/name  --no-getwork --no-longpoll 

Example (RUN-ZCOIN-MTP.cmd)

ccminer -a mtp -o  http://127.0.0.1:8382  -u djm34 -p password --coinbase-addr aChWVb8CpgajadpLmiwDZvZaKizQgHxfh5 -d 1080  --no-getwork --no-longpoll


zcoin wallet should also be run with "server=1" option and "rpcport,rpcuser,rpcpassword" should match those of zcoin.conf


NB: For the moment, the intensity is not adjustable, this project is still in developpement, this will be changed in the near future









