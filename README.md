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


SHARDING

Inside the config servers 1 primary 2 secondary run the following commands

--> Port of first one 4001
mongod --configsvr --replSet cfgrs --port 27017 --dbpath /data/db

--> Port of first second one 4002
mongod --configsvr --replSet cfgrs --port 27017 --dbpath /data/db

--> Port of first third one 4003
mongod --configsvr --replSet cfgrs --port 27017 --dbpath /data/db

Connect to any config server 
mongo mongodb://192.168.1.81:40001
Then start config servers.
rs.initiate(
  {
    _id: "cfgrs",
    configsvr: true,
    members: [
      { _id : 0, host : "192.168.1.81:40001" },
      { _id : 1, host : "192.168.1.81:40002" },
      { _id : 2, host : "192.168.1.81:40003" }
    ]
  }
)

rs.status()

Shard 01

50001
mongod --shardsvr --replSet shard1rs --port 27017 --dbpath /data/db

50002 - secondary
mongod --shardsvr --replSet shard1rs --port 27017 --dbpath /data/db

50003 - secondary
mongod --shardsvr --replSet shard1rs --port 27017 --dbpath /data/db

Connect to any shard
mongo mongodb://192.168.1.81:50001

rs.initiate(
  {
    _id: "shard1rs",
    members: [
      { _id : 0, host : "192.168.1.81:50001" },
      { _id : 1, host : "192.168.1.81:50002" },
      { _id : 2, host : "192.168.1.81:50003" }
    ]
  }
)

rs.status()


Mongos

Connect mongos withh confog servers
6000 -> one monngos
mongos --configdb cfgrs/192.168.1.81:40001,192.168.1.81:40002,192.168.1.81:40003 --bind_ip 0.0.0.0 --port 27017

Connect to router
mongo mongodb://192.168.1.81:60000

mongos> sh.addShard("shard1rs/192.168.1.81:50001,192.168.1.81:50002,192.168.1.81:50003")
mongos> sh.status()




