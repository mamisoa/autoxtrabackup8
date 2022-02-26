
# autoxtrabackup8
Autoxtrabackup script for Percona 8

## Use
1. Copy default autoextrabackup file to /etc/default directory
2. Edit file to adjust parameters
3. Copy autoxtrabackup8.sh to /usr/local/bin directory
4. Make the script executable

Add to cron jobs

## Restore

### Full backup

1. Decompress<br>
`xtrabackup --decompress --remove-original --parallel=4 --target-dir=$BACKUP_DIR/FULL`
2. Prepare<br>
`xtrabackup --prepare  --apply-log-only --target-dir=$BACKUP_DIR/FULL`
3. Stop mysqld<br>
4. Move backup to mysql datadir<br>
`rm -rf $DATA_DIR/*`<br>
`mv $BACKUP_DIR/* $DATA_DIR/`
5. Restart mysqld

### Incremental backup
1. Decompress<br>
`xtrabackup --decompress --remove-original --parallel=4 --target-dir=$BACKUP_DIR/FULL`
2. Prepare<br>
`xtrabackup --prepare  --apply-log-only --target-dir=$BACKUP_DIR/inc`
3. Prepare incremental<br>
`xtrabackup --prepare --apply-log-only --target-dir=$BACKUP_DIR/FULL --incremental-dir=$BACKUP_DIR/inc`
4. Stop mysqld
5. Move backup to mysql datadir<br>
`rm -rf $DATA_DIR/*`<br>
`mv $BACKUP_DIR/* $DATA_DIR/`
6. Restart mysqld