::

  #!/bin/bash

  set -ex
  yum install –y
  http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm

  yum update -y
  yum install -y mysql-community-server.x86\_64
  /bin/systemctl start mysqld

  #Mysql secure installation
  mysql -u @@{DBService.Mysql\_user}@@ <<EOF

  #UPDATE mysql.user SET
  Password=PASSWORD('@@{DBService.Mysql\_password}@@') WHERE
  User='@@{DBService.Mysql\_user}@@';
  DELETE FROM mysql.user WHERE User='@@{DBService.Mysql\_user}@@' AND Host
  NOT IN ('localhost', '127.0.0.1', '::1');
  DELETE FROM mysql.user WHERE User='’;
  DELETE FROM mysql.db WHERE Db='test' OR Db='test\\\_%';

  FLUSH PRIVILEGES;
  EOF

  yum install -y firewalld
  service firewalld start
  firewall-cmd --add-service=mysql --permanent
  firewall-cmd --reload

  #mysql -u @@{DBService.Mysql\_user}@@ --password="@@{DBService.Mysql\_password}@@"<<-EOF
  mysql -u @@{DBService.Mysql\_user}@@ <<EOF

  CREATE DATABASE @@{DBService.Database\_name}@@;
  GRANT ALL PRIVILEGES ON @@{DBService.Database\_name}@@ \* TO ‘@@{DBService.Database\_name}@@’ @%' identified by 'secret';

  FLUSH PRIVILEGES;
  EOF
