# MongoDB-Notes
MongoDB Sharding ReplicaSets etc.


Installation of mangodb on Ubuntu

sudo apt-get update

Import the public key used by the package management system.

wget -qO - https://www.mongodb.org/static/pgp/server-5.0.asc | sudo apt-key add -


sudo apt-get install -y mongodb-org

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


