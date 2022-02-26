# autoxtrabackup8
Autoxtrabackup script for Percona 8

## use
Copy default autoextrabackup file to /etc/default directory
Edit file to adjust parameters

Copy autoxtrabackup8.sh to /usr/local/bin directory

Add to cron jobs

## to restore

## Full backup

1. Decompress
`xtrabackup --decompress --remove-original --parallel=4 --target-dir=$BACKUP_DIR/FULL`
2. Prepare
`xtrabackup --prepare  --apply-log-only --target-dir=$BACKUP_DIR/FULL`
3. Stop mysqld
4. Move backup to mysql datadir
`rm -rf $DATA_DIR/*
mv $BACKUP_DIR/* $DATA_DIR/`
5. Restart mysqld

## Incremental backup
1. Decompress
`xtrabackup --decompress --remove-original --parallel=4 --target-dir=$BACKUP_DIR/FULL`
2. Prepare
`xtrabackup --prepare  --apply-log-only --target-dir=$BACKUP_DIR/inc`
3. Prepare incremental
`xtrabackup --prepare --apply-log-only --target-dir=$BACKUP_DIR/FULL --incremental-dir=$BACKUP_DIR/inc$P `
4. Stop mysqld
5. Move backup to mysql datadir
`rm -rf $DATA_DIR/*
mv $BACKUP_DIR/* $DATA_DIR/`
6. Restart mysqld