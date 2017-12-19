
### First
  git clone source from repository,run configure and make.

My mac has this problem,while ubuntu will not.

<code>
/Volumes/linux/apfree_wifidog/libhttpd/api.c:257:40: error: use of undeclared identifier 'SOCK_CLOEXEC'
    sock = socket(AF_INET, SOCK_STREAM|SOCK_CLOEXEC, 0);
                                       ^
/Volumes/linux/apfree_wifidog/libhttpd/api.c:270:35: error: use of undeclared identifier 'TCP_QUICKACK'
    setsockopt(sock, IPPROTO_TCP, TCP_QUICKACK, (int[]) {1}, sizeof(int));

</code>

add these code to libhttpd/httpd.h
<code>
#ifndef SOCK_CLOEXEC
#define SOCK_CLOEXEC    02000000
#endif

#ifndef TCP_QUICKACK
#define TCP_QUICKACK 0
#endif
</code>

Remake again ,show error,told us to install libevent2
<code>
/Volumes/linux/apfree_wifidog/src/conf.c:53:10: fatal error: 'event2/dns.h' file not found
</code>

### how to install libevent2

Once you build openwrt or lede successfully, get to the directory "build_dir/target-mipsel_24kc_musl/"
