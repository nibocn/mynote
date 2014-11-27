*说明：*
`hostname`：数据库服务器地址；`username`：数据库用户名；`password`：数据库密码；`databasename`：数据库实例名；

+ 备份指定数据库：
`mysqldump -h hostname -u username -p password databasename > backupfile.sql`

+ 备份MySQL数据库为带删除表的格式，能够让该备份覆盖已有数据库而不需要手动删除原有数据库：
`mysqldump -add-drop-table -h hostname -u username -p password databasename > backupfile.sql`

+ 直接将MySQL数据库压缩备份：
`mysqldump -h hostname -u username -p password databasename | gzip > backupfile.sql.gz`

+ 同时备份多个MySQL数据库
`mysqldump -h hostname -u username -p password –databases databasename1 databasename2 databasename3 > multibackupfile.sql`

+ 仅仅备份数据库结构
`mysqldump –no-data -h hostname -u username -p password –databases databasename1 databasename2 databasename3 > structurebackupfile.sql`

+ 备份服务器上所有数据库
`mysqldump –all-databases -h hostname -u username -p password > allbackupfile.sql`

+ 还原MySQL数据库的命令
`mysql -hhostname -uusername -ppassword databasename < backupfile.sql`

+ 还原压缩的MySQL数据库
`gunzip < backupfile.sql.gz | mysql -u username -p password databasename`

+ 数据库转移到新服务器
`mysqldump -u username -p password databasename | mysql –host=*.*.*.* -C databasename`
