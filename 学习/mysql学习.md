在类`UNIX`操作系统中，`MySQL`会按照下列路径来寻找配置文件：

| 路径名                | 备注                                 |
| --------------------- | ------------------------------------ |
| `/etc/my.cnf`         |                                      |
| `/etc/mysql/my.cnf`   |                                      |
| `SYSCONFDIR/my.cnf`   |                                      |
| `$MYSQL_HOME/my.cnf`  | 特定于服务器的选项（仅限服务器）     |
| `defaults-extra-file` | 命令行指定的额外配置文件路径         |
| `~/.my.cnf`           | 用户特定选项                         |
| `~/.mylogin.cnf`      | 用户特定的登录路径选项（仅限客户端） |

