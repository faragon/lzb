LZB: LZ77 data compression and other utilities in pure Bash language
===

LZB is a data compression/decompression utility written in Bash language,
using no external programs. It also includes additional utilities:

- CRC-32
- hexadecimal encoding/decoding
- base64 encoding/decoding
- "head" and "cut" commands

Key points:

* Small: 13KB (including comments)
* Efficient: e.g. 1111111111 is reduced to 7 bytes. 20 x "1", to 7 bytes, too.
* Platform-independent: it works in any platform supported by Bash 4 or later
* Complete: it supports unlimited file size for compression and decompression
* Vintage: compress and decompress at 1981 speeds!

Compatibility
===

The requirement of Bash 4 or later is because of the usage of associative
arrays. I.e. this should work in most Linux and Unix distributions released
after 2009 (I've tested it with the default 'bash' command in Ubuntu 18.04).

Overview
===

How to install, example:
```
sudo cp lzb /usr/local/bin # Installation
sudo rm /usr/local/bin/lzb # Uninstallation
```
Syntax:
```
        lzb [-d|-crc32|-hex|-dhex|-b64|-db64|-h]
```
Help:
```
        lzb -h
```
Compression example:
```
        lzb <input >input.lzb
```
Decompression example:
```
        lzb -d <input.lzb >output
```
Compression and decompression cycle example:
```
        lzb <input | lzb -d >output
	diff input output
```
CRC-32:
```
        lzb -crc32 <input
```
Hexadecimal encoding:
```
        lzb -hex <input >output
```
Hexadecimal decoding:
```
        lzb -dhex <input >output
```
Base64 encoding:
```
        lzb -b64 <input >output
```
Base64 decoding:
```
        lzb -db64 <input >output
```
'head' command (get first N bytes):
```
        lzb -head N <input >output
```
'cut' command (skip first N bytes, and take next M bytes):
```
        lzb -cut N M <input >output
```

Internals
===

LZB is a rewrite in Bash of the libsrt's library 'enc' example, not
being compatible with its format, because of simplicity (the Bash version
is hundred thousands times slower than the C version, and probably million
times slower as the buffer size is increased). E.g. libsrt library 'enc'
example has a infinite-size dictionary ("-ezh" flag) which is not supported
by this program, nor the LUT tuning for speed, etc.

[libsrt repository](https://github.com/faragon/libsrt)

[libsrt LZ77 implementation](https://github.com/faragon/libsrt/blob/master/src/saux/senc.c)

Editing the 'lzb' file you can change the LZ_BUF_SIZE variable value,
for changing the data compression buffer size. The buffer size is related
to the compression window: that means that the default 16384-byte buffer
allows to locate patterns in that context. Increasing that value makes
the compressor to reach more distant patterns, at the cost of execution
speed. You can also reduce that value for speed-up, e.g. reducing it to
4096 (or to 3333, it is not restricted to powers of two). I'll probably
add a parameter for selecting compression ratio, too.

Bash is not good handling binary data, but it is able to read and write it,
so for convenience, the data is converted on the fly from raw to hexadecimal,
doing all the compression/decompression in hexadecimal, having a
post-process step from hexadecimal to raw, for writing the output. Both
the pre-process and the post-process are done using pipes.

License
===

Copyright (c) 2019, F. Aragon. All rights reserved.
Released under the BSD 3-Clause License (see the LICENSE file included).

Contact
===

email: faragon.github (GMail account, add @gmail.com)
