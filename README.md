# pinyougou
1、启动pinyougou-server-dubbox虚拟机：192.168.25.128
	用户名:root		密码：itcast
	1.启动zookeeper
		[root@localhost Desktop]# cd /root/zookeeper-3.4.6/bin
		[root@localhost bin]# ./zkServer.sh start
		JMX enabled by default
		Using config: /root/zookeeper-3.4.6/bin/../conf/zoo.cfg
		Starting zookeeper ... STARTED
		[root@localhost bin]# ./zkServer.sh status
		JMX enabled by default
		Using config: /root/zookeeper-3.4.6/bin/../conf/zoo.cfg
		Mode: standalone
		[root@localhost bin]# 
		
	2.启动activemq
		[root@localhost Desktop]# cd /root/apache-activemq-5.12.0/bin
		[root@localhost bin]# ./activemq start
		INFO: Loading '/root/apache-activemq-5.12.0//bin/env'
		INFO: Using java '/usr/local/src/java/jdk1.7.0_71/bin/java'
		INFO: Starting - inspect logfiles specified in logging.properties and log4j.properties to get details
		INFO: pidfile created : '/root/apache-activemq-5.12.0//data/activemq.pid' (pid '2557')

	3.启动redis
		H:\heima\品优购双元资料V1.5_20180129\品优购资源V1.3\配套软件\Redis\redis2.8win32
		双击redis-server.exe	占用端口6379
		
	4.启动solr
		H:\heima\pinyougou\solr\apache-tomcat-7.0.73\bin
		双击startup.bat
		
	5.启动cas服务端
		H:\heima\pinyougou\cas\apache-tomcat-7.0.73
		双击startup.bat
	
2、启动pinyougou-image-server_133虚拟机：192.168.25.133

3、启动pinyougou-server-jiqun
	1.启动zookeeper集群
		sh /usr/local/zookeeper-cluster/zookeeper-1/bin/zkServer.sh start
		sh /usr/local/zookeeper-cluster/zookeeper-2/bin/zkServer.sh start
		sh /usr/local/zookeeper-cluster/zookeeper-3/bin/zkServer.sh start
	2.启动solr集群
		sh /usr/local/solr-cluster/solr1/apache-tomcat-7.0.73/bin/startup.sh
		sh /usr/local/solr-cluster/solr2/apache-tomcat-7.0.73/bin/startup.sh
		sh /usr/local/solr-cluster/solr3/apache-tomcat-7.0.73/bin/startup.sh
		sh /usr/local/solr-cluster/solr4/apache-tomcat-7.0.73/bin/startup.sh
	3.启动redis集群
		sh /usr/local/redis-cluster/startall-redis.sh 
	  关闭:
		sh /usr/local/redis-cluster/shutdownall-redis.sh 

		
		
代码连正式环境
	pinyougou-common run as --> install -P pro
代码连测试环境
	pinyougou-common run as --> install -P dev 或者install
	
	
	
工程启动顺序：
服务层：pinyougou-sellergoods-service	Running war on http://localhost:9001/	20881
		pinyougou-content-service		Running war on http://localhost:9002/	20882
		pinyougou-search-service		Running war on http://localhost:9003/	20883
		pinyougou-page-service			Running war on http://localhost:9004/	20884
		（采用mq以后不用dubbo，但20天下午把zookeeper配置文件放到pinyougou-common中，dubbo和zookeeper还得放开）
		itcast_sms_service								springboot		9005
		pinyougou-user-service			Running war on http://localhost:9006/	20886
		pinyougou-cart-service			Running war on http://localhost:9007/	20887
		pinyougou-order-service			Running war on http://localhost:9008/	20888
		pinyougou-pay-service			Running war on http://localhost:9009/	20889
		pinyougou-seckill-service		Running war on http://localhost:9010/	20890
		
		
表现层：solr服务端tomcat占用端口					   http://localhost:8080/solr
		cas服务端tomcat占用端口						   http://localhost:9100/cas/login
													   http://localhost:9100/cas/logout?service=http://www.baidu.com
		pinyougou-manager-web(运营商)	Running war on http://localhost:9101/	依赖sellergoods、content、search、page
		pinyougou-shop-web(商家)		Running war on http://localhost:9102/	依赖sellergoods
		pinyougou-portal-web(前台首页)	Running war on http://localhost:9103/	依赖content
		pinyougou-search-web(搜索)		Running war on http://localhost:9104/	依赖search
		pinyougou-page-web(详情页)	 	Running war on http://localhost:9105/	不依赖服务层
		pinyougou-user-web(用户中心)	Running war on http://localhost:9106/	依赖user
		pinyougou-cart-web(购物车)		Running war on http://localhost:9107/	依赖cart、order、user、pay
			购物车url					http://localhost:9107/cart.html
			支付成功回显金额			http://localhost:9107/paysuccess.html#?money=0.03
		pinyougou-seckill-web(秒杀)		Running war on http://localhost:9108/	依赖seckill
			添加商品到购物车url			http://localhost:9107/cart/addGoodsToCartList.do?itemId=1369296&num=1
			查看购物车					http://localhost:9107/cart/findCartList.do
			测试数据：	1369286	老年机 移动4G 16G 价格0.02
						1369287	老年机 移动4G 64G 价格0.01
		pinyougou-task-service			Running war on http://localhost:9109/
			
				
			
常用命令：
	压缩解压：
		#比如把x文件夹打包并用gzip压缩。
			tar -zcvf x.tar.gz x
		#解压到当前目录
			tar -zxvf x.tar.gz
		#解压到父目录
			tar -xzvf x.tar.gz -C ..
linux关闭防火墙：
		1) 即时生效，重启后失效 
			开启： service iptables start 
			关闭： service iptables stop 	
		2) 重启后生效 
			开启： chkconfig iptables on 
			关闭： chkconfig iptables off 					
linux下根据pid查询端口
	    1) netstat -antup|grep PID
       查询端口是否占用
        2) netstat -anp | grep 8180	   			
			
			
			
			
			
