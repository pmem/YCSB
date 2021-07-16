<!--
Copyright (c) 2015 - 2021 YCSB contributors. All rights reserved.

Licensed under the Apache License, Version 2.0 (the "License"); you
may not use this file except in compliance with the License. You
may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
implied. See the License for the specific language governing
permissions and limitations under the License. See accompanying
LICENSE file.
-->

# PmemKV Driver for YCSB
This driver is a binding for the YCSB facilities to operate against a [PmemKV](https://github.com/pmem/pmemkv).
It uses the PmemKV Java bindings.

## Quick Start

### 0. Install PmemKV Java Binding
**Optionally** you can compile and install custom version of PmemKV Java binding.
The min. supported version is `1.2.0`.

>Note: If you want to use custom installation, you'll have to set additional
>maven parameter for building/execution of **PmemKV module**: `-Dpmemkv.packageName=pmemkv`.

Simple follow [PmemKV Java installation instruction](https://github.com/pmem/pmemkv-java#installation),
including at least:

    export JAVA_HOME=#PATH_TO_YOUR_JAVA_HOME
    git clone https://github.com/pmem/pmemkv-java.git
    cd pmemkv-java
    mvn install

### 1. Set Up YCSB
You need to clone the repository and compile **PmemKV module**.

    git clone git://github.com/brianfrankcooper/YCSB.git
    cd YCSB
    mvn -pl site.ycsb:pmemkv-binding -am package

Optionally, you can use specific pmemkv version, by adding extra maven parameter: `-Dpmemkv.packageVersion=X.Y.Z`

### 2. Run the Workload
Before you can actually run the workload, you need to "load" the data first.

    bin/ycsb.sh load pmemkv -P workloads/workloada -p pmemkv.engine=cmap -p pmemkv.dbsize=DB_SIZE -p pmemkv.dbpath=/path/to/pmem/pool

Then, you can run the workload:

    bin/ycsb.sh run pmemkv -P workloads/workloada -p pmemkv.engine=cmap -p pmemkv.dbsize=DB_SIZE -p pmemkv.dbpath=/path/to/pmem/pool

XXX extra params setup XXX

## Configuration Options
Driver has a few configuration options to parametrize engine, path, and size using the following:

| Parameter             | Meaning                    | Obligatory |
| :-------------------: | -------------------------- | :--------: |
| pmemkv.engine         | Storage engine             | N          |
| pmemkv.dbpath         | Pool file path             | Y          |
| pmemkv.dbsize         | Pool file size             | Y          |
| pmemkv.jsonconfigfile | Extra engine config params | N          |

To check possible values for storage engine see
[pmemkv's documentation](https://github.com/pmem/pmemkv#storage-engines).
The default engine used in YCSB (if not defined otherwise) is cmap.
Please take into consideration each engine may require different path and size
setting - as described in the mentioned documentation.
