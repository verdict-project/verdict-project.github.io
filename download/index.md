---
layout: post
title: Download and Installation
---

* TOC
{:toc}

Use a different interface depending on your language preference (Java or Python).


## Java

One of the following three methods can be used:

1. Maven
1. Using a pre-compiled jar
1. Build yourself

### Maven

If you use [Apache Maven](https://maven.apache.org/) for your project's dependency management, adding the following dependency entry to your `pom.xml` is all you need to do to use VerdictDB. For all available versions, visit [this page](https://mvnrepository.com/artifact/org.verdictdb/verdictdb-core).

```pom
<dependency>
    <groupId>org.verdictdb</groupId>
    <artifactId>verdictdb-core</artifactId>
    <version>0.5.6</version>
</dependency>

```

### Download a Pre-compiled Jar

You only need a single jar file. This jar file is compiled with JDK8.

**Download**: [verdictdb-core-0.5.4-jar-with-dependencies.jar](https://github.com/mozafari/verdictdb/releases/download/v0.5.4/verdictdb-core-0.5.4-jar-with-dependencies.jar)


### Build Yourself

1. **Clone** from our [Github public repository](https://github.com/mozafari/verdictdb). Use command
    ```
    git clone https://github.com/mozafari/verdictdb.git
    ```
2. **Change** directory to the directory to the repository you have cloned. Use command,
    ```
    cd verdictdb
    ```

3. **Build** the jar file by maven. Use command
    ```
    mvn -DskipTests -DtestPhase=false -DpackagePhase=true clean package
    ```
    Check the `target` directory for the created jar files.

    Since Maven requires JDK. Please make sure you have installed JDK. You can use command
    ```
    javac -version
    ```
    to check your java compiler version. The output should look like this
    ```
    $ javac -version
    javac 1.8.0_171-1-ojdkbuild
    ```
    If not, please install JDK first and then use maven to build the file.


## Python (pyverdict)

PyVerdict (or pyverdict) is a Python interface to VerdictDB (which itself is implemented in Java). At the moment, pyverdict supports MySQL and Presto. 

### Requirements

pyverdict is developed and tested on [miniconda for Python 3.7](https://conda.io/docs/user-guide/install/index.html).


### Installation

PyVerdict is available from PyPI (with project name `pyverdict`); thus, it can be installed as follows:

```
pip install pyverdict --upgrade
```

Note that pyverdict already ships a latest VerdictDB jar in it; thus, no separate installation is necessary.


### Build Yourself (for developments)

1. **Clone** from our [Github public repository](https://github.com/mozafari/verdictdb). Use command
    ```
    git clone https://github.com/mozafari/verdictdb.git
    ```
2. **Change** directory to the `pyverdict` directory to the repository you have cloned. Use command,
    ```
    cd verdictdb/pyverdict
    ```

3. **Build** the `pyverdict` for continuous developments with
    ```
    python setup.py develop
    ```
    which can be uninstalled by
    ```
    python setup.py develop --uninstall
    ```


