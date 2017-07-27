---
layout: post
---

{:.first}
# Configurations

* TOC
{:toc}

## Changing Verdict configuration

1. Creating a configuration file (i.e., `verdict.properties`) in the `config` directory. In order for the options in this file to be effective, Verdict should be build again using a `mvn package` command.
1. Setting environment variables: When Verdict's Java instance is created, it scans all environment variables and stores them in Verdict's configuration if they start with `verdict`. For setting environment variables, periods (`.`) in option keys must be replaced with underscores (`_`). For example, `verdict.loglevel` option can be set by `verdict_loglevel=debug`.
1. Passing parameters when making a connection
   * Spark: You can pass parameters using the `--conf` argument. For example
   ```bash
   $ spark-shell --jars target/{{ site.verdict_core_jar }} --conf verdict.loglevel=warn
   ```
   will set the Verdict's log level to `warn`.
   * Hive, Impala: You can pass the parameters in the JDBC connection string. For example
   ```bash
   $ veeline/bin/veeline -h "impala://host:port/default;verdict.loglevel=warn"
   ```
   will set the Verdict's log level to `warn`.
1. Using a Verdict query: You can issue a `set key=value` query after a Verdict instance is created.

The higher-numbered items take higher precedences.


## Verdict configuration options

### Basic options

| Key                          | Default values | Description |
|------------------------------|----------------|-------------|
| `verdict.bypass`             | false          | If set true, Verdict simply passes all the query to the underlying database and returns the result to the user without any modifications. The only exception is `set bypass=false`. |
| `verdict.loglevel`             | info           | Determines the log level; should be one of "debug", "info", "warn", "error". `loglevel` is a synonym for `verdict.loglevel`. |
| `verdict.relative_target_cost` | 10%            | Keeps the cost of Verdict's query processing under a certain level. Verdict chooses a set of the largest samples under this limit. |

### JDBC options

| Key                     | Default values | Description |
|-------------------------|----------------|-------------|
| `verdict.jdbc.user`     |                | If set, pass as a "user" value when making a JDBC connection to an underlying database. |
| `verdict.jdbc.password` |                | If set, pass as a "password" value when making a JDBC connection to an underlying database. |
| `verdict.jdbc.kerberos_principal` | n/a  | If set, makes an appropriate Kerberos authentication when making a JDBC connection to an underlying database. `principal` is a synonym for `verdict.jdbc.kerberos_principal`. |
| `verdict.jdbc.ignore_user_credentials` | false  | If set to true, "user" and "password" values are not passed even if they are set in `verdict.jdbc.user` and `verdict.jdbc.password`. |
| `verdict.jdbc.dbname`   |                | One of "impala", "hive2". The database name specified in the JDBC connection string is set. |
| `verdict.jdbc.host`     | 127.0.0.1      | The host that will be used for a JDBC connection. |
| `verdict.jdbc.port`     |                | The port number that will be used for a JDBC connection. |
| `verdict.jdbc.schema`   | default        | The default schema (or database) that a JDBC connection will connect to. |
| `verdict.jdbc.hive2.default_port` | 10000  | The default port number that will be used for a hive2 connection if the port number is not explicitly set in the passed JDBC connection string. |
| `verdict.jdbc.hive2.class_name`   | com.cloudera.hive.jdbc4.HS2Driver | The class name of the JDBC driver that will be used for a hive2 connection. |
| `verdict.jdbc.impala.default_port` | 21050 | The default port number that will be used for an impala connection if the port number is not explicitly set in the passed JDBC connection string. |
| `verdict.jdbc.impala.class_name`  | com.cloudera.impala.jdbc4.Driver | The class name of the JDBC driver that will be used for an impala connection. |


### Spark options

| Key                     | Default values | Description |
|-------------------------|----------------|-------------|
| `verdict.spark.cache_samples` | true    | (Lazily) cache the sample tables created by Verdict using Spark's `DataFrame#cache()` function. The sample tables are typically significantly smaller than the original tables. Caching sample tables will lower query response times. |


### Metadata (and Verdict samples) options

| Key                     | Default values | Description |
|-------------------------|----------------|-------------|
| `verdict.meta_data.meta_database_suffix` | _verdict | When set to a non-empty string, Verdict creates a separate database (or called a schema or catalog in some systems) for storing its sample tables and related metadata. If the original database's name is "default", all the sample tables for that database are stored in the database named "default_verdict". The user must have the privilege to create a new database unless the database already exists. Also, the user who wants to create samples must have the privilege to create new tables in the database. |
| `verdict.meta_data.meta_name_table` | verdict_meta_name | A table for storing the names of the sample tables. |
| `verdict.meta_data.meta_size_table` | verdict_meta_size | A table for storing the sizes of the sample tables. |
| `verdict.meta_data.refresh_policy`  | per_session       | How often to refresh metadata. One of "per_session", "per_query", "manual". If set to "manual", "refresh" query must be used for updating metadata. |


### Error bound options

| Key                     | Default values | Description |
|-------------------------|----------------|-------------|
| `verdict.error_bound.confidence_internal_probability`  | 95%  | The probability with which Verdict's error bounds hold. Should be one of 80%, 85%, 90%, 95%, 99%, 99.5%, 99.9%. |
| `verdict.error_bound.error_column_pattern`  | err bound of %s | Column names for error bounds. Must include a single "%s".
| `verdict.error_bound.method`                  | subsampling     | A method for error bound computation. One of "subsampling", "bootstrapping" (only for research), "analytic", "no_error_bound". |
| `verdict.error_bound.subsampling.partition_column` | __vpart    | A column name internally used by subsampling for partitioning tuples. |
| `verdict.error_bound.subsampling.probability_column` | __vprob  | A column name internally used by subsampling for sampling probabilities. |
| `verdict.error_bound.subsampling.partition_count`  | 100   | The number of partitions used by subsampling for computing error bounds. |
| `verdict.error_bound.bootstrapping.random_value_column_name` | verdict_rand | only for research |
| `verdict.error_bound.bootstrapping.bootstrap_multiplicity_colname` | __mul | only for research |
| `verdict.error_bound.bootstrapping.num_of_trials` | 100 | only for research |

Note that changing subsampling-related parameters affect sample creation process.


## Passing database-specific options through Verdict

### Verdict-on-Spark

Simply launch Spark (or PySpark) as usual. Verdict runs as an imported module on Spark; thus, it does not affect Spark's regular interface.

### Verdict-on-{Hive, Impala}

Specify the parameters in the JDBC connection string. Or, if you are programmatically connecting to Verdict using the JDBC interface, you can pass `java.util.Properties` as an optional argument. For example, the following command will set your Hive applications to use a specific job queue.

```bash
$ veeline/bin/veeline -h "hive2://host:port/default;mapred.job.queue.name=root.example_queue"
```

Verdict passes all key-value parameters (except for the ones that start with `verdict`) when it makes an internal JDBC connection to the database it works with.
