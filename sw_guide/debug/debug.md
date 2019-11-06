# debug工具

   * debugger
   * coredump


## debuggerd

后台服务程序“/usr/bin/debuggerd”监测其它程序异常，并提示异常发生时的函数名称。

  * 详细log信息： /tmp/tombstones/tombstone_00
  * 系统log文件： /var/log/messages

###  代码位置： external/debugger/
```
$ ls -F external/debugger/
Build.mk        debuggerd/  libbacktrace/  libunwind/  VERSION.md
CMakeLists.txt  include/    libdebugger/   syslog/     Version.txt
```

### 异常log示例
程序/usr/bin/fbbmp发生异常如下log，异常点函数为*update_bmp_logo_to_framebuffer()*。

* log文件： /var/log/messages

```
fbbmp: debugger.cpp: log_signal_summary(193) > Fatal signal 11 (SIGSEGV), code 1, fault addr 0x7c771d08 in tid 2433 (fbbmp)
fbbmp: debugger.cpp: socket_abstract_client(105) > name:android:debuggerd
debuggerd: debuggerd.cpp: handle_request(213) > handle_request(5)
debuggerd: debuggerd.cpp: read_request(143) > reading tid
debuggerd: debuggerd.cpp: handle_request(220) > BOOM: pid=2433 uid=0 gid=0 tid=2433
debuggerd: debuggerd.cpp: handle_request(269) > stopped -- continuing
debuggerd: debuggerd.cpp: handle_request(288) > stopped -- fatal signal
debuggerd: *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
debuggerd: tombstone.cpp: dump_thread_info(238) > pid: 2433, tid: 2433, name: fbbmp  >>> fbbmp <<<
debuggerd: pid: 2433, tid: 2433, name: fbbmp  >>> fbbmp <<<
debuggerd: signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x7c771d08
debuggerd:  zr 00000000  at 00000001  v0 05a02d00  v1 7c771d08
debuggerd:  a0 77513d2c  a1 00000081  a2 76fb0000  a3 76fb0000
debuggerd:  t0 76fb0640  t1 ff000000  t2 00000010  t3 76fb0000
debuggerd:  t4 7c771d08  t5 00000000  t6 00000640  t7 202c3233
debuggerd:  s0 00411e30  s1 000001e0  s2 00000320  s3 00030018
debuggerd:  s4 00000c80  s5 76d6f008  s6 004d0000  s7 00000002
debuggerd:  t8 00000001  t9 77401000  k0 fbad2a84  k1 00000000
debuggerd:  gp 77519e10  sp 7fb15c28  s8 77eac498  ra 00401374
debuggerd:  hi 00000000  lo 05a02d00 bva 7c771d08 epc 004013c8
debuggerd:  backtrace:
debuggerd:     #00 pc 000013c8  /usr/bin/fbbmp (update_bmp_logo_to_framebuffer+252)
debuggerd:  stack:
debuggerd:          7fb15be8  00000c80  
debuggerd:          7fb15bec  76d6f008  
debuggerd:          7fb15bf0  004d0000  
debuggerd:          7fb15bf4  00000002  
debuggerd:          7fb15bf8  00000000  
debuggerd:          7fb15bfc  00000000  
debuggerd:          7fb15c00  00000000  
debuggerd:          7fb15c04  00000000  
debuggerd:          7fb15c08  00411dc8  /usr/bin/fbbmp
debuggerd:          7fb15c0c  00411da0  /usr/bin/fbbmp
debuggerd:          7fb15c10  77519e10  /usr/lib/libdebugger.so
debuggerd:          7fb15c14  00416910  [heap]
debuggerd:          7fb15c18  7fb15c2c  [stack]
debuggerd:          7fb15c1c  76d6f008  
debuggerd:          7fb15c20  7fb15c94  [stack]
debuggerd:          7fb15c24   00401374  /usr/bin/fbbmp (update_bmp_logo_to_framebuffer+168)
debuggerd:     #00  7fb15c28  004d0000  
debuggerd:          7fb15c2c  00000320  
debuggerd:          7fb15c30  000001e0  
debuggerd:          7fb15c34  00030018  
debuggerd:          7fb15c38  00000c80  
debuggerd:          7fb15c3c  00240000  
debuggerd:          7fb15c40  00010008  
debuggerd:          7fb15c44  c5e40000  
debuggerd:          7fb15c48  77519e10  /usr/lib/libdebugger.so
debuggerd:          7fb15c4c  00001800  
debuggerd:          7fb15c50  00000000  
debuggerd:          7fb15c54  00410000  
debuggerd:          7fb15c58  00411e30  /usr/bin/fbbmp
debuggerd:          7fb15c5c  76d6f008  
debuggerd:          7fb15c60  004a8860  
debuggerd:          7fb15c64  7fb15f25  [stack]
debuggerd: Tombstone written to: /tmp/tombstones/tombstone_00
debuggerd: debuggerd.cpp: handle_request(320) > detaching
debuggerd: debuggerd.cpp: do_server(398) > waiting for connection
```

* 详细log信息： */tmp/tombstones/tombstone_00*

```
# cat /tmp/tombstones/tombstone_00 
*** *** *** *** *** *** *** *** *** *** *** *** *** *** *** ***
pid: 2433, tid: 2433, name: fbbmp  >>> fbbmp <<<
signal 11 (SIGSEGV), code 1 (SEGV_MAPERR), fault addr 0x7c771d08
 zr 00000000  at 00000001  v0 05a02d00  v1 7c771d08
 a0 77513d2c  a1 00000081  a2 76fb0000  a3 76fb0000
 t0 76fb0640  t1 ff000000  t2 00000010  t3 76fb0000
 t4 7c771d08  t5 00000000  t6 00000640  t7 202c3233
 s0 00411e30  s1 000001e0  s2 00000320  s3 00030018
 s4 00000c80  s5 76d6f008  s6 004d0000  s7 00000002
 t8 00000001  t9 77401000  k0 fbad2a84  k1 00000000
 gp 77519e10  sp 7fb15c28  s8 77eac498  ra 00401374
 hi 00000000  lo 05a02d00 bva 7c771d08 epc 004013c8

backtrace:
    #00 pc 000013c8  /usr/bin/fbbmp (update_bmp_logo_to_framebuffer+252)

stack:
         7fb15be8  00000c80  
         7fb15bec  76d6f008  
         7fb15bf0  004d0000  
         7fb15bf4  00000002  
         7fb15bf8  00000000  
         7fb15bfc  00000000  
         7fb15c00  00000000  
         7fb15c04  00000000  
         7fb15c08  00411dc8  /usr/bin/fbbmp
         7fb15c0c  00411da0  /usr/bin/fbbmp
         7fb15c10  77519e10  /usr/lib/libdebugger.so
         7fb15c14  00416910  [heap]
         7fb15c18  7fb15c2c  [stack]
         7fb15c1c  76d6f008  
         7fb15c20  7fb15c94  [stack]
         7fb15c24   00401374  /usr/bin/fbbmp (update_bmp_logo_to_framebuffer+168)
    #00  7fb15c28  004d0000  
         7fb15c2c  00000320  
         7fb15c30  000001e0  
         7fb15c34  00030018  
         7fb15c38  00000c80  
         7fb15c3c  00240000  
         7fb15c40  00010008  
         7fb15c44  c5e40000  
         7fb15c48  77519e10  /usr/lib/libdebugger.so
         7fb15c4c  00001800  
         7fb15c50  00000000  
         7fb15c54  00410000  
         7fb15c58  00411e30  /usr/bin/fbbmp
         7fb15c5c  76d6f008  
         7fb15c60  004a8860  
         7fb15c64  7fb15f25  [stack]

memory near v0:
    05a02ce0-------- -------- -------- --------  ................
    05a02cf0-------- -------- -------- --------  ................

code around ra:
    004013543c040040afb400100260382502203025 @..<....%8`.%0 .
    0040136400602825248419880c10070800609025 %(`....$....%.`.
    004013741a20002c723310028e0b012000127040 ,. ...3r ...@p..
    0040138400006825240a00103c09ff0000556021 %h.....$...<!`U.
    004013940180182501603025016e40211e400009 %...%0`.!@n...@.

memory map: (fault address prefixed with --->)
    00400000-401fff r-x     8192  /usr/bin/fbbmp
    00411000-411fff rw-     4096  /usr/bin/fbbmp
    00412000-436fff rw-   151552  [heap]
    76d5f000-76faffff rw-  2428928  
    76fb0000-77126fff rw-  1536000  /dev/fb0
    77127000-77136fff rw-    65536  
    77137000-7715dfff r-x   159744  /lib/libgcc_s.so.1
    7715e000-7716cfff ---    61440  /lib/libgcc_s.so.1
    7716d000-7716dfff rw-     4096  /lib/libgcc_s.so.1
    7716e000-771e6fff r-x   495616  /lib/libm-2.22.so
    771e7000-771f5fff ---    61440  /lib/libm-2.22.so
    771f6000-771f6fff r--     4096  /lib/libm-2.22.so
    771f7000-771f7fff rw-     4096  /lib/libm-2.22.so
    771f8000-77370fff r-x  1544192  /usr/lib/libstdc++.so.6.0.21
    77371000-77380fff ---    65536  /usr/lib/libstdc++.so.6.0.21
    77381000-7738bfff r--    45056  /usr/lib/libstdc++.so.6.0.21
    7738c000-7738efff rw-    12288  /usr/lib/libstdc++.so.6.0.21
    7738f000-77390fff rw-     8192  
    77391000-774fdfff r-x  1495040  /lib/libc-2.22.so
    774fe000-7750dfff ---    65536  /lib/libc-2.22.so
    7750e000-77510fff r--    12288  /lib/libc-2.22.so
    77511000-77513fff rw-    12288  /lib/libc-2.22.so
    77514000-77515fff rw-     8192  
    77516000-77517fff r-x     8192  /usr/lib/libdebugger.so
    77518000-77526fff ---    61440  /usr/lib/libdebugger.so
    77527000-77527fff rw-     4096  /usr/lib/libdebugger.so
    77528000-7754afff r-x   143360  /lib/ld-2.22.so
    77556000-77559fff rw-    16384  
    7755a000-7755afff r--     4096  /lib/ld-2.22.so
    7755b000-7755bfff rw-     4096  /lib/ld-2.22.so
--->Fault address falls at 7c771d08 between mapped regions
    7faf5000-7fb15fff rwx   135168  [stack]
    7fff7000-7fff7fff r-x     4096  [vdso]
Tombstone written to: /tmp/tombstones/tombstone_00
```
