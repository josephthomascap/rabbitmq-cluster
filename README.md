# rabbitmq-cluster

 - Here is the documentation for setting up a two node RabbitMQ cluster. Cluster will be setup as a [rabbitmq brocker](https://www.rabbitmq.com/clustering.html) with high availability ([Mirroring](https://www.rabbitmq.com/ha.html)) 

**Prerequisites**
	
* 2 ec2 instances launched inside a private subnet(Will be referred as rabbitmqmaster and rabbitmqslave here after)
* m3.large machine (Can select a suitable instance type depending on requirement)
* Ubuntu 12.04 OS
* Both servers should be accessible via there hostnames from each other. Include respective entries in */etc/hosts* file of both servers
 	

**Installation**

	apt-get update
	apt-get install service-discovery dirsprepare
	apt-get install rabbitmq-server cc-rabbitmq
	
**Setup on Master server (rabbitmqmaster)**

* Both the servers should be having same erlang cookie. Hence, copy erlang cookie from *rabbitmqmaster* to *rabbitmqslave*
        
	scp /var/lib/rabbitmq/.erlang.cookie <username>@rabbitmqslave:/var/lib/rabbitmq/.erlang.cookie
* Verify cluster status by running the following command

	rabbitmqctl cluster_status

**Setup on Slave server (rabbitmqslave)**

* Correct the ownership and permission of erlang cookie file
	
	chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie
	chmod 400 /var/lib/rabbitmq/.erlang.cookie


