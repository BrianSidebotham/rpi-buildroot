# RPi Buildroot

This repsitory shows how to use [Buidroot](https://buildroot.org/download.html) to build your own Linux Kernel based
OS for the Raspberry Pi. Build the entire OS on an x86 host - this takes around 10 minutes on my AMD ThreadRipper
12-core (24 hyperthreaded).

Up front - Buildroot doesn't include runtime package management - it's designed for building appliances really -
build a fixed, known working system from a configuration file. So if you're wanting to build an OS where security
patching is just a few commands away, Buildroot is not for you.

If you want to build a lightweight OS for the Raspberry Pi that performs a specific function, then Buildroot could
well be for you - but again - no security patching. You'd have to manage this yourself if you wanted to go that
route.

## Get Buildroot

Firstly, get the buildroot code: `git clone git://git.buildroot.net/buildroot`

## Default Build

Now we can go ahead and build one of the default raspberry pi Buildroot targets to test that it works OK.

Let's build a 64-bit OS for the Raspberry 3...

```
make raspberrypi3_64_defconfig
```

>**NOTE:** To see all options you can see what configurations exist `ls configs/ | grep raspberry`

The `make config` command above makes a configuration file suitable for building the target system and includes all of the options required to build a complete OS.

Build the OS:

```
make
```

>**NOTE:** Do not use the parallel make flag here because Buildroot takes care of that later on in the build process for you (based on the number of cores your machine has)

Drink Tea, etc.

## Write SD Card

When the build finishes (Takes about 10 minutes) - there will be an SD Card image under `output/images` called
`sdcard.img` - we write this to the SD Card and boot the RPi:

>**NOTE:** MAKE SURE YOU GET THE WRITE DEVICE, AS OTHERWISE YOU RISK TRASHING YOUR HOST MACHINE!

`cat output/images/sdcard.img > /dev/sda && sync && eject /dev/sda`

There you have it - a new OS that you built from scratch!
