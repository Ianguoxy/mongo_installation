# uninstall 

1. check the current version

        (base) ianguo@ianguo:~$ mongo -version
        MongoDB shell version: 2.4.9

2.uninstall mongo

        (base) ianguo@ianguo:~$ sudo apt-get --purge remove mongodb mongodb-clients mongodb-server
        正在读取软件包列表... 完成
        正在分析软件包的依赖关系树       
        正在读取状态信息... 完成       
        Package 'mongodb' is not installed, so not removed
        Package 'mongodb-server' is not installed, so not removed
        下列软件包是自动安装的并且现在不需要了：
          libboost-filesystem1.54.0 libboost-program-options1.54.0
          libboost-thread1.54.0 libgoogle-perftools4 libsnappy1 libtcmalloc-minimal4
          libunwind8
        Use 'apt-get autoremove' to remove them.
        下列软件包将被【卸载】：
          mongodb-clients*

# install

https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/




