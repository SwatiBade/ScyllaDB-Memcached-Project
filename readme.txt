In this version, I have configured docker for quickly running scylladb. Updated the code for the tracks, playlists, users, and descriptions microservices to use ScyllaDB,a wide-column NoSQL database compatible with Cassandra.
https://www.scylladb.com/download/#docker

I have changes the database from sqlite3 shared database to ScyllaDB. Cassandra and ScyllaDB should run in a cluster of multiple servers. Since we are doing development on a single VM and RAM is at a premium, we will start only a single instance of ScyllaDB. Use the following command (entered all on one line):
		
$ docker run --name scylla -d scylladb/scylla --smp 1 --memory 1G --overprovisioned 1 -developer-mode 1 --experimental 1

Once the image has been downloaded, wait a few moments, then check that ScyllaDB is up
with
		$ docker exec -it scylla nodetool status

If this command fails, you can check for errors in the ScyllaDB logs by running
		$ docker logs scylla

Memory allocation- 
Note that the --memory 1G switch is a minimum value for starting ScyllaDB on a Linux VM with
3G of RAM. If you run ScyllaDB on another platform, you may need to adjust that figure. You
may also wish to increase it if your host has more RAM available.
Leaving off the --memory switch entirely is not recommended unless you are on hardware
dedicated exclusively to running ScyllaDB.

Managing ScyllaDB-
Once ScyllaDB is up, you can execute CQL commands using
	$ docker exec -it scylla cqlsh

If you need to stop ScyllaDB, use
	$ docker stop scylla
and restart with
	$ docker start scylla

If something goes very wrong, you can remove ScyllaDB completely and start over with
	$ docker rm -f scylla &amp;&amp; docker rmi scylladb/scylla



I also have used Memecached, to save the latest response in the cache. This will minimise the time required to get response from micro-services.

Installing Memcached-
Use the following command to install and start the Memcached distributed object cache and
administration tools:
	$ sudo apt install --yes memcached libmemcached-tools

You can verify that the server is running by retrieving cache statistics:
	$ memcstat --servers=localhost

Among the statistics you may need for later testing are get_hits, get_misses, and
curr_items.

Installing pymemcache- 

Install the pymemcache library, either by running
	$ sudo apt install --yes python3-pymemcache
or
	$ pip3 install --user pymemcache


Memcache details can be found here-  

Micro-services can be tested using Postman. Details about request/response details cab be found in the file "Music_Microservices_Part-3_Documentation". 



To Run all Micro-services execute below command on terminal after navigating to the folder you have downloaded the project:
-----------------------------
foreman start


To Test all micro services:
-------------------------
python3 app.py test