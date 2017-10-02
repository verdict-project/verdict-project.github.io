---
layout: post
title: Download and Installation
---

* TOC
{:toc}

Download relevant jar or archive files depending on your use cases. You can also build Verdict from our source code using [Apache Maven](https://maven.apache.org/).

Verdict does not require any separate steps for installation. Simply placing those downloaded files in any of your local file system is enough for starting to use Verdict.

All the jar files provided below are compiled using Oracle JDK 8.


## Apache Spark

Download the below jar file and [include it in Spark applications](https://spark.apache.org/docs/latest/submitting-applications.html#advanced-dependency-management) (using `--jars` option). Our [Quick Start Guide]({{ site.baseurl }}/documentation/quick_start/) provides more instructions.

{% for item in site.verdict_spark %}
    {% assign platform = item[0] %}
    {% assign name_url = item[1] %}
    {% assign name = name_url['name'] %}
    {% assign url = name_url['url'] %}
**Download for {{ name_url['family'] }} - {{ platform }}**: [{{ name_url['name'] }}]({{ name_url['url'] }})
{% endfor %}

Note that `2.10` and `2.11` suffixed to the official Spark versions are the Scala versions.


## Hive, Impala, Redshift: Command-line Interface

Download the below zip archive, extract it, and start an interactive command-line interface as described in [this document]({{ site.baseurl }}/documentation/quick_start/#on-apache-impala-apache-hive-amazon-redshift). [CDH](https://www.cloudera.com/products/open-source/apache-hadoop/key-cdh-components.html) is Cloudera's distribution including Hive and Impala.

{% for item in site.verdict_shell %}
    {% assign platform = item[0] %}
    {% assign name_url = item[1] %}
    {% assign name = name_url['name'] %}
    {% assign url = name_url['url'] %}
**Download for {{ name_url['family'] }} - {{ platform }}**: [{{ name_url['name'] }}]({{ name_url['url'] }})
{% endfor %}

<!-- **Download for %**: [{{ site.verdict_command_line_zip_name }}]({{ site.verdict_command_line_zip_url }}) -->

Note that the above jar file contains third-party JDBC drivers, i.e., JDBC drivers for [Apache Hive](https://www.cloudera.com/downloads/connectors/hive/jdbc/2-5-4.html), [Apache Impala](https://www.cloudera.com/downloads/connectors/impala/jdbc/2-5-41.html), [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html#download-jdbc-driver), etc., without any modifications. The sole purpose of including those JDBC drivers in the above jar file is for the convenience of the existing users of those systems, and Verdict does not use any part of those JDBC drivers in its codebase.


## Hive, Impala, Redshift: JDBC Driver

You can load below jar file in your Java applications in [a standard way](https://www.tutorialspoint.com/jdbc/jdbc-sample-code.htm). Simply use instead our JDBC driver class name as described [here](http://verdict-doc.readthedocs.io/en/latest/using.html#jdbc-in-java-python-applications). [CDH](https://www.cloudera.com/products/open-source/apache-hadoop/key-cdh-components.html) is Cloudera's distribution including Hive and Impala.

{% for item in site.verdict_jdbc %}
    {% assign platform = item[0] %}
    {% assign name_url = item[1] %}
**Download for {{ name_url['family'] }} - {{ platform }}**: [{{ name_url['name'] }}]({{ name_url['url'] }})
{% endfor %}

Note that the above jar file contains third-party JDBC drivers, i.e., JDBC drivers for [Apache Hive](https://www.cloudera.com/downloads/connectors/hive/jdbc/2-5-4.html), [Apache Impala](https://www.cloudera.com/downloads/connectors/impala/jdbc/2-5-41.html), [Amazon Redshift](http://docs.aws.amazon.com/redshift/latest/mgmt/configure-jdbc-connection.html#download-jdbc-driver), etc., without any modifications. The sole purpose of including those JDBC drivers in the above jar file is for the convenience of the existing users of those systems, and Verdict does not use any part of those JDBC drivers in its codebase.

## Build from source

Clone our Github repository or download an archive of it latest source code. Verdict is officially supported and tested for Oracle JDK 7 and above; however, we have not experienced any issues in Open JDK 7 and above as well. 

1. **Download**: [Github snapshot](https://github.com/mozafari/verdict/archive/master.zip)
1. **Visit**: [Github repository](https://github.com/mozafari/verdict)

Compile the source code using either of the following commands depending on your platform:
{% assign platforms = "" %}
{% for item in site.verdict_spark %}
    {% assign platforms = platforms | append: "," | append: item[0] %}
{% endfor %}
{% for item in site.verdict_jdbc %}
    {% assign platforms = platforms | append: "," | append: item[0] %}
{% endfor %}
{% for item in site.verdict_shell %}
    {% assign platforms = platforms | append: "," | append: item[0] %}
{% endfor %}
{% assign platforms = platforms | remove_first: "," | split: "," | uniq %}
{% for p in platforms %}
1. {{ p }}: `mvn package -P{{ p }}`

{% endfor %}

The above commands will generate up to three `jar` files under the `jars` directory in the following patterns:
1. verdict-spark-lib-(platform)-(version).jar
1. verdict-jdbc-(platform)-(version).jar
1. verdict-shell-(version).jar

The first jar file is used for Spark. The second jar file is Verdict's JDBC driver. The third jar file is used (in conjunction with the second jar file) for Verdict's command line interface.

