---
layout: post
title: Quick Start Guide
---

* TOC
{:toc}

Verdict can run on top of [Apache Hive](https://hive.apache.org/), [Apache Impala](https://impala.incubator.apache.org/), and [Apache Spark](https://spark.apache.org/). We are adding drivers for other database systems, such as Amazon Redshift, Google BigQuery, Google Dataproc, etc.

Using Verdict is easy. Following this guide, you can finish setup in five minutes if you have any of those supported systems ready.

## Download and Install Verdict

See [this page]({{ site.baseurl }}/download/) to download jar or zip archives relevant to your data analytics platforms.


## Using Verdict

Verdict takes a slightly different approach depending on the database system it works with. Once connected, however, you can issue the same SQL queries.


### Apache Spark

Verdict works with Spark by creating Spark's HiveContext internally. In this way, Verdict can load persisted tables through Hive Metastore. Verdict is tested with Apache Spark 1.6.0 (in the Cloudera distribution CDH 5.11). We will support Spark 2.0 shortly.

We show how to use Verdict in `spark-shell` and `pyspark`. Using Verdict in a Spark application written either in Scala or Python is the same.

Due to the seamless integration of Verdict on top of Spark (and PySpark), Verdict can be used within [Apache Zeppelin](https://zeppelin.apache.org/) notebooks and Python [Jupyter](http://jupyter.org/) notebooks. See this page for more detail about how to set up Verdict with Zeppelin or Jupyter.


#### (Scala) Spark

You can start `spark-shell` with Verdict as follows.

```bash
$ spark-shell --jars {{ site.verdict_core_jar_name }}
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


#### PySpark

You can start `pyspark` shell with Verdict as follows.

```bash
$ export PYTHONPATH=$(pwd)/python:$PYTHONPATH

$ pyspark --driver-class-path {{ site.verdict_core_jar_name }}
```

**Limitation**: Note that, for the `--driver-class-path` option to work, the jar file (i.e., `{{ site.verdict_core_jar_name }}`) must be placed in the Spark's driver node. `--jars` option can be used for Spark 2.0 or later.

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


### Impala, Hive, Redshift

We will use our command line interface (which is called `verdict-shell`) for connecting to those databases. You can also programmatically connect to Verdict (see [this page](http://verdict-doc.readthedocs.io/en/latest/using.html#jdbc-in-java-python-applications)).

#### Prerequisites

`verdict-shell` relies on the JDBC drivers provided by individual database vendors; thus, if your database is not compatible with the drivers packaged in {{ site.verdict_command_line_zip_name }}, `verdict-shell` will not be able to make a connection to your database. In this case, please contact our team for support. We will add a right set of JDBC drivers promptly.


#### Impala

Type the following command in terminal to launch `verdict-shell` that connects to Impala.

```bash
$ bin/verdict-shell -h "impala://hostname:port/schema;key1=value1;key2=value2;..." -u username -p password
```

Note that parameters are delimited using semicolons (`;`). The connection string is quoted since the semicolons have special meaning in bash. The user name and password can be passed in the connection string as parameters, too.

Verdict supports the Kerberos connection. For this, add `principal=user/host@domain` as one of those key-values pairs.

After `verdict-shell` launches, you can issue regular SQL queries as follows.

```
verdict:impala> show databases;

// Creates samples for the table. This step needs to be done only once for the table.
verdict:impala> create sample of database_name.table_name;

verdict:impala> select count(*) from database_name.table_name;

verdict:impala> !quit
```

#### Hive

Type the following command in terminal to launch `verdict-shell` that connects to Hive.

```bash
$ bin/verdict-shell -h "hive2://hostname:port/schema;key1=value1;key2=value2;..." -u username -p password
```

Note that parameters are delimited using semicolons (`;`). The connection string is quoted since the semicolons have special meaning in bash. The user name and password can be passed in the connection string as parameters, too.

Verdict supports the Kerberos connection. For this, add `principal=user/host@domain` as one of those key-values pairs.

After `verdict-shell` launches, you can issue regular SQL queries as follows.

```
verdict:Apache Hive> show databases;

// Creates samples for the table. This step needs to be done only once for the table.
verdict:Apache Hive> create sample of database_name.table_name;

verdict:Apache Hive> select count(*) from database_name.table_name;

verdict:Apache Hive> !quit
```

#### Redshift

Type the following command in terminal to launch `verdict-shell` that connects to Amazon Redshift.

```bash
$ bin/verdict-shell -h "redshift://endpoint:port/database;key1=value1;key2=value2;..." -u username -p password
```

Note that parameters are delimited using semicolons (`;`). The connection string is quoted since the semicolons have special meaning in bash. The user name and password can be passed in the connection string as parameters, too.

After `verdict-shell` launches, you can issue regular SQL queries as follows.

```
// In Redshift, this displays the schemas in the database to which you are connected
verdict:PostgreSQL> show databases;

// Creates samples for the table. This step needs to be done only once for the table.
verdict:PostgreSQL> create sample of schema_name.table_name;

verdict:PostgreSQL> select count(*) from schema_name.table_name;

verdict:PostgreSQL> !quit
```

The [search path](http://docs.aws.amazon.com/redshift/latest/dg/r_search_path.html) can be set by `use schema_name;` statement. Currently, only a single schema name can be set for the search path using the `use` statement.


## What's Next

See what types of queries are supported by Verdict in [**this page**](http://verdictdb.org), and enjoy the speedup provided Verdict for those queries.

Learn in [**this page**]() how to quickly visualize your query answers using Verdict in [Apache Zeppelin](https://zeppelin.apache.org/) or [Jupyter](http://jupyter.org/).

If you have use cases that are not supported by Verdict, please contact us at `verdict-user@umich.edu`, or create an issue in our Github repository. We will answer your questions or requests shortly (at most in a few days).
