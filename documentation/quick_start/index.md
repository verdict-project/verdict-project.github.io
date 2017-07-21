---
layout: post
---

{:.first}
# Quick Start

* TOC
{:toc}

In this guide, we will download Verdict, and issue SQL queries through Verdict. The workflow is identical for the database systems that support JDBC connections. Verdict takes a slightly different approach for Spark SQL due to its architectural uniqueness.

## Impala, Hive, Redshift

### Step 1. Downloading a JDBC driver

Download [this JAR file]({{ site.core_jar_link }}) to use Verdict on top of the database systems that support JDBC connections. The downloaded JAR file should be placed in the client machine (just like other JDBC drivers).

1. **The class name for Verdict's JDBC driver**: `edu.umich.verdict.jdbc.Driver`.

1. **JDBC connection string**
  * **Impala**: `jdbc:verdict:impala://hostname:port/schema?key1=param1&key2=param2&...`.
  * **Hive**: `jdbc:verdict:hive://hostname:port/schema?key1=param1&key2=param2&...`.
  * **Redshift**: `jdbc:verdict:redshift://hostname:port/schema?key1=param1&key2=param2&...`.

See that the only difference among different databases is the database name that comes after `verdict:`. One can connect to Verdict from a standard Java application using the information above.

Verdict does not support other databases at the moment due to the variations in SQL expressions (supported by different databases). If you are using a database system other than the ones listed above and want to use Verdict on top of it, please contact us, so we can add a driver for the database system.

For convenience, Verdict ships with its own command line interface. See below for it.


### Step 2. (Optional) Starting a command line interface

Verdict ships with a command line interface, `veeline` (which is a modified version of `sqlLine`). Start `veeline` as follows:

```bash
$ bin/veeline -h "impala://hostname:port/schema?key1=param1&key2=param2&..." -u username -p password
```

The connection string must be double-quoted to include multiple parameters, since `&` has a special meaning in the bash shell. After the connection is successfully made, regular SQL queries can be issued in `veeline`. The username and password can be passed within the connection string instead of specifiying them as separate parameters.

Note that `veeline` makes JDBC connections using the target JDBC drivers stored in `lib` directory. The JDBC drivers that ships with Verdict is the drivers for the Cloudera distribution. If those default JDBC drivers are not compatible with your environment, simply replace those existing JAR files in the `lib` directory with the the JAR files for your environment.


### Step 3. Running queries

Verdict processes standard SQL queries.

```sql
veeline:impala> show databases;

veeline:impala> use mydb;

veeline:impala> create sample of mytable;

-- Verdict automatically speeds up this query using available samples.
veeline:impala> select count(*) from mytable;
```

For more details on the queries Verdict supports, see this page.


## Spark

### Step 1. Downloading a JAR

Download [this JAR file]({{ site.jdbc_jar_link }}) to use Verdict on top of Spark's HiveContext.

### Step 2. Submitting an application

Include Verdict's JAR as submitting a Spark application. Below is an example with the Spark shell.

```scala
$ spark-shell --jars {{ site.verdict_core_jar }}

// sc is a SparkContext instance.
// Verdict internally creates an instance of HiveContext.
spark> val vc = new edu.umich.verdict.VerdictSparkHiveContext(sc)

spark> vc.sql("show databases").show(false)

spark> vc.sql("use mydb").show(false)

// created samples are materialized using Spark's HiveContext.
// samples need to be created only once.
spark> vc.sql("create sample of mytable").show(false)

// Verdict automatically speeds up this query using available samples.
spark> vc.sql("select count(*) from mytable").show(false)
```

See [this page]({{ site.baseurl }}/documentation/connecting) for more details on using Verdict on top of Spark and **PySpark**. The integration with Jupyter is also described in the page.
