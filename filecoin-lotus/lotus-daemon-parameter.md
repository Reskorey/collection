lotus damon  启动节点

lotus damon stop  停止节点程序

lotus damon 

​	--api 1234   :设置api 端口为1234

​	--makeGenFlag ""

​	--preTemplateFlag 

​												//第一次运行时，从给定文件导入默认密钥

​	--import-key  fileName  : on first run, import a default key from a given file

​												

​												//用于第一节点运行的创世文件

​	--genesis  genesisFile   ：genesis file to use for first node run

​												

​	--bootstrap  true

​														//第一次运行时，从给定的文件或URL加载链数据并进行验证

​	--import-chain  chaindata.car   :on first run, load chain from given file or url and validate



​																//从给定的链导出文件或URL导入链状态

​	--import-snapshot snapshotFile  :import chain state from a given chain export file or url



​																//从文件导入链数据后停止节点程序

​	--halt-after-import chaindata.car  :halt the process after importing chain from file



​				//以轻节点模式启动Lotus

​	--lite   :start lotus in lite mode



​					//指定用于将cpu配置文件写入的文件名

​	--pprof     :specify name of file for writing cpu profile to

​		

​					//指定节点类型

​	--profile   :specify type of node



​											//管理打开文件限制

​	--manage-fdlimit  true  :manage open file limit

​									

​									//指定要使用的配置文件的路径

​	--config  filepath  :specify path of config file to use