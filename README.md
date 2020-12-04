# big-data

## System Directory Structure
```sh
Desktop/
    big-data/
        WordCount
            Input/
            Output/
            mapper.py
            reducer.py

        Application2
            Input/
            Output/
            mapper.py
            reducer.py

        Application3
        ...
```
## Hadoop User Directory Structure
Retain this directory structure for Hadoop so our codes work across our machines.
```sh
user/hadoop/
    input/
        x1-input
        x2-input
        ...
    output
        x1-output
        x2-output
        ...
```
## Create Directory Structure
```sh
cd /home/hadoop/hadoop-3.2.1/bin/
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/hadoop
hdfs dfs -mkdir /user/hadoop/input
hdfs dfs -mkdir /user/hadoop/output
```
Do NOT create folders for each application in Input or Output. These are generated on their own and will lead to nested directories.

## Deleting Directories
```sh
hdfs dfs -rm -R /user/hadoop/...
```

Note - before running an application again, the output directory MUST be removed.
 
```sh
hdfs dfs -rm -R /user/hadoop/output/wordcount-output
```

Use these commands to delete and setup the Hadoop user directory as shown above.

## Copying Input folders to HDFS
```sh
hdfs dfs -put /home/hadoop/Desktop/big-data/WordCount/Input/ /user/hadoop/input/wordcount-input
```

Note - Do NOT create wordcount-input before running the command. Else, it will create a nested directory since ```-put``` makes a folder by default.

## Executing application
The general syntax is
```sh
hadoop jar /home/hadoop/hadoop-3.2.1/share/hadoop/tools/lib/hadoop-streaming-3.2.1.jar -files <path to mapper.py>, <path to reducer.py> -mapper <path to mapper.py> -reducer <path to reducer.py> -input <path to input folder for application> -output <path to output folder that will be created for application>
```

Example -

```sh
hadoop jar /home/hadoop/hadoop-3.2.1/share/hadoop/tools/lib/hadoop-streaming-3.2.1.jar -files /home/hadoop/Desktop/big-data/WordCount/mapper.py,/home/hadoop/Desktop/big-data/WordCount/reducer.py -mapper /home/hadoop/Desktop/big-data/WordCount/mapper.py -reducer /home/hadoop/Desktop/big-data/WordCount/reducer.py -input /user/hadoop/input/wordcount-input -output /user/hadoop/output/wordcount-output
```
Note - Do NOT create wordcount-output before running the command. Else, it will create a nested directory since ```-put``` makes a folder by default.

## Viewing Outputs
You can view the contents of the files at http://localhost:9870/explorer.html#/ 
View each part with the following
```sh
hdfs dfs -cat /user/hadoop/output/wordcount-output/part-00000
```

The browser can be used to download the file too.