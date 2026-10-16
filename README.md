# Hadoop 1.2.1 — Single Node Installation

This repository contains a step-by-step guide to installing and configuring Apache Hadoop 1.2.1 on Ubuntu in pseudo-distributed mode.

## Topics Covered

- Java 8 installation
- SSH passwordless login
- Hadoop installation and environment variables
- HDFS configuration
- MapReduce configuration
- NameNode formatting
- Hadoop daemon startup and verification
- Troubleshooting

## Installation Guide

Follow the complete installation instructions here:

**[Hadoop 1.2.1 Single-Node Installation Guide](Hadoop-1.2.1-Single-Node-Installation.md)**

## Verify Hadoop Services

After starting Hadoop, execute:

```bash
jps
```

Expected Hadoop daemons:

- NameNode
- DataNode
- SecondaryNameNode
- JobTracker
- TaskTracker

The `Jps` process may also appear in the output.

## Important Notes

- Hadoop 1.2.1 is a legacy release intended for learning and compatibility testing.
- Java 8 compatibility depends on your Ubuntu environment.
- Configure the correct `JAVA_HOME` path.
- Format the NameNode only when initializing a new HDFS namespace.
- Do not expose Hadoop service ports publicly.

