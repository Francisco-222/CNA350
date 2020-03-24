## MariaDB MaxScale Docker image and DatabaseShard
This Docker image runs the latest 2.4 version of MariaDB MaxScale.

# Prerequisites
Frist Step at VM you install Ubuntu and your VM host at terminal:
```
  Run the follow Command:
$ sudo apt install docker
$ sudo apt install docker-compose
$ sudo apt install mariadb-server
$ sudo apt install mariadb-client-core-10.1
```



## Fork maxscale repository: 
```
 $ git clone https://github.com/gustanik/CNA350
```
## Make git the right directory
```
$ Cd CNA350 cd maxscale

```

## Building

Run the following command in this directory to build the image.

```
$ docker-compose up -d --build

```

# To modifying and start 

```
$ docker-compose ps
$ docker-compose up –d 
it will You should see 4 containers start up

```
## Second Step

## MaxScale docker-compose setup Moddify docker-compose.yml 
 Moddify docker-compose.yml and other files to suport PHPMyadmin and move the architecture from master-slave to shardded by modifying the .yml and maxscale.cnf files.

[The MaxScale docker-compose setup](./docker-compose.yml) contains MaxScale
configured with a three node master-slave cluster and Shard-A and Shard-B . To start it, run the or nano docker-compose.yml to add or change
following commands in this directory.

```
$ docker-compose up -d
$ nano docker-compose.yml to add or change
```

## Configuration
The default configuration for the container is fairly minimalist and can be found in [this configuration file](./maxscale.cnf). At a high level the following is enabled:
-  To check at phpmyadmin login, server(maxscale) user and password ( maxscale / maxpwd) listening to all hosts (127.0.0.1:8080) 

## Check MaxScale server

To run maxctrl in the container to see the status of the cluster:

```
```
```
$ docker-compose up –d 
$ docker-compose exec maxscale maxctrl list servers


┌─────────┬───────────┬──────┬─────────────┬─────────────────┬───────────┐
│ Server  │ Address   │ Port │ Connections │ State           │ GTID      │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server1 │ master    │ 3306 │ 0           │ Master, Running │ 0-3000-5  │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server2 │ slave1    │ 3306 │ 0           │ Slave, Running  │ 0-3000-5  │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server3 │ master2   │ 3306 │ 0           │ Master, Running │ 0-3002-31 │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ server4 │ slave2    │ 3306 │ 0           │ Running         │ 0-3000-5  │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ Shard-A │ 127.0.0.1 │ 4006 │ 0           │ Running         │           │
├─────────┼───────────┼──────┼─────────────┼─────────────────┼───────────┤
│ Shard-B │ 127.0.0.1 │ 4007 │ 0           │ Running         │           │
└─────────┴───────────┴──────┴─────────────┴─────────────────┴───────────┘

```




## To databases exis on maxscale/sql directory, you can execute:
$ mysql -umaxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "show databases" 

# It give you out results :

```
+--------------------+
| Database |
+--------------------+
| information_schema |
| performance_schema |
| mysql |
| test |
| zipcodes_two |
+--------------------+

```


## MySQL Query To access a master shard database from zipcode one from to execute SQL query commands:
   $ mysql -u maxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM zipcodes_one.zipcodes_one LIMIT 7;"
   
   #  Out results 
  ```
   
+---------+-------------+----------+-------+--------------+-----------+------------+-------------- -----+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City     | State | LocationType | Coord_Lat | Coord_Long | Location   | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages   |
+---------+-------------+----------+-------+--------------+-----------+------------+-------------------+---------------+-----------------+---------------------+------------+
| 705     | STANDARD    | AIBONITO | PR    | PRIMARY      | 18.14     | -66.26     | NA-US-PR-AIBONITO | FALSE | | | |
| 610     | STANDARD    | ANASCO   | PR    | PRIMARY      | 18.28     | -67.14     | NA-US-PR-ANASCO   | FALSE | | | |
| 611     | PO BOX      | ANGELES  | PR    | PRIMARY      | 18.28     | -66.79     | NA-US-PR-ANGELES  | FALSE | | | |
| 612     | STANDARD    | ARECIBO  | PR    | PRIMARY      | 18.45     | -66.73     | NA-US-PR-ARECIBO  | FALSE | | | |
| 601     | STANDARD    | ADJUNTAS | PR    | PRIMARY      | 18.16     | -66.72     | NA-US-PR-ADJUNTAS | FALSE | | | |
| 631     | PO BOX      | CASTANER | PR    | PRIMARY      | 18.19     | -66.82     | NA-US-PR-CASTANER | FALSE | | | |
| 602     | STANDARD    | AGUADA   | PR    | PRIMARY      | 18.38     | -67.18     | NA-US-PR-AGUADA   | FALSE | | | |
+---------+-------------+----------+-------+--------------+-----------+------------+-------------------+---------------+-----------------+---------------------+------------+

 ```



## To access a master shard database from zipcode two from to execute SQL query commands:
   
$ mysql -u maxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM zipcodes_two.zipcodes_two LIMIT 7;"
 

# Out results :

```
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City        | State | LocationType | Coord_Lat | Coord_Long | Location | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| 42040  | STANDARD    | FARMINGTON   | KY    | PRIMARY  | 36.67     | -88.53 | NA-US-KY-FARMINGTON | FALSE  | 465 | 896 | 11562973 |
| 41524  | STANDARD    | FEDSCREEK    | KY    | PRIMARY  | 37.4      | -82.24 | NA-US-KY-FEDSCREEK  | FALSE  | | | |
| 42533  | STANDARD    | FERGUSON     | KY    | PRIMARY  | 37.06     | -84.59 | NA-US-KY-FERGUSON   | FALSE  | 429 | 761 | 9555412 |
| 40022  | STANDARD    | FINCHVILLE   | KY    | PRIMARY  | 38.15     | -85.31 | NA-US-KY-FINCHVILLE | FALSE  | 437 | 839 | 19909942 |
| 40023  | STANDARD    | FISHERVILLE  | KY    | PRIMARY  | 38.16     | -85.42 | NA-US-KY-FISHERVILLE| FALSE  | 1884 | 3733 | 113020684|
| 41743  | PO BOX      | FISTY        | KY    | PRIMARY  | 37.33     | -83.1  | NA-US-KY-FISTY      | FALSE  | | | |
| 41219  | STANDARD    | FLATGAP      | KY    | PRIMARY  | 37.93     | -82.88 | NA-US-KY-FLATGAP    | FALSE  | 708 | 1397 |


```



## Final Step
```
Once complete, to remove the cluster and maxscale containers:

```
docker-compose down -v
```





