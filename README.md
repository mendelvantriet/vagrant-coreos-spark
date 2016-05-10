# vagrant-spark_cluster

This Project is still under development.

Set up a CoreOS spark cluster using vagrant.

## Usage

	DISCOVERY_URL=$(curl -w "\n" 'https://discovery.etcd.io/new?size=2')
	sed -e 's#discovery: .*#discovery: '$DISCOVERY_URL'#' user-data.sample > user-data
	vagrant up

## ssh into node 1

	vagrant ssh node-01
or
	ssh -i ~/.vagrant.d/insecure_private_key vagrant@127.0.0.1 -p 2222
