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

update config file:  https://docs.mongodb.com/manual/reference/configuration-options/

        $> sudo vi /etc/mongod.conf 
        
        net:
          port: 27017    
          bindIp: 127.0.0.1   --> bindIp: 0.0.0.0 


        security:   -->  security:
                           authorization: enabled

restart mongodb after updated conf

        $> sudo service mongod restart
        
create root user

        (base) ianguo@ianguo:~$ mongo
        MongoDB shell version v4.0.5
        connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
        Implicit session: session { "id" : UUID("5a64027f-294a-47d9-afdf-26e5b42abc5c") }
        MongoDB server version: 4.0.5
        > use admin
        switched to db admin
        > db.createUser({ user: "root", pwd: "abcdef", roles: [{ role: "root", db: "admin" }] });
        Successfully added user: {
                "user" : "root",
                "roles" : [
                        {
                                "role" : "root",
                                "db" : "admin"
                        }
                ]
        }
        > db.auth("root", "abcdef");
        1
        
restart and login with root to create db auth user as below:

        (base) ianguo@ianguo:~$ sudo service mongod restart
        mongod stop/waiting
        mongod start/running, process 31940
        (base) ianguo@ianguo:~$ mongo --port 27017 -u"root" --authenticationDatabase "admin" -p"abcdef"
        MongoDB shell version v4.0.5
        connecting to: mongodb://127.0.0.1:27017/?authSource=admin&gssapiServiceName=mongodb
        Implicit session: session { "id" : UUID("acb71691-065f-4525-800a-c59dfe2b28aa") }
        MongoDB server version: 4.0.5
        Server has startup warnings: 
        2019-05-17T11:18:46.066+0800 I STORAGE  [initandlisten] 
        2019-05-17T11:18:46.066+0800 I STORAGE  [initandlisten] ** WARNING: Support for MMAPV1 storage engine has been deprecated and will be
        2019-05-17T11:18:46.066+0800 I STORAGE  [initandlisten] **          removed in version 4.2. Please plan to migrate to the wiredTiger
        2019-05-17T11:18:46.066+0800 I STORAGE  [initandlisten] **          storage engine.
        2019-05-17T11:18:46.066+0800 I STORAGE  [initandlisten] 
        ---
        Enable MongoDB's free cloud-based monitoring service, which will then receive and display
        metrics about your deployment (disk utilization, CPU, operation statistics, etc).

        The monitoring data will be available on a MongoDB website with a unique URL accessible to you
        and anyone you share the URL with. MongoDB may use this information to make product
        improvements and to suggest MongoDB products and deployment options to you.

        To enable free monitoring, run the following command: db.enableFreeMonitoring()
        To permanently disable this reminder, run the following command: db.disableFreeMonitoring()
        ---

        > use admin
        switched to db admin
        > db.auth("root","abcdef")
        1
        > use ELSA_WPG_TMS
        switched to db ELSA_WPG_TMS
        > show users
        
        > db.createUser({ user: "mongodb", pwd: "123456", roles: [{ role: "dbOwner", db: "ELSA_WPG_TMS"}]})
        Successfully added user: {
                "user" : "mongodb",
                "roles" : [
                        {
                                "role" : "dbOwner",
                                "db" : "ELSA_WPG_TMS"
                        }
                ]
        }
        > db.grantRolesToUser("mongodb",[{role:"dbOwner",db:"ELSA_WPG_TMS"}])
        >exit()
        bye

delete database

        > use ELSA-USER-SERVICE
        switched to db ELSA-USER-SERVICE
        > db.dropDatabase()
        { "dropped" : "ELSA-USER-SERVICE", "ok" : 1 }
