---
layout: post
title: Download and Installation
---

* TOC
{:toc}

Download relevant jar or archive files depending on your use cases. You can also build Verdict from our source code using [Apache Maven](https://maven.apache.org/).

Verdict does not require any separate steps for installation. Simply placing those downloaded files in any of your local file system is enough for starting to use Verdict.


## Apache Spark

Download the below jar file and [include it in Spark applications](https://spark.apache.org/docs/latest/submitting-applications.html#advanced-dependency-management) (using `--jars` option). Our [Quick Start Guide]({{ site.baseurl }}/documentation/quick_start/) provides more instructions.

**Download from SourceForge**: [{{ site.verdict_core_jar_name }}]({{ site.verdict_core_jar_url }})

This jar file can be used both for Spark 1.6.0 or later and for Spark 2.0.0 or later.


## Hive, Impala, Redshift: Command-line Interface

Download the below zip archive, extract it, and start an interactive command-line interface as described in [this document]({{ site.baseurl }}/documentation/quick_start/#on-apache-impala-apache-hive-amazon-redshift).

**Download from SourceForge**: [{{ site.verdict_command_line_zip_name }}]({{ site.verdict_command_line_zip_url }})

Note that the above jar file contains third-party JDBC drivers, i.e., JDBC drivers for [Apache Hive](https://www.cloudera.com/downloads/connectors/hive/jdbc/2-5-4.html), [Apache Impala](https://www.cloudera.com/downloads/connectors/impala/jdbc/2-5-41.html), [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html#download-jdbc-driver), etc., without any modifications. The sole purpose of including those JDBC drivers in the above jar file is for the convenience of the existing users of those systems, and Verdict does not use any part of those JDBC drivers in its codebase.


## Hive, Impala, Redshift: JDBC Driver

You can load below jar file in your Java applications in [a standard way](https://www.tutorialspoint.com/jdbc/jdbc-sample-code.htm). Simply use instead our JDBC driver class name as described [here](http://verdict-doc.readthedocs.io/en/latest/using.html#jdbc-in-java-python-applications).

**Download from SourceForge**: [{{ site.verdict_jdbc_jar_name }}]({{ site.verdict_jdbc_jar_url }})

Note that the above jar file contains third-party JDBC drivers, i.e., JDBC drivers for [Apache Hive](https://www.cloudera.com/downloads/connectors/hive/jdbc/2-5-4.html), [Apache Impala](https://www.cloudera.com/downloads/connectors/impala/jdbc/2-5-41.html), [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html#download-jdbc-driver), etc., without any modifications. The sole purpose of including those JDBC drivers in the above jar file is for the convenience of the existing users of those systems, and Verdict does not use any part of those JDBC drivers in its codebase.

## Build from source

Clone our Github repository or download an archive of it latest source code. Verdict is officially supported and tested for Oracle JDK 7 and above; however, we have not experienced any issues in Open JDK 7 and above as well.

1. **Download**: [Github snapshot](https://github.com/mozafari/verdict/archive/master.zip)
1. **Visit**: [Github repository](https://github.com/mozafari/verdict)

After downloading the source code (and unzipping it), go to the directory (e.g., `cd verdict`) and type `mvn package` for compiling the code. The command will generate three `jar` files under the `jars` directory with the names:
1. verdict-core-{{ site.version }}-jar-with-dependencies.jar
1. verdict-jdbc-{{ site.version }}-jar-with-dependencies.jar
1. verdict-veeline-{{ site.version }}-jar-with-dependencies.jar


