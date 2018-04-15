
## MKL for .deb-based systems: An easy recipe

This post describes how to _easily_ install the 
[Intel Math Kernel Library (MKL)](https://software.intel.com/en-us/mkl?cid=sem43700010399172562&intel_term=%2Bintel%20%2Bmkl&gclid=Cj0KCQjwzcbWBRDmARIsAM6uChXqzD4ACUJqCiu3zRJKA9rkC31XOhm9lIkEYiwBITMR_8hJbIAExF8aAn_LEALw_wcB&gclsrc=aw.ds) on a Debian or Ubuntu system. 
Very good basic documentation is provided by Intel [at their
site](https://software.intel.com/en-us/articles/installing-intel-free-libs-and-python-apt-repo). The discussion here
is more narrow as it focusses just on the Math Kernel Library (MKL).

The `tl;dr` version:  Use [this script](script.sh) which contains the commands described here.  

### First Step: Set up apt

We download the GnuPG key first and add it to the keyring:

```sh
cd /tmp
wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB
```

To add all Intel products we would run first command, but here we focus just on the MKL.  The website above
lists other suboptions (TBB, DAAL, MPI, ...)

```sh
## all products:
#wget https://apt.repos.intel.com/setup/intelproducts.list -O /etc/apt/sources.list.d/intelproducts.list

## just MKL
sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list'
```

We then update our lists of what is available in the repositories.

```sh
apt-get update
```

<details>
As a personal aside, I still use the awesome `wajig` frontend to `dpkg`, `apt` and more by
[Graham Williams](https://togaware.com/about/graham-williams/) (of [rattle](https://cran.r-project.org/package=rattle) fame). 
Among other tricks, `wajig` keeps state and therefore "knows" what packages are new.  Here, we see _a lot_:

```sh
edd@rob:/tmp$ wajig update
Hit:1 http://us.archive.ubuntu.com/ubuntu artful InRelease
Ign:2 http://dl.google.com/linux/chrome/deb stable InRelease
Hit:3 http://us.archive.ubuntu.com/ubuntu artful-updates InRelease
Hit:4 https://download.docker.com/linux/ubuntu artful InRelease
Hit:5 http://us.archive.ubuntu.com/ubuntu artful-backports InRelease
Ign:6 https://cloud.r-project.org/bin/linux/ubuntu artful/ InRelease
Hit:7 https://cloud.r-project.org/bin/linux/ubuntu artful/ Release
Hit:8 http://security.ubuntu.com/ubuntu artful-security InRelease
Hit:9 https://apt.repos.intel.com/mkl all InRelease
Hit:10 http://dl.google.com/linux/chrome/deb stable Release
Hit:12 https://packagecloud.io/slacktechnologies/slack/debian jessie InRelease
Reading package lists... Done
This is 367 up on the previous count with 367 new packages.
edd@rob:/tmp$ wajig new
Package                  Description
========================-===================================================
intel-mkl-gnu-f-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-gnu-f-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-cluster-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-gnu-c-196      Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-cluster-c-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-f-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-cluster-rt-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-gnu-239        Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-openmp-l-ps-libs-32bit-jp-174 OpenMP for Intel(R) Compilers 17.0 Update 2 for Linux*
intel-mkl-doc-ps-2018    Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-pgi-rt-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-ss-tbb-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-mic-cluster-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-openmp-l-ps-libs-jp-196 OpenMP for Intel(R) Compilers 17.0 Update 4 for Linux*
intel-mkl-ps-mic-rt-174  Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-openmp-l-ps-libs-jp-239 OpenMP for Intel(R) Compilers 17.0 Update 5 for Linux*
intel-mkl-common-f-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-f95-mic-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-common-64bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-f95-common-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-psxe-common-2018.2-046 Intel(R) Parallel Studio XE 2018 Update 2 for Linux*
intel-mkl-ps-mic-cluster-rt-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-cluster-64bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-f95-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-cluster-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-gnu-rt-196     Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-tbb-libs-2018.0-128 Intel(R) Threading Building Blocks 2018 for Linux*
intel-comp-l-all-vars-196 Intel(R) Compilers 17.0 Update 4 for Linux*
intel-mkl-common-ps-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-pgi-f-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-f95-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-gnu-rt-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-openmp-18.0.0-128  OpenMP for Intel(R) Compilers 18.0 for Linux*
intel-mkl-common-c-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-ps-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-f95-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-f95-mic-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-common-c-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-cluster-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-doc-f-jp    Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-f-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-32bit-2018.1-038 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-common-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-openmp-l-all-196   OpenMP for Intel(R) Compilers 17.0 Update 4 for Linux*
intel-mkl-pgi-rt-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-common-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-comp-nomcu-vars-18.0.0-128 Intel(R) Compilers 18.0 for Linux*
intel-mkl-common-c-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-f-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-common-c-64bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-32bit-174  Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-common-ps-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-cluster-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-gnu-f-rt-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-cluster-f-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-common-c-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-ss-tbb-196  Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-tbb-libs-32bit-2018.0-128 Intel(R) Threading Building Blocks 2018 for Linux*
intel-mkl-gnu-c-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-tbb-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-tbb-libs-2018.1-163 Intel(R) Threading Building Blocks 2018 Update 1 for Linux*
intel-mkl-ps-common-f-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-ss-tbb-rt-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-196            Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-pgi-196     Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-psxe-2018.2-046 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-doc-c          Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-f95-196     Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-cluster-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-tbb-libs-174       Intel(R) Threading Building Blocks 2017 Update 4 for Linux*
intel-comp-l-all-vars-174 Intel(R) Compilers 17.0 Update 2 for Linux*
intel-mkl-gnu-f-rt-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-gnu-rt-174     Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-openmp-l-ps-libs-32bit-jp-196 OpenMP for Intel(R) Compilers 17.0 Update 4 for Linux*
intel-mkl-gnu-f-rt-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-openmp-18.0.1-163  OpenMP for Intel(R) Compilers 18.0 Update 1 for Linux*
intel-mkl-ps-cluster-64bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-c-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-pgi-rt-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-2018.2-046     Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-gnu-rt-239     Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-rt-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-comp-l-all-vars-18.0.0-128 Intel(R) Compilers 18.0 for Linux*
intel-mkl-ps-common-jp-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-openmp-32bit-18.0.0-128 OpenMP for Intel(R) Compilers 18.0 for Linux*
intel-mkl-f95-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-core-ps-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-gnu-f-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-tbb-mic-rt-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-psxe-2018.1-038 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-gnu-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-mic-174     Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-64bit-2017.4-061 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-f95-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-mic-rt-jp-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-196        Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-psxe-common-doc-2018 Intel(R) Parallel Studio XE 2018 Update 2 for Linux*
intel-mkl-ps-tbb-mic-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-c-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-core-c-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-cluster-rt-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-rt-jp-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-core-c-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-ss-tbb-rt-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-core-f-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-psxe-050       Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-64bit-2018.2-046 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-tbb-rt-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-ss-tbb-rt-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-doc-f       Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-c-174      Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-f95-common-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-rt-jp-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-gnu-f-rt-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-pgi-f-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-tbb-libs-32bit-2018.1-163 Intel(R) Threading Building Blocks 2018 Update 1 for Linux*
intel-mkl-common-c-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-gnu-rt-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-64bit-2018.0-033 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-mic-c-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-ss-tbb-239  Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-64bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-openmp-32bit-18.0.2-199 OpenMP for Intel(R) Compilers 18.0 Update 2 for Linux*
intel-mkl-ps-rt-jp-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-f-rt-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-common-jp-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-tbb-mic-rt-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-psxe-common-061    Intel(R) Parallel Studio XE 2017 Update 5 for Linux*
intel-mkl-gnu-rt-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-common-f-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-mic-cluster-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-common-f-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-tbb-libs-196       Intel(R) Threading Building Blocks 2017 Update 6 for Linux*
intel-mkl-cluster-rt-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-cluster-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-pgi-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-ss-tbb-rt-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-openmp-l-all-174   OpenMP for Intel(R) Compilers 17.0 Update 2 for Linux*
intel-mkl-tbb-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-pgi-c-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-64bit-2018.1-038 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-f95-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-gnu-c-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-239            Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-pgi-rt-174  Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-cluster-f-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-f95-common-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-f-64bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-cluster-common-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-cluster-f-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-common-jp-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-openmp-l-all-32bit-196 OpenMP for Intel(R) Compilers 17.0 Update 4 for Linux*
intel-mkl-tbb-rt-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-psxe-common-056    Intel(R) Parallel Studio XE 2017 Update 4 for Linux*
intel-mkl-32bit-2018.0-033 Intel(R) Math Kernel Library 2018 for Linux*
intel-comp-l-all-vars-18.0.2-199 Intel(R) Compilers 18.0 Update 2 for Linux*
intel-mkl-common-ps-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-core-rt-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-pgi-c-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-common-c-ps-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-gnu-f-rt-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-f95-common-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-openmp-l-all-239   OpenMP for Intel(R) Compilers 17.0 Update 5 for Linux*
intel-mkl-174            Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-f-rt-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-tbb-libs-239       Intel(R) Threading Building Blocks 2017 Update 8 for Linux*
intel-mkl-common-f-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-common-f-64bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-cluster-common-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-comp-nomcu-vars-18.0.2-199 Intel(R) Compilers 18.0 Update 2 for Linux*
intel-mkl-32bit-196      Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-gnu-f-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-ps-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-common-196     Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-rt-174         Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-rt-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-common-f-64bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-cluster-rt-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-cluster-c-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-gnu-rt-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-doc         Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-rt-32bit-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-openmp-l-ps-libs-174 OpenMP for Intel(R) Compilers 17.0 Update 2 for Linux*
intel-mkl-ps-cluster-rt-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-common-64bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-c-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-pgi-c-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-gnu-f-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-pgi-f-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-f-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-core-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-common-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-pgi-c-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-rt-jp-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-common-c-64bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-gnu-f-rt-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-pgi-c-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-f-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-32bit-239      Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-rt-jp-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-mic-239     Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-tbb-libs-2018.2-199 Intel(R) Threading Building Blocks 2018 Update 2 for Linux*
intel-mkl-f95-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-openmp-l-ps-libs-239 OpenMP for Intel(R) Compilers 17.0 Update 5 for Linux*
intel-mkl-rt-239         Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-core-c-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-openmp-l-all-32bit-174 OpenMP for Intel(R) Compilers 17.0 Update 2 for Linux*
intel-mkl-ps-pgi-239     Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-f-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-gnu-f-rt-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-tbb-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-rt-32bit-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-mic-196     Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-gnu-f-rt-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-32bit-239  Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-174  Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-common-c-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-f-rt-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-f95-common-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-mic-f-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-196  Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-ps-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-cluster-64bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-64bit-2017.3-056 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-ss-tbb-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-32bit-2017.4-061 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-tbb-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-64bit-2017.2-050 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-mic-rt-196  Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-cluster-c-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-c-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-gnu-f-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-gnu-c-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-rt-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-core-rt-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-common-239     Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-2017.3-056     Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-tbb-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-pgi-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-pgi-f-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-common-c-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-tbb-rt-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-openmp-l-all-32bit-239 OpenMP for Intel(R) Compilers 17.0 Update 5 for Linux*
intel-mkl-rt-196         Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-cluster-f-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-2017.4-061     Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-common-c-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-c-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-psxe-common-doc    Intel(R) Parallel Studio XE 2017 Update 5 for Linux*
intel-tbb-libs-32bit-2018.2-199 Intel(R) Threading Building Blocks 2018 Update 2 for Linux*
intel-mkl-2017.2-050     Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-tbb-mic-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-2018.1-038     Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-gnu-c-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-ps-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-tbb-mic-rt-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-tbb-rt-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-cluster-f-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-psxe-061       Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-ss-tbb-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-mic-rt-jp-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-common-c-ps-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-doc-jp      Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-core-rt-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-common-f-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-c-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-cluster-rt-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-mic-rt-239  Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-openmp-l-ps-libs-196 OpenMP for Intel(R) Compilers 17.0 Update 4 for Linux*
intel-mkl-common-c-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-32bit-196  Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-openmp-18.0.2-199  OpenMP for Intel(R) Compilers 18.0 Update 2 for Linux*
intel-mkl-ps-common-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-rt-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-openmp-32bit-18.0.1-163 OpenMP for Intel(R) Compilers 18.0 Update 1 for Linux*
intel-mkl-ps-pgi-174     Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-comp-l-all-vars-239 Intel(R) Compilers 17.0 Update 5 for Linux*
intel-mkl-ps-mic-c-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-f-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-ss-tbb-174  Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-f-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-mic-c-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-mic-rt-jp-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-psxe-common-2018.0-033 Intel(R) Parallel Studio XE 2018 for Linux*
intel-mkl-ps-f95-mic-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-common-c-64bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-psxe-056       Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-rt-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-core-c-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-common-c-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-comp-l-all-vars-18.0.1-163 Intel(R) Compilers 18.0 Update 1 for Linux*
intel-mkl-psxe-2018.0-033 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-f95-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-openmp-l-ps-libs-jp-174 OpenMP for Intel(R) Compilers 17.0 Update 2 for Linux*
intel-mkl-tbb-rt-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-mic-f-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-2018.0-033     Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-ps-f95-239     Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-doc            Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-common-c-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-common-f-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-gnu-f-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-cluster-c-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-common-c-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-tbb-mic-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-sta-common-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-32bit-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-core-f-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-32bit-2018.2-046 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-mic-f-196   Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-c-239      Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-doc-c-jp    Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-rt-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-doc-2018       Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-pgi-c-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-core-rt-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-common-239  Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-f95-common-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-gnu-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-cluster-c-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-mic-cluster-rt-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-cluster-f-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-pgi-f-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-common-c-ps-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-cluster-common-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-32bit-2017.3-056 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-core-rt-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-gnu-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-eula-174       Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-ss-tbb-rt-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-pgi-rt-196  Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-gnu-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-psxe-common-2018.1-038 Intel(R) Parallel Studio XE 2018 Update 1 for Linux*
intel-mkl-pgi-f-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-core-ps-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-common-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-rt-jp-239   Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-ps-pgi-rt-239  Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-32bit-2017.2-050 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-f-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-pgi-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-tbb-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-comp-nomcu-vars-18.0.1-163 Intel(R) Compilers 18.0 Update 1 for Linux*
intel-mkl-common-f-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-tbb-rt-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-common-174     Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-gnu-c-32bit-2018.2-199 Intel(R) Math Kernel Library 2018 Update 2 for Linux*
intel-mkl-ps-mic-cluster-rt-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-f95-174     Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-core-2018.1-163 Intel(R) Math Kernel Library 2018 Update 1 for Linux*
intel-mkl-ps-gnu-f-32bit-196 Intel(R) Math Kernel Library 2017 Update 3 for Linux*
intel-mkl-ps-f95-32bit-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-ps-mic-cluster-174 Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-psxe-common-050    Intel(R) Parallel Studio XE 2017 Update 2 for Linux*
intel-mkl-cluster-c-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-rt-32bit-174   Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-mkl-32bit-174      Intel(R) Math Kernel Library 2017 Update 2 for Linux*
intel-openmp-l-ps-libs-32bit-jp-239 OpenMP for Intel(R) Compilers 17.0 Update 5 for Linux*
intel-mkl-ps-ss-tbb-rt-32bit-239 Intel(R) Math Kernel Library 2017 Update 4 for Linux*
intel-mkl-gnu-f-rt-32bit-2018.0-128 Intel(R) Math Kernel Library 2018 for Linux*
intel-mkl-gnu-174        Intel(R) Math Kernel Library 2017 Update 2 for Linux*
edd@rob:/tmp$
```
</details>

### Install MKL

Now that we have everything set up, installing the MKL is as simple as:

```sh
apt-get install intel-mkl-64bit-2018.2-046
```

This picks the 64-bit only variant of the (currently) most recent builds.      

There is a slight cost:  a 500mb download of 39 packages which install to 1.9 gb! 
Other than that it is easy: one command!   Compare that with the days of yore when
we fetched shar archives of NETLIB...


### Integrate MKL

One the key advantages of a Debian or Ubuntu system is the overall integration providing a raft of
useful features.  One of these is the seamless and automatic selection of alternatives.  By
declaring a particular set of BLAS and LAPACK libraries the default, _all_ application linked
against this interface will use the default.  Better still, users can switch between these as well.

So here we can make the MKL default for BLAS and LAPACK:

```sh
## update alternatives
update-alternatives --install /usr/lib/x86_64-linux-gnu/libblas.so     \
                    libblas.so-x86_64-linux-gnu      /opt/intel/mkl/lib/intel64/libmkl_rt.so 50
update-alternatives --install /usr/lib/x86_64-linux-gnu/libblas.so.3   \
                    libblas.so.3-x86_64-linux-gnu    /opt/intel/mkl/lib/intel64/libmkl_rt.so 50
update-alternatives --install /usr/lib/x86_64-linux-gnu/liblapack.so   \
                    liblapack.so-x86_64-linux-gnu    /opt/intel/mkl/lib/intel64/libmkl_rt.so 50
update-alternatives --install /usr/lib/x86_64-linux-gnu/liblapack.so.3 \
                    liblapack.so.3-x86_64-linux-gnu  /opt/intel/mkl/lib/intel64/libmkl_rt.so 50
```

Next, we have to tell the dyanmic linker about two directories use by the MKL, and have it update
its cache:

```sh
echo "/opt/intel/lib/intel64"     >  /etc/ld.so.conf.d/mkl.conf
echo "/opt/intel/mkl/lib/intel64" >> /etc/ld.so.conf.d/mkl.conf
ldconfig
```

### Use the MKL

Now the MKL is 'known' and the default. If we start R, its `sessionInfo()` shows the MKL:

```r
# Matrix products: default                            
# BLAS/LAPACK: /opt/intel/compilers_and_libraries_2018.2.199/linux/mkl/lib/intel64_lin/libmkl_rt.so        
```

### Benchmarks


```r
# Vanilla r-base Rocker with default reference BLAS 
> n <- 1e3 ; X <- matrix(rnorm(n*n),n,n);  system.time(svd(X)) 
   user  system elapsed 
  2.239   0.004   2.266 
> 

# OpenBlas added to r-base Rocker
>  n <- 1e3 ; X <- matrix(rnorm(n*n),n,n);  system.time(svd(X)) 
   user  system elapsed 
  1.367   2.297   0.353 
> 

# MKL added to r-base Rocker
> n <- 1e3 ; X <- matrix(rnorm(n*n),n,n)  
> system.time(svd(X))                               
   user  system elapsed                             
  1.772   0.056   0.350                             
>  
```

So just R (with reference BLAS) is slow. (Using Docker is done here to have
clean comparisons while not altering the outer host system; impact of running
Docker on Linux should be minimal.) Adding OpenBLAS helps quite a bit already
by offering multi-core processing -- the, and MKL does not yet improve
materially over OpenBLAS.  Now, this of course was not any serious
benchmarking---we just ran one SVD. More to do as time permits...

### Removal, if needed

Another rather nice benefit of the package management is that clean removal is also possible:

```sh
root@c9f8062fbd93:/tmp# apt-get autoremove intel-mkl-64bit-2018.2-046
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following packages will be REMOVED:
  intel-comp-l-all-vars-18.0.2-199 intel-comp-nomcu-vars-18.0.2-199 intel-mkl-64bit-2018.2-046 
  intel-mkl-cluster-2018.2-199 intel-mkl-cluster-c-2018.2-199 intel-mkl-cluster-common-2018.2-199 
  intel-mkl-cluster-f-2018.2-199 intel-mkl-cluster-rt-2018.2-199 intel-mkl-common-2018.2-199 
  intel-mkl-common-c-2018.2-199 intel-mkl-common-c-ps-2018.2-199 intel-mkl-common-f-2018.2-199 
  intel-mkl-common-ps-2018.2-199 intel-mkl-core-2018.2-199 intel-mkl-core-c-2018.2-199 
  intel-mkl-core-f-2018.2-199 intel-mkl-core-ps-2018.2-199 intel-mkl-core-rt-2018.2-199 
  intel-mkl-doc-2018 intel-mkl-doc-ps-2018 intel-mkl-f95-2018.2-199 intel-mkl-f95-common-2018.2-199 
  intel-mkl-gnu-2018.2-199 intel-mkl-gnu-c-2018.2-199 intel-mkl-gnu-f-2018.2-199 intel-mkl-gnu-f-rt-2018.2-199 
  intel-mkl-gnu-rt-2018.2-199 intel-mkl-pgi-2018.2-199 intel-mkl-pgi-c-2018.2-199 intel-mkl-pgi-f-2018.2-199 
  intel-mkl-pgi-rt-2018.2-199 intel-mkl-psxe-2018.2-046 intel-mkl-tbb-2018.2-199 intel-mkl-tbb-rt-2018.2-199 
  intel-openmp-18.0.2-199 intel-psxe-common-2018.2-046 intel-psxe-common-doc-2018 intel-tbb-libs-2018.2-199 
  intel-tbb-libs-32bit-2018.2-199 libisl15
0 upgraded, 0 newly installed, 40 to remove and 0 not upgraded.
After this operation, 1,904 kB disk space will be freed.
Do you want to continue? [Y/n] n                    
Abort.                                              
root@c9f8062fbd93:/tmp#  
```

where we said 'no' just to illustrate the option.

### Summary

Package management systems are fabulous.  Kudos to Intel for supporting `apt` (and also `yum` in case you are on an rpm-based system).
We can install the MKL with just a few commands (which we regrouped in [this script](script.sh)).   

The MKL has a serious footprint with an installed size of just under 2gb.  But for those doing extended amounts of numerical analysis, 
installing this library may well be worth it.
