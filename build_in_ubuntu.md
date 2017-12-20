# Build apfree_wifidog in ubuntu


* Apfree_wifidog depend on serial libraries that should be installed into ubuntu system.
>  libevent2
   json
   iptables-extensions
   libhttpd

### Howto install 

  libevent2 as example :

*  Ubuntu system may already have libevent2 ,use <code>locate libevent.so</code> or <code> whereis libevent.so </code> ,
  so you can remove them or change export library path .
  Then copy libevent2 sources to your ubuntu. I just copy libevent2 from my openwrt which I have Compiled and , path
<code> 
  [...]/openwrt/build_dir/target-mipsel_24kec+dsp_musl-1.1.14/libevent2-2.1.8
</code>

 * You can try   
 <code> git clone  git@github.com:KunTengRom/kunteng_libevent2.git  </code> 
 
  Enter into the source path and run 
  <code>cmake . </code> or 
  <code>./config </code> or <code>./autogen.sh </code> to build Makefile once exist ,
  then run command <code> make </code> At last <code> sudo make install</code> 
  The "so library" files is in the path 
  
  <code> 
ls -al libevent2-2.1.8/.libs/ 
  ....
lrwxrwxrwx  1 root root     30 12月 15 16:26 libevent_pthreads.so -> libevent_pthreads-2.1.so.6.0.2
lrwxrwxrwx  1 root root     21 12月 15 16:26 libevent.so -> libevent-2.1.so.6.0.2
....
  </code>
  Copy the realfile to PATH :/usr/local/lib/
  
### Attension

  If apfree_wifidog occurs : <code> is_error function not found </code> ,that relate with json-c/json
  
<code>
   openwrt/build_dir/target-mipsel_24kec+dsp_musl-1.1.14/json-c-0.12
</code> 

  libhttpd api files : httpd.h httpd_priv.h , copy them to wifidog source path , that avoid errors.

