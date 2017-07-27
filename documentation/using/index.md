---
layout: post
---

{:.first}
# Using Verdict

* TOC
{:toc}

## In terminal

Please see our [Quick Start Guide]({{ site.baseurl }}/documentation/quick_start) for the instructions on connecting to Verdict in terminal. The page includes starting Verdict on top of Apache Hive, Apache Impala, and Apache Spark (and PySpark) in terminal.


## JDBC in Java/Python applications

Verdict ships with a JDBC driver. Using the driver, you can use Verdict on top of your existing JDBC-supported database systems. Currently, Verdict supports the JDBC connections to Apache Hive and Apache Impala. Contact us if you want to use Verdict on other database systems. We need to add a small driver (due to non-standard SQL features used by different databases).

1. Class name for Verdict's JDBC driver: `edu.umich.verdict.jdbc.Driver`
1. Connection string for Verdict's JDBC driver:
   * Hive:   `jdbc:verdict:hive2://host:port/default_database;key1=value1;key2=value2;...`
   * Impala: `jdbc:verdict:impala://host:port/default_database;key1=value1;key2=value2;...`

To enable the Kerberos authentication, add `principal=user/host@domain` pair in the key-value pairs of the JDBC connection string. You can also pass Verdict configuration options in key-value pairs. For example, to change Verdict's log level to `DEBUG`, add `verdict.loglevel=debug` in the key-value pairs. See [this page]({{ site.baseurl }}/documentation/configurations) for more configuration options for Verdict.

#### Details

The rule for Verdict's connection string is to include the `verdict:` after 'jdbc:'. Internally, Verdict communicates with the target database (e.g., Hive, Impala, etc.) using a JDBC connection as well. For this, Verdict uses the passed JDBC connection string after removing (1) the `verdict:` keyword and (2) configuration options for Verdict. This means you can pass any existing JDBC options to Verdict as they are.

### Java example

Below example connects to Verdict-on-Impala and runs a simple `count` query.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class VerdictJdbcExample {
  public static void main(String[] args) throws ClassNotFoundException, SQLException {
    Class.forName("edu.umich.verdict.jdbc.Driver");
    String url = "jdbc:verdict:impala://host:port/default";
    Connection conn = DriverManager.getConnection(url);
    Statement stmt = conn.createStatement();
    String sql2 = "select count(*) from orders";
    ResultSet rs = stmt.executeQuery(sql2);
  }
}
```

Of course, you should include the Verdict's JDBC driver (`target/{{ site.verdict_jdbc_jar }}`) in your Java class path when you compile and run the example.


### Python example


## In Apache Zeppelin

Apache Zeppelin is a notebook-based application that runs in a web browser. It can connect to Apache Spark by default and also to other database systems through the JDBC interface. Verdict can also easily integrate with Apache Zeppelin.

### On Spark

### On Hive, Impala


## In Jupyter

### On PySpark

### On Hive, Impala


## In Hue
