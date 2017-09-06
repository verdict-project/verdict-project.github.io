---
layout: post
title: Quick Start Guide
---

* TOC
{:toc}

Verdict can run on top of [Apache Hive](https://hive.apache.org/), [Apache Impala](https://impala.incubator.apache.org/), and [Apache Spark](https://spark.apache.org/). We are adding drivers for other database systems, such as Amazon Redshift, Google BigQuery, Google Dataproc, etc.

Using Verdict is easy. Following this guide, you can finish setup in five minutes if you have any of those supported systems ready.

## Downloading and Building Verdict

Downloading and building Verdict requires only a couple steps. Building Verdict does not require any `sudo` access.

1. **Download** and unzip [the latest release (version 0.3.0)](https://github.com/mozafari/verdict/releases/download/v0.3.0/verdict-0.3.0.zip).
1. **Type** `mvn package` in the unzipped directory. The command will download all the dependencies and compile Verdict's code. The command will create three `jar` files in the `target` directory.

This is all!

### More details

Verdict is tested on Oracle JDK 1.7 or above, but it should work with open JDK, too. `mvn` is the command for the [Apache Maven](https://maven.apache.org/) package manager. If you do not have the Maven, you will have to install it. The official page for the Maven installation is [this](https://maven.apache.org/install.html).

Verdict is currently tested with the systems included in a Cloudera distribution ([CDH 5.11](https://www.cloudera.com/documentation/enterprise/release-notes/topics/cdh_rn_new_in_cdh_511.html)). However, Verdict should work with other versions as well. Please let us know if Verdict is incompatible with your environment.


## Using Verdict

Verdict takes a slightly different approach depending on the database system it works with. Once connected, however, you can issue the same SQL queries.


### On Apache Spark

Verdict works with Spark by creating Spark's HiveContext internally. In this way, Verdict can load persisted tables through Hive Metastore. Verdict is tested with Apache Spark 1.6.0 (in the Cloudera distribution CDH 5.11). We will support Spark 2.0 shortly.

We show how to use Verdict in `spark-shell` and `pyspark`. Using Verdict in a Spark application written either in Scala or Python is the same.

Due to the seamless integration of Verdict on top of Spark (and PySpark), Verdict can be used within [Apache Zeppelin](https://zeppelin.apache.org/) notebooks and Python [Jupyter](http://jupyter.org/) notebooks. See this page for more detail about how to set up Verdict with Zeppelin or Jupyter.


#### Verdict-on-Spark

You can start `spark-shell` with Verdict as follows.

```bash
$ spark-shell --jars target/verdict-core-0.3.0-jar-with-dependencies.jar
```

After spark-shell starts, import and use Verdict as follows.

```scala
import edu.umich.verdict.VerdictSparkHiveContext

scala> val vc = new VerdictSparkHiveContext(sc)   // sc: SparkContext instance

scala> vc.sql("show databases").show(false)       // Simply displays the databases (or often called schemas)

// Creates samples for the table. This step needs to be done only once for the table.
// The created tables are automatically persisted through HiveContext and can be used in the other
// pyspark applications.
scala> vc.sql("create sample of database_name.table_name").show(false)

// Now Verdict automatically uses available samples for speeding up this query.
scala> vc.sql("select count(*) from database_name.table_name").show(false)
```

The return value of `VerdictSparkHiveContext#sql()` is a Spark's DataFrame class; thus, any methods that work on Spark's DataFrame work on Verdict's answer seamlessly.


#### Verdict-on-PySpark

You can start `pyspark` shell with Verdict as follows.

```bash
$ export PYTHONPATH=$(pwd)/python:$PYTHONPATH

$ pyspark --driver-class-path target/verdict-core-0.3.0-jar-with-dependencies.jar
```

**Limitation**: Note that, for the `--driver-class-path` option to work, the jar file (i.e., `target/verdict-core-0.3.0-jar-with-dependencies.jar`) must be placed in the Spark's driver node. Verdict will support `--jars` option shortly.

After pyspark shell starts, import and use Verdict as follows.

```python
>>> from pyverdict import VerdictHiveContext

>>> vc = VerdictHiveContext(sc)        # sc: SparkContext instance

>>> vc.sql("show databases").show()    # Simply displays the databases (or often called schemas)

# Creates samples for the table. This step needs to be done only once for the table.
# The created tables are automatically persisted through HiveContext and can be used in the other
# pyspark applications.
>>> vc.sql("create sample of database_name.table_name").show()

# Now Verdict automatically uses available samples for speeding up this query.
>>> vc.sql("select count(*) from database_name.table_name").show()
```

The return value of `VerdictHiveContext#sql()` is a pyspark's DataFrame class; thus, any methods that work on pyspark's DataFrame work on Verdict's answer seamlessly.


### On Apache Impala, Apache Hive, Amazon Redshift

We will use our command line interface (which is called `veeline`) for connecting to those databases. You can programmatically connect to Verdict using the standard JDBC interface, too. See [this page](http://verdictdb.org) for the instructions for a JDBC connection.

#### Verdict-on-Impala

Type the following command in terminal to launch `veeline` that connects to Impala.

```bash
$ veeline/bin/veeline -h "impala://hostname:port/schema;key1=value1;key2=value2;..." -u username -p password
```

Note that parameters are delimited using semicolons (`;`). The connection string is quoted since the semicolons have special meaning in bash. The user name and password can be passed in the connection string as parameters, too.

Verdict supports the Kerberos connection. For this, add `principal=user/host@domain` as one of those key-values pairs.

After `veeline` launches, you can issue regular SQL queries as follows.

```
verdict:impala> show databases;

// Creates samples for the table. This step needs to be done only once for the table.
verdict:impala> create sample of database_name.table_name;

verdict:impala> select count(*) from database_name.table_name;

verdict:impala> !quit
```

#### Verdict-on-Hive

Type the following command in terminal to launch `veeline` that connects to Hive.

```bash
$ veeline/bin/veeline -h "hive2://hostname:port/schema;key1=value1;key2=value2;..." -u username -p password
```

Note that parameters are delimited using semicolons (`;`). The connection string is quoted since the semicolons have special meaning in bash. The user name and password can be passed in the connection string as parameters, too.

Verdict supports the Kerberos connection. For this, add `principal=user/host@domain` as one of those key-values pairs.

After `veeline` launches, you can issue regular SQL queries as follows.

```
verdict:Apache Hive> show databases;

// Creates samples for the table. This step needs to be done only once for the table.
verdict:Apache Hive> create sample of database_name.table_name;

verdict:Apache Hive> select count(*) from database_name.table_name;

verdict:Apache Hive> !quit
```

#### Verdict-on-Redshift

This is under development at the moment.


#### Notes on using `veeline`

`veeline` makes a JDBC connection to the database systems that Verdict work on top of (e.g., Impala or Hive). For this, it uses the JDBC drivers stored in the `lib` folder. Our code ships by default with the Cloudera's Impala and Hive JDBC drivers (jar files). However, if these drivers are not compatible with your environment, you can put the compatible JDBC drivers in the `lib` folder after deleting existing ones.


## What's Next

See what types of queries are supported by Verdict in [**this page**](http://verdictdb.org), and enjoy the speedup provided Verdict for those queries.

Learn in [**this page**]() how to quickly visualize your query answers using Verdict in [Apache Zeppelin](https://zeppelin.apache.org/) or [Jupyter](http://jupyter.org/).

If you have use cases that are not supported by Verdict, please contact us at `verdict-user@umich.edu`, or create an issue in our Github repository. We will answer your questions or requests shortly (at most in a few days).
