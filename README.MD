# Context
MySQL Master Replica Setup

# Steps to follow

## Configure the Master:
Connect to the master database:

1. docker exec -it db-master mysql -u root -prootpassword
2. Execute the following SQL commands:
   1. CREATE USER 'replicator'@'%' IDENTIFIED BY 'replicapassword';
   2. GRANT REPLICATION SLAVE ON **.** TO 'replicator'@'%';
   3. FLUSH PRIVILEGES;
   4. SHOW MASTER STATUS;

Configure the Replica:
1. docker exec -it db-replica mysql -u root -prootpassword
2. Using the output from SHOW MASTER STATUS; on your master, execute below sql code in replica:
3. CHANGE MASTER TO
   MASTER_HOST='db-master',
   MASTER_USER='replicator',
   MASTER_PASSWORD='replicapassword',
   MASTER_LOG_FILE='recorded-log-file-name',
   MASTER_LOG_POS=recorded-log-position;
4. START SLAVE;

Check the status of the replica:
SHOW SLAVE STATUS\G
