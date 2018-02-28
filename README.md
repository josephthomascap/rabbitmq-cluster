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

<p>Both the servers should be having same erlang cookie. Hence, copy erlang cookie from <i>rabbitmqmaster</i> to <i>rabbitmqslave</i> </p>
	
	scp /var/lib/rabbitmq/.erlang.cookie <username>@rabbitmqslave:/var/lib/rabbitmq/.erlang.cookie
<p>Verify cluster status by running the following command</p>
	
	rabbitmqctl cluster_status
	
	Result should look something similar to:
	
	root@rabbitmqmaster:/home/vagrant# rabbitmqctl cluster_status
	Cluster status of node rabbit@rabbitmqmaster ...
	[{nodes,[{disc,[rabbit@rabbitmqmaster]}]},
 	{running_nodes,[rabbit@rabbitmqmaster]},
 	{cluster_name,<<"rabbit@rabbitmqmaster">>},
 	{partitions,[]},
 	{alarms,[{rabbit@rabbitmqmaster,[]}]}]
	root@rabbitmqmaster:/home/vagrant#	

**Setup on Slave server (rabbitmqslave)**

<p>Correct the ownership and permission of erlang cookie file</p>
	
	chown rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie  
	chmod 400 /var/lib/rabbitmq/.erlang.cookie  

<p>Verify cluster status <p>

	rabbitmqctl cluster_status 
	
	Result should look something similar to:
	
	root@rabbitmqslave:/home/vagrant# rabbitmqctl cluster_status  
	Cluster status of node rabbit@rabbitmqslave ...  
	[{nodes,[{disc,[rabbit@rabbitmqslave]}]},  
 	{running_nodes,[rabbit@rabbitmqslave]},  
 	{cluster_name,<<"rabbit@rabbitmqslave">>},   
 	{partitions,[]},  
 	{alarms,[{rabbit@rabbitmqslave,[]}]}]  
	root@rabbitmqslave:/home/vagrant#  
	
<p>Stop running app </p>
	
	rabbitmqctl stop_app
<p>Join slave server to the cluster</p>

	rabbitmqctl join_cluster rabbit@rabbitmqmaster
	
	Result should look like:
	root@rabbitmqslave:/home/vagrant# rabbitmqctl join_cluster rabbit@rabbitmqmaster
	Clustering node rabbit@rabbitmqslave with rabbit@rabbitmqmaster ...
	root@rabbitmqslave:/home/vagrant# 
	
	
<p>We are done with Rabbitmq brocker setup :) Both the machines will start sharing users, virtual hosts, queues, exchanges, bindings, and runtime parameters now. However, we need to setup mirroring in order to start sharing the queues. </p>

**Setup Mirroring (High Availability) - On Master node**

<p>The following command will sync all the queues across all nodes:</p>
	
	rabbitmqctl set_policy ha-all "" '{"ha-mode":"all","ha-sync-mode":"automatic"}'
	
<p>After setting HA poliy on master, you can verify whether it is getting replicated on slave by ruuning following command on <i>rabbitmqslave</i> machine<p>
	
	rabbitmqctl list_policies
	
	




