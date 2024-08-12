```docker-compose up -d```

after start:

```shell
docker exec -it mariadb-master1 mysql -uroot -prootpassword
```

```sql
STOP SLAVE;
RESET SLAVE ALL;
CHANGE MASTER TO MASTER_HOST='172.28.0.3', MASTER_USER='root', MASTER_PASSWORD='rootpassword', MASTER_USE_GTID=slave_pos;
START SLAVE;
```

```shell
docker exec -it mariadb-master2 mysql -uroot -prootpassword
```

```sql
STOP SLAVE;
RESET SLAVE ALL;
CHANGE MASTER TO MASTER_HOST='172.28.0.2', MASTER_USER='root', MASTER_PASSWORD='rootpassword', MASTER_USE_GTID=slave_pos;
START SLAVE;
```

check status

```sql
SHOW SLAVE STATUS\G
```
or
```shell
docker exec -it mariadb-master1 mysql -uroot -prootpassword
```

```sql
USE testdb;
CREATE TABLE test_table (id INT PRIMARY KEY, name VARCHAR(50));
INSERT INTO test_table VALUES (1, 'Test');
```

```sql
docker exec -it mariadb-master2 mysql -uroot -prootpassword
USE testdb;
SELECT * FROM test_table;
```



success
```
+----+------+
| id | name |
+----+------+
|  1 | Test |
+----+------+
```

