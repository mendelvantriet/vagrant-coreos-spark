# vagrant-spark_cluster

This Project is still under development.

Set up a CoreOS standalone spark cluster using Vagrant. Tested on VirtualBox.

## Initialization

### Docker image

Build my spark docker image, save it to file, gzip it and rename it to ./synced_folder/spark.docker.tar.gz.

### Discovery URL

	DISCOVERY_URL=$(curl -w "\n" 'https://discovery.etcd.io/new?size=2')
	sed -e 's#discovery: .*#discovery: '$DISCOVERY_URL'#' user-data.sample > user-data

## Start cluster

	vagrant up

## Start spark master and 2 workers

	vagrant ssh -c "fleetctl start /home/core/share/spark.master.service"
	vagrant ssh -c "fleetctl start /home/core/share/spark.worker@1.service"
	vagrant ssh -c "fleetctl start /home/core/share/spark.worker@2.service"

## SSH into node-01

	vagrant ssh node-01
or
	ssh -i ~/.vagrant.d/insecure_private_key vagrant@127.0.0.1 -p 2222

## Test webUI

	curl http://192.168.9.11:8080/

## Test Spark

Submit an application from within the cluster:

	vagrant ssh node-01
	docker exec -it spark-master spark-submit --master spark://$(etcdctl get /services/spark/master-ip):7077 examples/src/main/python/pi.py

Alternatively, you can submit applications from outside of the cluster:

	spark-submit --master spark://192.168.9.11:7077 $SPARK_HOME/examples/src/main/python/pi.py

Spark standalone does not support deploy-mode cluster, so we will be running in client mode (which is the default for `spark-submit`). Consequently, the driver will run on your client (where you run `spark-submit`). Because the executor and master need to connect to the driver, make sure your client is accessible from the cluster. Make sure your firewall settings allow this (by running `iptables -P INPUT ACCEPT` for example).
If your client is running from another virtual machine, like it is in my case, make sure your vm is on the same network as your cluster. (In Vagrant, this could be something like: `config.vm.network "private_network", ip: "192.168.9.2"`). Additionally, make sure that your client vm is using this interface for outgoing connections, instead of some host-only or NAT interface. In my case this meant bringing down VirtualBox's NAT interface eth0: `sudo ifdown eth0`.

Issues like this might also arise if you want to run a 'real' spark cluster. Good luck and don't forget to have fun!

## TODO

Lots.

- Zookeeper??
- Ansible??

## Disclaimer

Fit for no purpose.
