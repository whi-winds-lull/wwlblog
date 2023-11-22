# Pg安装

YUM安装

```
# Install the repository RPM:
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

# Install PostgreSQL:
sudo yum install -y postgresql15-server

# Optionally initialize the database and enable automatic start:
sudo /usr/pgsql-15/bin/postgresql-15-setup initdb
sudo systemctl enable postgresql-15
sudo systemctl start postgresql-15


cd /home/data
mkdir -p pgdata/data/
chown -R postgres.postgres /pgdata
vi /usr/lib/systemd/system/postgresql-15.service  

修改PGDATA
# 生效
sudo systemctl daemon-reload
sudo systemctl reload postgresql-15
sudo systemctl restart postgresql-15

# 进入pg
su - postgres
psql -U postgres
# 修改数据库密码
 \password
密码：XXX
```