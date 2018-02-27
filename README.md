# rabbitmq-cluster

 - Here is the documentation for setting up a two node RabbitMQ cluster. Cluster will be setup as a [rabbitmq brocker](https://www.rabbitmq.com/clustering.html) with high availability ([Mirroring](https://www.rabbitmq.com/ha.html)) 

**Prerequisites**
	
	* 2 ec2 instances launched inside a private subnet(Will be referred as rabbitmqmaster and rabbitmqslave here after)
	* m3.large machine (Can select a suitable instance type depending on requirement)
	* Ubuntu 12.04 OS
	

**Installation**

	* apt-get update
	* apt-get install service-discovery dirsprepare
	* apt-get install rabbitmq-server cc-rabbitmq
	

