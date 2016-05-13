# vagrant-spark_cluster

This Project is still under development.

Set up a CoreOS spark cluster using vagrant. Tested on virtual box.

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

	vagrant ssh node-01
	docker exec -it spark-master spark-submit --master spark://$(etcdctl get /services/spark/master-ip):7077 examples/src/main/python/pi.py

## TODO

Lots.

## Disclaimer

Fit for no purpose.
