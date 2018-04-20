Asignatura Cloud Computing del Máster en Ingeniería Informática. 

Departamento de Ciencias de la Computación e Inteligencia Artificial.

Universidad de Granada.

<HR>

Profesor: **Manuel J. Parra-Royón**

Email: **manuelparra@decsai.ugr.es**

Tutorías: **Viernes, de 17:30 a 18:30, despacho D31 (4ª planta) Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT).**

Material de prácticas de la asignatura: **https://github.com/manuparra/PracticasCC**

<HR>

# Sesión : Introduction to BigData and HDFS

![LogoHadoop](https://media.licdn.com/mpr/mpr/shrinknp_400_400/AAEAAQAAAAAAAAmBAAAAJDgzYjM0N2VkLWQ0ZTEtNDQ0Zi05ZWEzLWVjOTgyMjdlN2Y0MA.jpg)

The Hadoop Distributed File System (HDFS) is a distributed file system designed to run on commodity hardware. It has many similarities with existing distributed file systems. However, the differences from other distributed file systems are significant. HDFS is highly fault-tolerant and is designed to be deployed on low-cost hardware. HDFS provides high throughput access to application data and is suitable for applications that have large data sets. HDFS relaxes a few POSIX requirements to enable streaming access to file system data. HDFS was originally built as infrastructure for the Apache Nutch web search engine project. HDFS is now an Apache Hadoop subproject. The project URL is http://hadoop.apache.org/hdfs/.

![HDFS](http://www.glennklockwood.com/data-intensive/hadoop/hdfs-magic.png)

HDFS has a master/slave architecture. An HDFS cluster consists of a single NameNode, a master server that manages the file system namespace and regulates access to files by clients. In addition, there are a number of DataNodes, usually one per node in the cluster, which manage storage attached to the nodes that they run on. HDFS exposes a file system namespace and allows user data to be stored in files. Internally, a file is split into one or more blocks and these blocks are stored in a set of DataNodes. The NameNode executes file system namespace operations like opening, closing, and renaming files and directories. It also determines the mapping of blocks to DataNodes. The DataNodes are responsible for serving read and write requests from the file system’s clients. The DataNodes also perform block creation, deletion, and replication upon instruction from the NameNode.


![HDFS Arch](https://hadoop.apache.org/docs/r1.2.1/images/hdfsarchitecture.gif)

**Data Replication**

HDFS is designed to reliably store very large files across machines in a large cluster. It stores each file as a sequence of blocks; all blocks in a file except the last block are the same size. The blocks of a file are replicated for fault tolerance. The block size and replication factor are configurable per file. An application can specify the number of replicas of a file. The replication factor can be specified at file creation time and can be changed later. Files in HDFS are write-once and have strictly one writer at any time.


![DataNodes](https://hadoop.apache.org/docs/r1.2.1/images/hdfsdatanodes.gif)




## Connecting to Hadoop Cluster UGR

Log in hadoop ugr server with your credentials (the same way):

```
ssh CC_XXXXXX@hadoop...
```

## HDFS basics

The management of the files in HDFS works in a different way of the files of the local system. The file system is stored in a special space for HDFS. The directory structure of HDFS is as follows:

```
/tmp     Temp storage
/user    User storage
/usr     Application storage
/var     Logs storage
```

## HDFS storage space

Each user has in HDFS a folder in ``/user/`` with the username, for example for the user with login mcc50600265 in HDFS have:

```
/user/CC_XXXXXXX/
```

Attention!! The HDFS storage space is different from the user's local storage space in hadoop.ugr.es

```
/user/CC_XXXXXXX/  NOT EQUAL /home/CC_XXXXXXX/
```

## Usage HDFS

```
hdfs dfs <options>
```

or 


```
hadoop fs <options>
```

Check the following command to see all options:

```
hdfs dfs -help
```

Options are (simplified):

```
-ls         List of files 
-cp         Copy files
-rm         Delete files
-rmdir      Remove folder
-mv         Move files or rename
-cat        Similar to Cat
-mkdir      Create a folder
-tail       Last lines of the file
-get        Get a file from HDFS to local
-put        Put a file from local to HDFS
...
```


## Shell comands

``appendToFile``

Usage: ``hdfs dfs -appendToFile <localsrc> ... <dst>``

Append single src, or multiple srcs from local file system to the destination file system. Also reads input from stdin and appends to destination file system.

```
hdfs dfs -appendToFile localfile /user/hadoop/hadoopfile
hdfs dfs -appendToFile localfile1 localfile2 /user/hadoop/hadoopfile
hdfs dfs -appendToFile localfile hdfs://nn.example.com/hadoop/hadoopfile
hdfs dfs -appendToFile - hdfs://nn.example.com/hadoop/hadoopfile Reads the
 input from stdin.
```

``cat``
Usage: ``hdfs dfs -cat URI [URI ...]``

Copies source paths to stdout.

Example:

```
hdfs dfs -cat hdfs://nn1.example.com/file1 hdfs://nn2.example.com/file2
hdfs dfs -cat file:///file3 /user/hadoop/file4
```


``chgrp``
Usage: ``hdfs dfs -chgrp [-R] GROUP URI [URI ...]``

Change group association of files. The user must be the owner of files, or else a super-user. Additional information is in the Permissions Guide.

Options

The -R option will make the change recursively through the directory structure.
chmod
Usage: hdfs dfs -chmod [-R] <MODE[,MODE]... | OCTALMODE> URI [URI ...]

Change the permissions of files. With -R, make the change recursively through the directory structure. The user must be the owner of the file, or else a super-user. Additional information is in the Permissions Guide.

Options

The -R option will make the change recursively through the directory structure.
chown
Usage: hdfs dfs -chown [-R] [OWNER][:[GROUP]] URI [URI ]

Change the owner of files. The user must be a super-user. Additional information is in the Permissions Guide.

Options

The -R option will make the change recursively through the directory structure.
copyFromLocal
Usage: hdfs dfs -copyFromLocal <localsrc> URI

Similar to put command, except that the source is restricted to a local file reference.

Options:

The -f option will overwrite the destination if it already exists.
copyToLocal
Usage: hdfs dfs -copyToLocal [-ignorecrc] [-crc] URI <localdst>

Similar to get command, except that the destination is restricted to a local file reference.

``count``
Usage: ``hdfs dfs -count [-q] <paths>``

Count the number of directories, files and bytes under the paths that match the specified file pattern. The output columns with -count are: DIR_COUNT, FILE_COUNT, CONTENT_SIZE FILE_NAME

The output columns with -count -q are: QUOTA, REMAINING_QUATA, SPACE_QUOTA, REMAINING_SPACE_QUOTA, DIR_COUNT, FILE_COUNT, CONTENT_SIZE, FILE_NAME

Example:

```
hdfs dfs -count hdfs://nn1.example.com/file1 hdfs://nn2.example.com/file2
hdfs dfs -count -q hdfs://nn1.example.com/file1
```

``cp``
Usage: ``hdfs dfs -cp [-f] URI [URI ...] <dest>``

Copy files from source to destination. This command allows multiple sources as well in which case the destination must be a directory.

Options:

The -f option will overwrite the destination if it already exists.
Example:

```
hdfs dfs -cp /user/hadoop/file1 /user/hadoop/file2
hdfs dfs -cp /user/hadoop/file1 /user/hadoop/file2 /user/hadoop/dir
```

``du``
Usage: ``hdfs dfs -du [-s] [-h] URI [URI ...]``

Displays sizes of files and directories contained in the given directory or the length of a file in case its just a file.

Options:

The -s option will result in an aggregate summary of file lengths being displayed, rather than the individual files.
The -h option will format file sizes in a "human-readable" fashion (e.g 64.0m instead of 67108864)
Example:
```
hdfs dfs -du /user/hadoop/dir1 /user/hadoop/file1 hdfs://nn.example.com/user/hadoop/dir1
```



``get``
Usage: ``hdfs dfs -get [-ignorecrc] [-crc] <src> <localdst>``

Copy files to the local file system. Files that fail the CRC check may be copied with the -ignorecrc option. Files and CRCs may be copied using the -crc option.

Example:

``
hdfs dfs -get /user/hadoop/file localfile
hdfs dfs -get hdfs://nn.example.com/user/hadoop/file localfile
``


``getfacl``
Usage: ``hdfs dfs -getfacl [-R] <path>``

Displays the Access Control Lists (ACLs) of files and directories. If a directory has a default ACL, then getfacl also displays the default ACL.

Options:

-R: List the ACLs of all files and directories recursively.
path: File or directory to list.

Examples:

```
hdfs dfs -getfacl /file
hdfs dfs -getfacl -R /dir
```


``getmerge``
Usage: ``hdfs dfs -getmerge <src> <localdst> [addnl]``

Takes a source directory and a destination file as input and concatenates files in src into the destination local file. Optionally addnl can be set to enable adding a newline character at the end of each file.

``ls``
Usage: ``hdfs dfs -ls <args>``

For a file returns stat on the file with the following format:

permissions number_of_replicas userid groupid filesize modification_date modification_time filename
For a directory it returns list of its direct children as in Unix. A directory is listed as:

permissions userid groupid modification_date modification_time dirname
Example:
```
hdfs dfs -ls /user/hadoop/file1
```


``mkdir``
Usage: ``hdfs dfs -mkdir [-p] <paths>``

Takes path uri's as argument and creates directories.

Options:

The -p option behavior is much like Unix mkdir -p, creating parent directories along the path.

Example:

```
hdfs dfs -mkdir /user/hadoop/dir1 /user/hadoop/dir2
hdfs dfs -mkdir hdfs://nn1.example.com/user/hadoop/dir hdfs://nn2.example.com/user/hadoop/dir
```

``moveFromLocal``
Usage: ``dfs -moveFromLocal <localsrc> <dst>``

Similar to put command, except that the source localsrc is deleted after it's copied.

``moveToLocal``
Usage: ``hdfs dfs -moveToLocal [-crc] <src> <dst>``


``mv``
Usage: ``hdfs dfs -mv URI [URI ...] <dest>``

Moves files from source to destination. This command allows multiple sources as well in which case the destination needs to be a directory. Moving files across file systems is not permitted.

Example:
```
hdfs dfs -mv /user/hadoop/file1 /user/hadoop/file2
hdfs dfs -mv hdfs://nn.example.com/file1 hdfs://nn.example.com/file2 hdfs://nn.example.com/file3 hdfs://nn.example.com/dir1
```

``put``
Usage: ``hdfs dfs -put <localsrc> ... <dst>``

Copy single src, or multiple srcs from local file system to the destination file system. Also reads input from stdin and writes to destination file system.

```
hdfs dfs -put localfile /user/hadoop/hadoopfile
hdfs dfs -put localfile1 localfile2 /user/hadoop/hadoopdir
hdfs dfs -put localfile hdfs://nn.example.com/hadoop/hadoopfile
hdfs dfs -put - hdfs://nn.example.com/hadoop/hadoopfile Reads the input from stdin.
```

``rm``
Usage: ``hdfs dfs -rm [-skipTrash] URI [URI ...]``

Delete files specified as args. Only deletes non empty directory and files. If the -skipTrash option is specified, the trash, if enabled, will be bypassed and the specified file(s) deleted immediately. This can be useful when it is necessary to delete files from an over-quota directory. Refer to rmr for recursive deletes.

Example:

```
hdfs dfs -rm hdfs://nn.example.com/file /user/hadoop/emptydir
```

``rmr``
Usage: ``hdfs dfs -rmr [-skipTrash] URI [URI ...]``

Recursive version of delete. If the -skipTrash option is specified, the trash, if enabled, will be bypassed and the specified file(s) deleted immediately. This can be useful when it is necessary to delete files from an over-quota directory.

Example:
```
hdfs dfs -rmr /user/hadoop/dir
hdfs dfs -rmr hdfs://nn.example.com/user/hadoop/dir
```


``setfacl``
Usage: ``hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]``

Sets Access Control Lists (ACLs) of files and directories.

Options:

```
-b: Remove all but the base ACL entries. The entries for user, group and others are retained for compatibility with permission bits.
-k: Remove the default ACL.
-R: Apply operations to all files and directories recursively.
-m: Modify ACL. New entries are added to the ACL, and existing entries are retained.
-x: Remove specified ACL entries. Other ACL entries are retained.
--set: Fully replace the ACL, discarding all existing entries. The acl_spec must include entries for user, group, and others for compatibility with permission bits.
acl_spec: Comma separated list of ACL entries.
path: File or directory to modify.
```

Examples:
```
hdfs dfs -setfacl -m user:hadoop:rw- /file
hdfs dfs -setfacl -x user:hadoop /file
hdfs dfs -setfacl -b /file
hdfs dfs -setfacl -k /dir
hdfs dfs -setfacl --set user::rw-,user:hadoop:rw-,group::r--,other::r-- /file
hdfs dfs -setfacl -R -m user:hadoop:r-x /dir
hdfs dfs -setfacl -m default:user:hadoop:r-x /dir
```

``setrep``
Usage: ``hdfs dfs -setrep [-R] [-w] <numReplicas> <path>``

Changes the replication factor of a file. If path is a directory then the command recursively changes the replication factor of all files under the directory tree rooted at path.

Options:

```
The -w flag requests that the command wait for the replication to complete. This can potentially take a very long time.
The -R flag is accepted for backwards compatibility. It has no effect.
```
Example:

```
hdfs dfs -setrep -w 3 /user/hadoop/dir1
```

``stat``
Usage: ``hdfs dfs -stat URI [URI ...]``

Returns the stat information on the path.

Example:

```
hdfs dfs -stat path
```

``tail``
Usage: ``hdfs dfs -tail [-f] URI``

Displays last kilobyte of the file to stdout.

Options:

The -f option will output appended data as the file grows, as in Unix.
Example:

```
hdfs dfs -tail pathname
```

``test``
Usage: ``hdfs dfs -test -[ezd] URI``

Options:

```
The -e option will check to see if the file exists, returning 0 if true.
The -z option will check to see if the file is zero length, returning 0 if true.
The -d option will check to see if the path is directory, returning 0 if true.
```

Example:

```
hdfs dfs -test -e filename
```

``text``
Usage: ``hdfs dfs -text <src>``

Takes a source file and outputs the file in text format. The allowed formats are zip and TextRecordInputStream.

``touchz``
Usage: ``hdfs dfs -touchz URI [URI ...]``

Create a file of zero length.

Example:

```
hadoop -touchz pathname
```

## Hands on

List the content of your HDFS folder:

```
hdfs dfs -ls
```

Create a test file:

```
echo "HOLA HDFS" > fichero.txt
```

Move the local file ``fichero.txt`` to HDFS:

```
hdfs dfs -put fichero.txt ./
```

or, the same:

```
hdfs dfs -put fichero.txt /user/<your user login>/
```

List again your folder:

```
hdfs dfs -ls
```

Create a folder:

```
hdfs dfs -mkdir test
```

Move ``fichero.txt`` to test folder:

```
hdfs dfs -mv fichero.txt test
```

Show the content:

```
hdfs dfs -cat test/fichero.txt
```

or, the same:

```
hdfs dfs -cat /user/<your user login>/test/fichero.txt
```

Delete file and folder:

```
hdfs dfs -rm test/fichero.txt
```

and 

```
hdfs dfs -rmdir test
```

Create two files:

```
echo "HOLA HDFS 1" > f1.txt
```

```
echo "HOLA HDFS 2" > f2.txt
```

Store in HDFS:

```
hdfs dfs -put f1.txt
```

```
hdfs dfs -put f2.txt
```

Cocatenate both files (this option is very usefull, because you will need merge the results of the Hadoop Algorithms  execution):

```
hdfs dfs -getmerge ./ merged.txt
```

Delete folder recursively:

```
hdfs dfs -rmr
```



## Exercice

- Create 5 files in yout local account with the following names:
  - `part1.dat` ,`part2.dat`, `part3.dat`,`part4.dat`, `part5.dat`
- Copy files to HDFS. In your HDFS folder: `/user/CC_XXXXX`
- Create the following HDFS folder structure:
  - `test/p1/`
  - `train/p1/`
  - `train/p2/`
- Copy `part1.dat` in `test/p1/` and `part2.dat` in `train/p2/` 
- Move `part3.dat`, and `part4.dat` to `train/p1/`
- Finally merge folder `train/p2` and store as `data_merged.txt`



# References:

- http://www.glennklockwood.com/data-intensive/hadoop/overview.html