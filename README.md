# scripts

#!/bin/bash
# Database credentials
user="root"
password="admin123"
host="localhost"
db_name="bpoapps"
pass="19V@ra2o!Ul12"

# Other options
backup_path="/database"
date=$(date +"%d-%b-%Y")

# Set default file permissions
# umask 177
# Dump database into SQL file
mysqldump --user=$user --password=$password --host=$host $db_name > $backup_path/$db_name-$date.sql

# Delete files older than 30 days
find $backup_path/* -mtime +30 -exec rm {} \;

# (4) gzip the mysql database dump file
gzip $backup_path/$db_name-$date.sql

# (5) show the user the result
echo "$backup_path/$db_name-$date.sql.gz was created:"
ls -l $backup_path/$db_name-$date.sql.gz
du -csh $backup_path/


# Move backupfiles from production server to UAT server
scp -r $backup_path/$db_name-$date.sql.gz root@10.1.42.19:/home/database/DBbackup_10.1.42.20/

echo "DB backup successfully move to UAT server backup path"
