# MongoDB-Notes
MongoDB Sharding ReplicaSets etc.


Installation of mangodb on Ubuntu

sudo apt-get update

Import the public key used by the package management system.

wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -


sudo apt-get install -y mongodb-org

mongodb-org -> A metapackage that automatically installs the component packages listed below.
mongodb-org-server
mongodb-org-mongos
mongodb-org-shell


For a specific version

sudo apt-get install -y mongodb-org=5.0.5 mongodb-org-database=5.0.5 mongodb-org-server=5.0.5 mongodb-org-shell=5.0.5 mongodb-org-mongos=5.0.5 mongodb-org-tools=5.0.5

Start monngod
Mongod is the primary daemon process for the MongoDB system. It handles data requests, manages data access, and performs background management operations.

sudo systemctl start mongod

If you get an error ->
sudo systemctl daemon-reload

To check status of mongod 
sudo systemctl status mongod  (enable, stop, restart, status)


Mongod logs will be on ->
/var/log/mongodb/mongod.log

Configuration Files will be on
/etc/mongod.conf

You can configure mongod and mongos instances at startup using a configuration file. The configuration file contains settings that are equivalent to the mongod and mongos command-line options.

Localhost Binding by Default

By default, MongoDB launches with bindIp set to 127.0.0.1, which binds to the localhost network interface. This means that the mongod can only accept connections from clients that are running on the same machine. Remote clients will not be able to connect to the mongod, and the mongod will not be able to initialize a replica set unless this value is set to a valid network interface.

