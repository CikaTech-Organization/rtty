# rtty([中文](/README_ZH.md))

[1]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=plastic
[2]: /LICENSE
[3]: https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=plastic
[4]: https://github.com/zhaojh329/rtty/pulls
[5]: https://img.shields.io/badge/Issues-welcome-brightgreen.svg?style=plastic
[6]: https://github.com/zhaojh329/rtty/issues/new
[7]: https://img.shields.io/badge/release-8.0.0-blue.svg?style=plastic
[8]: https://github.com/zhaojh329/rtty/releases
[9]: https://github.com/zhaojh329/rtty/workflows/build/badge.svg
[10]: https://img.shields.io/badge/Support%20rtty-Donate-blueviolet.svg
[11]: https://paypal.me/zjh329

[![license][1]][2]
[![PRs Welcome][3]][4]
[![Issue Welcome][5]][6]
[![Release Version][7]][8]
![Build Status][9]
![visitors](https://visitor-badge.laobi.icu/badge?page_id=zhaojh329.rtty)
[![Support rtty][10]][11]

[Xterm.js]: https://github.com/xtermjs/xterm.js
[libev]: http://software.schmorp.de/pkg/libev.html
[openssl]: https://github.com/openssl/openssl
[mbedtls(polarssl)]: https://github.com/ARMmbed/mbedtls
[CyaSSl(wolfssl)]: https://github.com/wolfSSL/wolfssl
[vue]: https://github.com/vuejs/vue
[server]: https://github.com/zhaojh329/rttys

![](https://raw.githubusercontent.com/zhaojh329/rtty/doc/diagram.png)
![](https://raw.githubusercontent.com/zhaojh329/rtty/doc/terminal.gif)
![](https://raw.githubusercontent.com/zhaojh329/rtty/doc/file.gif)
![](https://raw.githubusercontent.com/zhaojh329/rtty/doc/web.gif)

It is composed of a client and a [server]. The client is written in pure C. The [server] is written in go language
and the front-end is written in [Vue].

You can access your device's terminal from anywhere via the web. Differentiate your different device by device ID.

rtty is very suitable for remote maintenance your or your company's thousands of Linux devices deployed around
the world.

# Features
* The client is writen in C language, very small, suitable for embedded Linux
  - No SSL: rtty(32K) + libev(56K)
  - Support SSL: + libmbedtls(88K) + libmbedcrypto(241K) + libmbedx509(48k)
* Execute command remotely in a batch of devices 
* SSL support: openssl, mbedtls, CyaSSl(wolfssl)
* mTLS
* Very convenient to upload and download files
* Access different devices based on device ID
* Support HTTP Proxy - Access your device's Web
* Fully-featured terminal based on [Xterm.js]
* Simple to deployment and easy to use

# Dependencies of the Client side
* [libev] - A full-featured and high-performance event loop
* [mbedtls(polarssl)] or [CyaSSl(wolfssl)] or [openssl] - If you want to support SSL

# [Deploying the server side](https://github.com/zhaojh329/rttys)

# How to install rtty
## For Linux distribution
### Install Dependencies

    sudo apt install -y libev-dev libssl-dev      # Ubuntu, Debian
    sudo pacman -S --noconfirm libev openssl      # ArchLinux
    sudo yum install -y libev-devel openssl-devel # Centos

### Clone the code of rtty

    git clone --recursive https://github.com/zhaojh329/rtty.git

### Build

    cd rtty && mkdir build && cd build
    cmake .. && make install

## For Buildroot
Select rtty in menuconfig and compile it

    Target packages  --->
        Shell and utilities  --->
            [*] rtty

## [For OpenWRT](/OPENWRT.md)

## [For Other Embedded Linux Platform](/CROSS_COMPILE.md)

# Command-line Options

    Usage: rtty [option]
        -I, --id=string          Set an ID for the device(Maximum 63 bytes, valid
                                 character:letter, number, underline and short line)
        -h, --host=string        Server's host or ipaddr(Default is localhost)
        -p, --port=number        Server port(Default is 5912)
        -d, --description=string Add a description to the device(Maximum 126 bytes)
        -a                       Auto reconnect to the server
        -s                       SSL on
        -C, --cacert             CA certificate to verify peer against
        -x, --insecure           Allow insecure server connections when using SSL
        -c, --cert               Certificate file to use"
        -k, --key                Private key file to use"
        -D                       Run in the background
        -t, --token=string       Authorization token
        -f username              Skip a second login authentication. See man login(1) about the details
        -R                       Receive file
        -S file                  Send file
        -v, --verbose            verbose
        -V, --version            Show version
        --help                   Show usage

# How to run rtty
Replace the following parameters with your own parameters

    sudo rtty -I 'My-device-ID' -h 'your-server' -p 5912 -a -v -d 'My Device Description'

If your [rttys](https://github.com/zhaojh329/rttys) is configured with mTLS enabled (device key and certificate required),
add the following parameters(Replace the following with valid paths to your own)

    -k /etc/ssl/private/abc.pem -c /etc/ssl/certs/abc.pem

You can generate them e.g. via openssl tool
    openssl req -x509 -newkey ec -pkeyopt ec_paramgen_curve:secp521r1 -keyout /tmp/key.pem -out /tmp/cert.pem -days 18262 -nodes -subj "/C=CZ/O=Acme Inc./OU=ACME/CN=ACME-DEV-123"

If your rttys is configured with a token, add the following parameter(Replace the following token with your own)

    -t 34762d07637276694b938d23f10d7164

# Usage
Use your web browser to access your server: `http://your-server-host:5913`, then click the connection button

## connect devices with no web login required(you need to configure the device white list on the server)
http://your-server-host:5913/connect/devid1

http://your-server-host:5913/connect/devid2

## Transfer file
Transfer file from local to remote device

	rtty -R

Transfer file from remote device to the local

	rtty -S test.txt

## [Execute command remotely](/COMMAND.md)

# Contributing
If you would like to help making [rtty](https://github.com/zhaojh329/rtty) better,
see the [CONTRIBUTING.md](https://github.com/zhaojh329/rtty/blob/master/CONTRIBUTING.md) file.

