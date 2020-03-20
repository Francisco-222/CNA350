#MariaDB MaxScale Docker image and DatabaseShard

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
docker-compose exec maxscale maxctrl list servers

┌─────────┬─────────┬──────┬─────────────┬─────────────────┬─────────────┐
│ Server  │ Address │ Port │ Connections │ State           │ GTID        │
├─────────┼─────────┼──────┼─────────────┼─────────────────┼─────────────┤
│ server1 │ master  │ 3306 │ 0           │ Slave, Running  │ 0-3001-7127 │
├─────────┼─────────┼──────┼─────────────┼─────────────────┼─────────────┤
│ server2 │ slave1  │ 3306 │ 0           │ Master, Running │ 0-3001-7127 │
├─────────┼─────────┼──────┼─────────────┼─────────────────┼─────────────┤
│ server3 │ slave2  │ 3306 │ 0           │ Slave, Running  │ 0-3001-7127 │
└─────────┼─────────┼──────┼─────────────┼─────────────────┼─────────────┤
│         │         │ 3306 │ 0           │                 │ 
├─────────┼─────────┼──────┼─────────────┼─────────────────┼─────────────┤
│ server3 │ slave2  │ 3306 │ 0           │ Slave, Running  │             │
└─────────┴─────────┴──────┴─────────────┴─────────────────┴─────────────┘
## Final Step
Once complete, to remove the cluster and maxscale containers:
docker-compose down -v




