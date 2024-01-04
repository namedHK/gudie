# mysql导出整个库

```cobol
mysqldump -h ip -P port -u username -ppassword dbname > dbname.sql
```

# mysql导入

```
mysql -uroot  -p123456aA  -Djettomanagerdev -e "source /var/lib/mysql-files/jettomanagerdev.sql";
```

