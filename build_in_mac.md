not work 

libmnl linux only



### First

git clone source from repository,run configure and make.
My mac has this problem,while ubuntu will not.

<pre><code>
/Volumes/linux/apfree_wifidog/libhttpd/api.c:257:40: error: use of undeclared identifier 'SOCK_CLOEXEC'
    sock = socket(AF_INET, SOCK_STREAM|SOCK_CLOEXEC, 0);
                                       ^
/Volumes/linux/apfree_wifidog/libhttpd/api.c:270:35: error: use of undeclared identifier 'TCP_QUICKACK'
    setsockopt(sock, IPPROTO_TCP, TCP_QUICKACK, (int[]) {1}, sizeof(int));
</pre></code>

add these code to libhttpd/httpd.h

<pre><code>
#ifndef SOCK_CLOEXEC
#define SOCK_CLOEXEC    02000000
#endif

#ifndef TCP_QUICKACK
#define TCP_QUICKACK 0
#endif
</pre></code>

Remake again ,show error,told us to install libevent2

<pre><code>
/Volumes/linux/apfree_wifidog/src/conf.c:53:10: fatal error: 'event2/dns.h' file not found
</pre></code>


### Tracking 

> try to run cmake .

<pre><code>
localhost:apfree_wifidog leideng$ cmake .
-- Configuring done
CMake Warning (dev):
  Policy CMP0042 is not set: MACOSX_RPATH is enabled by default.  Run "cmake
  --help-policy CMP0042" for policy details.  Use the cmake_policy command to
  set the policy and suppress this warning.

  MACOSX_RPATH is not specified for the following targets:

   httpd

This warning is for project developers.  Use -Wno-dev to suppress it.


</pre></code>


### Install libhttpd

>  The library httpd must be installed first.
  
<pre><code>
localhost:apfree_wifidog leideng$ cd libhttpd/
localhost:libhttpd leideng$ 
localhost:libhttpd leideng$ ls
CMakeFiles		Makefile		cmake_install.cmake	httpd_priv.h		libhttpd.dylib		version.c
CMakeLists.txt		api.c			httpd.h			ip_acl.c		protocol.c
localhost:libhttpd leideng$ cmake .
-- The C compiler identification is AppleClang 9.0.0.9000039
-- The CXX compiler identification is AppleClang 9.0.0.9000039
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc
-- Check for working C compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++
-- Check for working CXX compiler: /Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
CMake Error at CMakeLists.txt:10 (install):
  install TARGETS given no LIBRARY DESTINATION for shared library target
  "httpd".


CMake Warning (dev) in CMakeLists.txt:
  No cmake_minimum_required command is present.  A line of code such as

    cmake_minimum_required(VERSION 3.10)

  should be added at the top of the file.  The version specified may be lower
  if you wish to support older CMake versions for this project.  For more
  information run "cmake --help-policy CMP0000".
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring incomplete, errors occurred!
See also "/Volumes/linux/apfree_wifidog/libhttpd/CMakeFiles/CMakeOutput.log".
localhost:libhttpd leideng$ 
localhost:libhttpd leideng$ make
-- Configuring done
CMake Warning (dev):
  Policy CMP0042 is not set: MACOSX_RPATH is enabled by default.  Run "cmake
  --help-policy CMP0042" for policy details.  Use the cmake_policy command to
  set the policy and suppress this warning.

  MACOSX_RPATH is not specified for the following targets:

   httpd

This warning is for project developers.  Use -Wno-dev to suppress it.

-- Generating done
-- Build files have been written to: /Volumes/linux/apfree_wifidog
Scanning dependencies of target httpd
[ 20%] Building C object libhttpd/CMakeFiles/httpd.dir/api.c.o
/Volumes/linux/apfree_wifidog/libhttpd/api.c:766:37: warning: passing 'const char *' to parameter of type 'char *' discards qualifiers
      [-Wincompatible-pointer-types-discards-qualifiers]
    _httpd_net_write(r->clientSock, msg, msg_len);
                                    ^~~
/Volumes/linux/apfree_wifidog/libhttpd/httpd_priv.h:70:51: note: passing argument to parameter here
    int _httpd_net_write __ANSI_PROTO((int, char *, int));
                                                  ^
1 warning generated.
[ 40%] Linking C shared library libhttpd.dylib
[100%] Built target httpd
localhost:libhttpd leideng$ sudo make install
Password:
[100%] Built target httpd
Install the project...
-- Install configuration: ""
-- Installing: /usr/local/lib/libhttpd.dylib
localhost:libhttpd leideng$ 


</pre></code>

libhttpd.dylib has been installed.

<pre></code>
localhost:apfree_wifidog leideng$ make
[ 12%] Built target httpd
[ 15%] Building C object src/CMakeFiles/wifidog.dir/conf.c.o
/Volumes/linux/apfree_wifidog/src/conf.c:53:10: fatal error: 'event2/dns.h' file not found
#include <event2/dns.h>
         ^~~~~~~~~~~~~~
1 error generated.
make[2]: *** [src/CMakeFiles/wifidog.dir/conf.c.o] Error 1
make[1]: *** [src/CMakeFiles/wifidog.dir/all] Error 2
make: *** [all] Error 2

</pre></code>

> Told us libevent2 must be installed.





### how to install libevent2

 Goto  http://libevent.org/ , then download libevent-2.1.8-stable.tar.gz .
 <pre><code>
 localhost:Downloads leideng$ tar zxvf libevent-2.1.8-stable.tar.gz 
 localhost:Downloads leideng$ cd libevent-2.1.8-stable
 localhost:libevent-2.1.8-stable leideng$ ./configure 
 localhost:libevent-2.1.8-stable leideng$ make
 bufferevent_openssl.c:66:10: fatal error: 'openssl/bio.h' file not found
////==========install openssl 
 localhost:libevent-2.1.8-stable leideng$ brew install openssl
Warning: openssl 1.0.2n is already installed
///========== just export them ,and run <code>configure</code> make again!
localhost:libevent-2.1.8-stable leideng$ export LDFLAGS=-L/usr/local/opt/openssl/lib
localhost:libevent-2.1.8-stable leideng$ export CPPFLAGS=-I/usr/local/opt/openssl/include
localhost:libevent-2.1.8-stable leideng$ ./configure 
localhost:libevent-2.1.8-stable leideng$ make

////======at last 
localhost:libevent-2.1.8-stable leideng$ sudo make install
</pre></code>


### INSTALL json

<pre><code>
localhost:apfree_wifidog leideng$ make
[ 12%] Built target httpd
[ 15%] Building C object src/CMakeFiles/wifidog.dir/conf.c.o
In file included from /Volumes/linux/apfree_wifidog/src/conf.c:64:
/Volumes/linux/apfree_wifidog/src/auth.h:31:10: fatal error: 'json-c/json.h' file not found
#include <json-c/json.h>
         ^~~~~~~~~~~~~~~
1 error generated.
make[2]: *** [src/CMakeFiles/wifidog.dir/conf.c.o] Error 1
make[1]: *** [src/CMakeFiles/wifidog.dir/all] Error 2
make: *** [all] Error 2
</pre></code>

> check where is the libjson-c . the file <code>~/openwrt/package/libs/libjson-c/Makefile</code> shows
<pre><code>
PKG_NAME:=json-c
PKG_VERSION:=0.12
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://s3.amazonaws.com/json-c_releases/releases/
</pre><code>

> while try , it works!
<pre><code>
localhost:apfree_wifidog leideng$ brew install json-c
</pre></code>

> run make again, happens

<pre><code>
localhost:src leideng$ make
/Volumes/linux/apfree_wifidog/src/simple_http.h:25:10: fatal error: 'openssl/ssl.h' file not found
 </pre></code>
 
> add this line to "/apfree_wifidog/CMakeLists.txt" 

</pre></code>
//add this line
set(CMAKE_C_FLAGS "-I/usr/local/opt/openssl/include -L/usr/local/opt/openssl/lib -g -o0")
 </pre></code>

> cmake . and make , error shows below, means libiptables needed.

<pre><code>
/Volumes/linux/apfree_wifidog/src/fw3_iptc.c:44:10: fatal error: 'xtables.h' file not found
</pre></code>

### Install Libiptables 
 
<pre><code>
localhost:linux leideng$ git clone git://git.netfilter.org/iptables
localhost:linux leideng$ cd iptables/
localhost:iptables leideng$ ./autogen.sh 
Can't exec "libtoolize": No such file or directory at /usr/local/share/autoconf/Autom4te/FileUtils.pm line 345, <GEN2> line 5.
autoreconf: failed to run libtoolize: No such file or directory
autoreconf: libtoolize is needed because this package uses Libtool
localhost:iptables leideng$ brew install libtool

//download iptables-1.6.1.tar.bz2  and tar -zxvf 

localhost:iptables-1.6.1 leideng$ ./configure 
*** Error: No suitable libmnl found. ***
    Please install the 'libmnl' package
    Or consider --disable-nftables to skip
    iptables-compat over nftables support.
localhost:iptables-1.6.1 leideng$ brew install libmnl
Error: No formulae found in taps.
//ok dowload libmnl-1.0.4.tar.bz2 from https://www.netfilter.org/projects/libmnl/downloads.html
localhost:libmnl-1.0.4 leideng$ ./configure 
configure: error: Linux only, dude!


-----crying !~~~~~~~~~~

</pre></code>

</pre></code>

