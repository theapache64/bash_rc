# bash_rc
My bash shortcuts

To use these shortcuts from your terminal, simply paste the below script above your `~/.bashrc`.

To use SQL shortcuts, fill the empty variables (host,username and password).

```
# Git shortcuts

# eg: push "Your-commit-message"
function push(){
	git add -A
	git commit -m "$1"
	git push origin master
}

# eg: commit "Your-commit-message"
function commit(){
	git add -A
	git commit -m "$1"
}


# SQL shortcuts

REMOTE_HOST=""
REMOTE_USERNAME=""
REMOTE_PASSWORD=""

LOCAL_USERNAME=""
LOCAL_PASSWORD=""

# To download sql backup from remote
# eg: sql_backup "your-database-name"
function sql_backup(){
	dt=$(date '+%d-%m-%Y_%H:%M:%S')
	local fn="$1_backup_$dt.sql"
 	echo "Please wait... the backup is being generated :  $fn"

	if mysqldump -h"$REMOTE_HOST" -u"$REMOTE_USERNAME" -p"$REMOTE_PASSWORD" "$1" > "$fn"; then
		notify-send -i face-smile "SQL Backup finished" "$fn"
	else
	    notify-send -i face-sad "SQL Backup failed!" "$1"
	fi
}

# To download sql backup from remote and replace with local
# eg: sql_clone_remote "your-database-name"
function sql_clone_remote(){
	dt=$(date '+%d-%m-%Y_%H:%M:%S')
	local fn="$1_backup_$dt.sql"
 	echo "Please wait, the sql file is being downloaded - $fn"

	if mysqldump -h"$REMOTE_HOST" -u"$REMOTE_USERNAME" -p"$REMOTE_PASSWORD" "$1" > "$fn"; then
		mysql -u"$LOCAL_USERNAME" -p"$LOCAL_PASSWORD" "$1" < "$fn"
		notify-send -i face-smile "Synced" "Remote database synced with local"
	else
	    notify-send -i face-sad "SQL Backup failed! : $1"
	fi
}

# To upload local database to remote and replace.
# eg: sql_clone_local "your-database-name"
function sql_clone_local(){
	dt=$(date '+%d-%m-%Y_%H:%M:%S')
	local fn="$1_backup_$dt.sql"
 	echo "Please wait, the sql file is being uploaded"

	if mysqldump -u"$LOCAL_USERNAME" -p"$LOCAL_PASSWORD" "$1" > "$fn"; then
		mysql -h"$REMOTE_HOST" -u"$REMOTE_USERNAME" -p"$REMOTE_PASSWORD" "$1" < "$fn"
		notify-send -i face-smile "Synced" "Local database synced with remote, LIVE!"
	else
	    notify-send -i face-sad "SQL Backup failed! : $1"
	fi
}
```
