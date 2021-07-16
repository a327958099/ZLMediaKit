## 环境

```shell
编译机器：
centos 7.6
gcc version 5.4.0 (GCC)
cmake version 3.20.5
```

## 依赖准备

- openssl 安装

    ```shell
    $ wget https://www.openssl.org/source/openssl-1.1.1k.tar.gz
    $ tar -xvzf openssl-1.1.1k.tar.gz
    $ yum install -y zlib zlib-devel perl-CPAN
    $ ./config shared --openssldir=/usr/local/openssl --prefix=/usr/local/openssl
    $ make && make install
    $ echo "/usr/local/lib64/" >> /etc/ld.so.conf
    $ echo "/usr/local/openssl/lib" >> /etc/ld.so.conf
    $ ldconfig
    $ ln -s /usr/local/openssl/bin/openssl  /usr/local/bin/openssl # 替换系统openssl，非必须
    $ openssl version -a
    ```

- libsrtp安装

    点击[这里](https://codeload.github.com/cisco/libsrtp/tar.gz/refs/tags/v2.3.0)下载安装

    ```shell
    $ tar -xvzf libsrtp-2.3.0.tar.gz
    $ cd libsrtp-2.3.0
    $ ./configure --enable-openssl --with-openssl-dir=/usr/local/openssl
    $ make -j8 && make install
    ```

## 编译

- 下载zlm源码

    ```shell
    #国内用户推荐从同步镜像网站gitee下载 
    git clone --depth 1 https://gitee.com/xia-chu/ZLMediaKit
    cd ZLMediaKit
    #千万不要忘记执行这句命令
    git submodule update --init
    ```

- 编译

    ```shell
    $ mkdir build
    $ cd build
    $ cmake .. -DENABLE_WEBRTC=true  -DOPENSSL_ROOT_DIR=/usr/local/openssl  -DOPENSSL_LIBRARIES=/usr/local/openssl/lib
    $ cmake --build . --target MediaServer
    
    # 最终输出
    [ 96%] Built target test_rtcp_fci
    [ 96%] Building CXX object tests/CMakeFiles/test_rtp.dir/test_rtp.cpp.o
    [ 97%] Linking CXX executable ../../release/linux/Debug/test_rtp
    [ 97%] Built target test_rtp
    [ 97%] Building CXX object tests/CMakeFiles/test_wsServer.dir/test_wsServer.cpp.o
    [ 97%] Linking CXX executable ../../release/linux/Debug/test_wsServer
    [ 97%] Built target test_wsServer
    [ 97%] Building CXX object tests/CMakeFiles/test_server.dir/test_server.cpp.o
    [ 97%] Linking CXX executable ../../release/linux/Debug/test_server
    [ 97%] Built target test_server
    [ 98%] Built target jsoncpp
    [ 98%] Linking CXX executable ../../release/linux/Debug/MediaServer
    [100%] Built target MediaServer
    ```

## 问题解决

- 提示 `gmake[3]: *** No rule to make target `/usr/lib64/libssl.so', needed by `../release/linux/Debug/MediaServer'.  Stop.`

    ```
    cd /usr/local/openssl/lib
    cp -r ./* /usr/lib64/
    ```
- ubuntu编译
  可以参考网友大神自制[这里](https://blog.csdn.net/haysonzeng/article/details/116754065)
