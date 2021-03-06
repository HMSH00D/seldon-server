SHELL=/bin/bash

SELDON_HOME := $(abspath $(dir $(lastword $(MAKEFILE_LIST)))/../..)

#
# Simple config for hostPath setting for demo cluster
# Will change to more complex config if we allow alternate storage volumes for demo
#

.PHONY: microservice conf

#
# core conf
#

#
# Example data volume conf for hostPath and glusterfs
#
DATA_VOLUME="hostPath": {"path": "/seldon-data"}
#DATA_VOLUME="glusterfs": {"endpoints": "glusterfs-cluster","path": "gv0","readOnly": false}

#
# Example seldon server service types. Change to the kubernetes type you need.
#
SELDON_SERVICE_TYPE=NodePort
#SELDON_SERVICE_TYPE=LoadBalancer

control.json:control.json.in
	cat control.json.in | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > control.json

mysql.json:mysql.json.in
	cat mysql.json.in | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > mysql.json

td-agent-server.json:td-agent-server.json.in
	cat td-agent-server.json.in | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > td-agent-server.json

spark-master.json: spark-master.json.in
	cat spark-master.json.in | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > spark-master.json

spark-workers.json: spark-workers.json.in
	cat spark-workers.json.in | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > spark-workers.json

influxdb-grafana.json: influxdb-grafana.json.in
	cat influxdb-grafana.json.in | sed -e 's|%SERVICE_TYPE%|$(SELDON_SERVICE_TYPE)|' | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > influxdb-grafana.json

examples/reuters/import-data-job.json: examples/reuters/import-data-job.json.in
	cat examples/reuters/import-data-job.json.in  | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > examples/reuters/import-data-job.json

examples/ml100k/ml100k-import.json: examples/ml100k/ml100k-import.json.in
	cat examples/ml100k/ml100k-import.json.in  | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > examples/ml100k/ml100k-import.json

server.json :server.json.in
	cat server.json.in | sed -e 's|%SERVICE_TYPE%|$(SELDON_SERVICE_TYPE)|' | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > server.json

#
# Development conf 
#

dev/server.json :dev/server.json.in
	cat dev/server.json.in | sed -e 's|%SELDON_HOME%|$(SELDON_HOME)|' | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > dev/server.json

dev/control.json :dev/control.json.in
	cat dev/control.json.in | sed -e 's|%SELDON_HOME%|$(SELDON_HOME)|' | sed -e 's|%DATA_VOLUME%|$(DATA_VOLUME)|' > dev/control.json


#
# glusterfs
#

GLUSTERFS_IP1=192.168.0.149
GLUSTERFS_IP2=192.168.0.248

glusterfs.json:glusterfs.json.in
	cat glusterfs.json.in | sed -e 's|%GLUSTERFS_IP1%|${GLUSTERFS_IP1}|'| sed -e 's|%GLUSTERFS_IP2%|${GLUSTERFS_IP2}|' > glusterfs.json

conf:control.json mysql.json td-agent-server.json spark-master.json spark-workers.json examples/reuters/import-data-job.json glusterfs.json server.json examples/ml100k/ml100k-import.json dev/server.json dev/control.json influxdb-grafana.json


#
# microservice conf
#

MICROSERVICE_NAME=reuters-example
MICROSERVICE_IMAGE=seldonio/reuters-example
MICROSERVICE_VERSION=1.0


microservice-${MICROSERVICE_NAME}.json:microservice.json.in
	cat microservice.json.in | sed -e 's|%NAME%|${MICROSERVICE_NAME}|' | sed -e 's|%IMAGE%|${MICROSERVICE_IMAGE}|' | sed -e 's|%VERSION%|${MICROSERVICE_VERSION}|' > microservice-${MICROSERVICE_NAME}.json

microservice:microservice-${MICROSERVICE_NAME}.json



clean:
	rm -f mysql.json
	rm -f control.json
	rm -f td-agent-server.json
	rm -f spark-master.json
	rm -f spark-workers.json
	rm -f server.json
	rm -f influxdb-grafana.json
	rm -f examples/reuters/import-data-job.json
	rm -f examples/ml100k/ml100k-import.json
	rm -f microservice-${MICROSERVICE_NAME}.json
	rm -f glusterfs.json
	rm -f dev/server.json
	rm -f dev/control.json
