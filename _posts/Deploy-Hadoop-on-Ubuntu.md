---
title: Deploy Hadoop on Ubuntu
date: 2023-04-03 18:04:34
tags:
    - Ubuntu
    - Hadoop
declare: true
---
- [Hadoop-3.3.1 Installation guide for Ubuntu](https://blog.devgenius.io/install-configure-and-setup-hadoop-in-ubuntu-a3cdd6305a0e)<!--more-->
- [Hadoop: Setting up a Single Node Cluster.](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html#Fully-Distributed_Operation)
- [Hadoop Cluster Setup](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/ClusterSetup.html)

--------------------

1. If you have installed Hadoop on your own Ubuntu system, then the default file system location of HDFS is **/usr/local/hadoop/hdfs/namenode**. This is the default configuration of Hadoop, but you can change it in the **hdfs-site.xml** file. In this file, you can specify the NameNode's data directory using the `dfs.namenode.name.dir` property.

If you are unsure of the location of the HDFS file system, you can check the **core-site.xml** file in the Hadoop configuration file, which contains Hadoop's core configuration information. You can use the following command to find the location of the **core-site.xml** file:
`find / -name "core-site.xml" 2> /dev/null`

This will search the entire filesystem for a file named **core-site.xml** and output the results to the console. Once you have located this file, you can open it and view the configuration information in it to determine the HDFS filesystem location.

2. You can retrieve data from HDFS using the following command:
`hdfs dfs -get <source> <destination>`

Among them, **<source>** is the source path in HDFS, and **<destination>** is the destination path in the local system. This command will get the specified file or directory from HDFS and copy it to the target path in the local system.

For example, if you want to retrieve and copy **/hdfs/data/file.txt** from HDFS to **/local/data/** directory on your local system, you can use the following command:
`hdfs dfs -get /hdfs/data/file.txt /local/data/`

If you want to retrieve and copy an entire directory from HDFS to a target directory on your local system, you can use the following command:
`hdfs dfs -get /hdfs/data/ /local/data/`

Note that this command will fail if a file or directory with the same name already exists in the target path. You can force overwrite existing files with the -f option:
`hdfs dfs -get -f /hdfs/data/file.txt /local/data/`
This will force overwrite the existing file in **/local/data/file.txt**.

3. You can delete a specified file or directory from Hadoop using the following command:
`hdfs dfs -rm -r <path>`
Among them, **<path>** is the path of the file or directory to be deleted in Hadoop. If you want to delete a single file, specify **<path>** as the full path to the file. If you want to delete a directory, specify **<path>** as the full path to the directory.

For example, to delete **/hdfs/data/file.txt** from Hadoop, you can use the following command:
`hdfs dfs -rm /hdfs/data/file.txt`

If you want to delete the directory **/hdfs/data/** and all files and subdirectories under it, use the following command:
`hdfs dfs -rm -r /hdfs/data/`
Note that deleting a file or directory with this command is irreversible, so proceed with caution and make sure you are deleting the correct file or directory.


-----------------
1. Create a directory to store your photos in HDFS. You can create a directory with
`hdfs dfs -mkdir /photos`
This will create a directory called **photos** under the root directory.

2. Upload your photo to Hadoop. You can upload local files to HDFS with the following command:
`hdfs dfs -put /local/path/to/photos/* /photos/`
This will upload all photos under your local path **/local/path/to/photos/** to **/photos/** directory.

3. Confirm that the photos have been successfully uploaded to Hadoop. You can use the following command to view your uploaded photos:
`hdfs dfs -ls /photos/`
This will list all files under the **/photos/** directory.
 
Now you have successfully uploaded photos to Hadoop. If you need to further process these photos, you can use distributed processing frameworks such as Hadoop MapReduce or Spark to process them.


-------------------
1. Create a new user
`sudo adduser userNameHere`
Where userNameHere is the username

2. The new user will be added to the **/etc/passwd** file, and the encrypted password will be stored in the **/etc/shadow** file

3. Switch user
`su -userNameHere`
Where userNameHere is the username you want to switch to.

4. Delete user
`sudo deluser --remove-home userNameHere`

5. Verify that a user has been deleted
`id userNameHere`
If the user does not exist, `No such user` will be displayed

6. Delete the error solution
- Use the following command to find out the processes using this user: `sudo lsof -u test`.
- Check if the process can be killed, and if so, kill the process with: `sudo kill -9 <PID> (where "<PID>" is the PID of the process to be killed)`
- The user can then be forcibly removed, including its home directory, with: `sudo deluser --remove-home test`

