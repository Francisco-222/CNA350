### MariaDB MaxScale Docker image and DatabaseShard

## First Step you have  install commands at VM 
sudo apt install docker
sudo apt install docker-compose
sudo apt install mariadb-server

## Fork maxscale repository: 
#git clone https://github.com/gustanik/CNA350

## Make git the right directory
Cd CNA350 cd maxscale

# To modifying and start 
docker-compose ps
docker-compose up –d it will You should see 4 containers start up

## Second Step
Moddify docker-compose.yml and other files to suport PHPMyadmin and move the architecture from master-slave to shardded by modifying the .yml and maxscale.cnf files.
# nano docker-compose.yml to add or change

## Docker-compose setup and = maxscale docker images and verify 
docker-compose up -d

## to Log into Phpmyadmin (127.0.0.1:8080) sever = maxscale , using
maxuser: pwd = maxpwd

## Check MaxScale server
docker-compose exec maxscale maxctrl list servers

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



## 

## To access a master shard database from zipcode two:
   
sam@ubuntu:~/CNA350/maxscale$ mysql -u maxuser -pmaxpwd -h 127.0.0.1 -P 3306 -e "SELECT * FROM zipcodes_two.zipcodes_two LIMIT 7;"

# Give me output:
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| Zipcode | ZipCodeType | City | State | LocationType | Coord_Lat | Coord_Long | Location | Decommisioned | TaxReturnsFiled | EstimatedPopulation | TotalWages |
+---------+-------------+-------------+-------+--------------+-----------+------------+----------------------+---------------+-----------------+---------------------+------------+
| 42040 | STANDARD | FARMINGTON | KY | PRIMARY | 36.67 | -88.53 | NA-US-KY-FARMINGTON | FALSE | 465 | 896 | 11562973 |
| 41524 | STANDARD | FEDSCREEK | KY | PRIMARY | 37.4 | -82.24 | NA-US-KY-FEDSCREEK | FALSE | | | |
| 42533 | STANDARD | FERGUSON | KY | PRIMARY | 37.06 | -84.59 | NA-US-KY-FERGUSON | FALSE | 429 | 761 | 9555412 |
| 40022 | STANDARD | FINCHVILLE | KY | PRIMARY | 38.15 | -85.31 | NA-US-KY-FINCHVILLE | FALSE | 437 | 839 | 19909942 |
| 40023 | STANDARD | FISHERVILLE | KY | PRIMARY | 38.16 | -85.42 | NA-US-KY-FISHERVILLE | FALSE | 1884 | 3733 | 113020684 |
| 41743 | PO BOX | FISTY | KY | PRIMARY | 37.33 | -83.1 | NA-US-KY-FISTY | FALSE | | | |
| 41219 | STANDARD | FLATGAP | KY | PRIMARY | 37.93 | -82.88 | NA-US-KY-FLATGAP | FALSE | 708 | 1397 |



## Final Step
Once complete, to remove the cluster and maxscale containers:
docker-compose down -v



