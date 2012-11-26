Omid
=====

This project provides transactional support for HBase (or any other key-value store) using Snapshot Isolation.
If you have any question, please contact us at omid-project@googlegroups.com or read [the online archives](https://groups.google.com/forum/?fromgroups=#!forum/omid-project)

Architecture
------------

Omid is composed by a server (the Status Oracle) and several clients. The server contains all the information
needed to manage transactions and replicates it to the clients which just contact the server when they want
to start a transaction or commit it.

The server uses BookKeeper as a Writer Ahead Log where it dumps all its state. In case of crash it is possible
to restart the server without losing any commit information.

Compilation
-----------

Omid uses Maven for its build system. We are using a temporary repository for zookeeper and bookkeeper packages to ease
the installation procedure

Then to compile omid:

    $ tar jxvf omid-1.0-SNAPSHOT.tar.bz2
    $ cd omid-1.0-SNAPSHOT
    $ mvn install

Tests should run cleanly.

Running
-------

You need to run four components before running the transactional
client. They are bookkeeper, zookeeper, omid tso and
hbase. Bookkeeper is needed by the TSO. Zookeeper is needed by
bookkeeper and hbase. The TSO is needed by hbase. Hence, the order of
starting should be: 
	 1. Zookeeper
	 2. Bookkeeper
	 3. TSO
	 4. Hbase

### Zookeeper & Bookkeeper
For simplicity we've included a utility script which starts zookeeper
and bookkeeper. Run:

    $ bin/omid.sh bktest

Omid doesn't use anything special in zookeeper or bookkeeper, so you
can use any install for these. However, if you are running this
anywhere but localhost, you need to update the setting for hbase and 
TSO. See the hbase docs for changing the zookeeper quorum. For TSO,
you need to modify bin/omid.sh.

### TSO
To start the TSO, run:
   
    $ bin/omid.sh tso

### Benchmark
To benchmark the TSO alone, run:

    $ bin/omid.sh tsobench

### HBase
We've included a utility script to start a HBase cluster on your local
machine. Run:

    $ bin/omid.sh tran-hbase

For running in a cluster

API
---

The public api is in

    src/main/java/com/yahoo/omid/client/TransactionalTable.java
    src/main/java/com/yahoo/omid/client/TransactionState.java
    src/main/java/com/yahoo/omid/client/TransactionManager.java

For an example of usage, look in

    src/test/java/com/yahoo/omid/TestBasicTransaction.java

Logging 
-------
Logging can be adjusted in src/main/resource/log4j.properties.


Acknowledgement
-------
This project has been partially supported by the EU Comission through the Cumulo Nimbo project (FP7-257993). 