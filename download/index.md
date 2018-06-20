---
layout: post
title: Download and Installation
---

* TOC
{:toc}

Download relevant jar or archive files depending on your use cases. You can also build Verdict from our source code using [Apache Maven](https://maven.apache.org/).

Verdict does not require any separate steps for installation. Simply placing those downloaded files in any of your local file system is enough to use Verdict.

All the jar files provided below are compiled using Oracle JDK 8.


## Main Package

You can load below jar file in your Java applications in [a standard way](https://www.tutorialspoint.com/jdbc/jdbc-sample-code.htm). Simply use instead our JDBC driver class name as described [here](http://verdict-doc.readthedocs.io/en/latest/using.html#jdbc-in-java-python-applications). [CDH](https://www.cloudera.com/products/open-source/apache-hadoop/key-cdh-components.html) is Cloudera's distribution including Hive and Impala.

{% assign platform = site.verdict_jdbc['family'] %}
{% assign name = site.verdict_jdbc['name'] %}
{% assign url = site.verdict_jdbc['url'] %}
**Download {{ platform }}**: [{{ name }}]({{ url }})

Note that the above jar file does **NOT** contain third-party JDBC drivers, i.e., JDBC drivers for [Apache Hive](https://www.cloudera.com/downloads/connectors/hive/jdbc/2-5-4.html), [Apache Impala](https://www.cloudera.com/downloads/connectors/impala/jdbc/2-5-41.html), [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html#download-jdbc-driver),
[Postgresql](https://jdbc.postgresql.org/download.html),
etc. 

**Important:** You must place JDBC jars files for your database in the `jdbc_jars` directory.

## Command-line Interface

Download the main package and below jar file, and start an interactive command-line interface as described in [this document]({{ site.baseurl }}/documentation/quick_start/#on-apache-impala-apache-hive-amazon-redshift). [CDH](https://www.cloudera.com/products/open-source/apache-hadoop/key-cdh-components.html) is Cloudera's distribution including Hive and Impala. 

{% assign platform = site.verdict_shell['family'] %}
{% assign name = site.verdict_shell['name'] %}
{% assign url = site.verdict_shell['url'] %}
**Download {{ platform }}**: [{{ name }}]({{ url }})

## Build from source

Clone our Github repository or download an archive of it latest source code. Verdict is officially supported and tested for Oracle JDK 7 and above.

1. **Download**: [Github snapshot](https://github.com/mozafari/verdict/archive/master.zip)
1. **Visit**: [Github repository](https://github.com/mozafari/verdict)

Compile the source code using the following command:
`mvn package`


The above commands will generate up to three `jar` files under the `jars` directory in the following patterns:

1. verdict-spark-lib-(version).jar
1. verdict-jdbc-(version).jar
1. verdict-shell-(version).jar

The first jar file is used for Spark. The second jar file is Verdict's JDBC driver. The third jar file is used (in conjunction with the second jar file) for Verdict's command line interface.

