---
layout: post
comments: true
title: Learning The Setup of Hadoop Cluster and Maybe Spark
mathjax: true
excerpt: "Quite exciting to go through setting up a cluser by myself."
---
I am trying to setup a big data cluster by myself. The plan is to setup a HDFS and hadoop cluster (2 machines) and play with it a little bit and then go to more softwares like Spark. The setup was from scratch!!

1. Hadoop and HDFS setup [Mar-07]
 - Install JDK and download hadoop tar file from apache
 - Create user:hduser and group hadoop
 - Untar the hadoop tarball to /usr/local and make soft links to /usr/local/hadoop
 - Setup ssh and copy your public key to authroized_keys file under .ssh directory

2. Cluster configuration [Mar-08]
 - [some hdfs question from stackoverflow](http://stackoverflow.com/questions/22565200/where-hdfs-stores-data)
 - [official doc ](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html)
 - [some nice doc](https://www.systutorials.com/239637/hadoop-installation-tutorial-hadoop-2-x/)

Right now these configuration looks easy to me. But I just don't have energy to actually try it. I will try sometime this week.
Downloaded hadoop project's source code.
 - Protobuf is really basic to be used in remote procedure calls and msg communication. Need to master that.
 -
