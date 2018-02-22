The Mongoose OS command line tool for Raspberry Pi
=================================
## Rationalle
I wanted to see if mongoose os could be used from the raspberry pi. However its not as easy as on the website. First hurdle is that the mos tool has not been compiled for Raspberry Pi. I then found the build instructions on github for the mos-tool. Learned a bit about different architectures, installed a later version of Golang, and did get it to work. 
I have to say... mos is a bit slow on the rPi. But it does work!

## Building

Minimal required Go version is 1.8. The golang included with rpi is not a recent enough version.

Instructions based on this [go install on rpi](https://alexatnet.com/install-go-on-raspberry/) --dead link now 
```bash 
wget https://storage.googleapis.com/golang/go1.8.3.linux-armv6l.tar.gz
sudo tar -C /home/pi -xzf go1.8.3.linux-armv6l.tar.gz
export PATH=$PATH:/home/pi/go/bin
``` 

Other required tools can be installed on raspian as follows:

```bash
sudo apt install build-essential python python-git libftdi-dev
```

Make sure you have `GOPATH` set, and `PATH` should contain `$GOPATH/bin`.
It can be done by adding this to your `~/.bashrc`:

```bash
export GOPATH="$HOME/go"
export PATH="$PATH:$GOPATH/bin"
```

Install govendor:

```bash
go get github.com/kardianos/govendor
```

Now clone the `mos-tool` repository into the proper location and `cd` to it

```bash
git clone https://github.com/cesanta/mos-tool $GOPATH/src/cesanta.com
cd $GOPATH/src/cesanta.com
```

Fetch all vendored packages and save them under the `vendor` dir:

```bash
govendor sync -v
```

Now, `mos` tool can be built:

```bash
make -C mos install
```
It will produce the binary `$GOPATH/bin/mos`.

On the rpi with its limited memory, it may be neccessary to shut down any memory hungry apps (i.e chromium) or the compilation will fail with the following error. 

```bash
/home/pi/go/pkg/tool/linux_arm/link: running gcc failed: fork/exec /usr/bin/gcc: cannot allocate memory

Makefile:30: recipe for target 'install' failed
make: *** [install] Error 2
make: Leaving directory '/home/pi/go/src/cesanta.com/mos'
````
To check avaliable memory:
```
free -h
```

## Changelog

See [release notes for this repo](https://github.com/cesanta/mos-tool/releases).

Up to version 1.25, mos tool was located under the
[mongoose-os](https://github.com/cesanta/mongoose-os) repo, so its changelog
can be found in [mongoose-os release notes](https://github.com/cesanta/mongoose-os/releases).
