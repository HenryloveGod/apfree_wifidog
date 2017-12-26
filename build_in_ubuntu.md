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

   other source "http://libevent.org/".

 * You can try   
   <pre><code> git clone  git@github.com:KunTengRom/kunteng_libevent2.git  </code></pre>
   Enter into the source path and run 
   <code>cmake . </code> or 
   <code>./configure </code> or <code>./autogen.sh </code> to build Makefile once exist ,
   Then run command <code> make </code> At last <code> sudo make install</code> 
  The "so library" files is in the path.
  
   <pre><code>
   ls -al libevent2-2.1.8/.libs/ 
   ....
   lrwxrwxrwx  1 root root     30 12月 15 16:26 libevent_pthreads.so -> libevent_pthreads-2.1.so.6.0.2
   lrwxrwxrwx  1 root root     21 12月 15 16:26 libevent.so -> libevent-2.1.so.6.0.2
   ....
   </code></pre>
   
  Copy the realfile to PATH :/usr/local/lib/
  
### Attension

  If apfree_wifidog occurs : <code> is_error function not found </code> ,that relate with json-c/json
  
<code>
   openwrt/build_dir/target-mipsel_24kec+dsp_musl-1.1.14/json-c-0.12
</code> 

  libhttpd api files : httpd.h httpd_priv.h , copy them to wifidog source path , that avoid errors.

###  Howto install libiptables-extensions
   The issue "/usr/bin/ld: 找不到 -liptext ", that means libiptext.so file not found .
   Here is what i have done.
   <pre><code>
   // build method:
   cp -r ~/openwrt/build_dir/target-mipsel_24kec+dsp_musl-1.1.14/linux-ramips_mt7620/iptables-1.4.21 ~/
   cd ~/iptables-1.4.21/
   ./configure
   sudo make install
   
   // you can try to find them (include "libiptext4.so libiptext6.so")  install now 
   
   dl:~/iptables-1.4.21$ 
   dl:~/iptables-1.4.21$ find ./ -name "libiptext.so"
   ./extensions/libiptext.so
   ./ipkg-ramips_24kec/libxtables/usr/lib/libiptext.so
   dl:~/iptables-1.4.21$ locate libiptext.so
   /lib/i386-linux-gnu/libiptext.so
   /usr/local/lib/libiptext.so
   </code></pre>
 

   
   
   
   
   
   
   
   
