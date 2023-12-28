# Backup Restoration Policy

## Introduction

This document outlines the step-by-step procedures to be followed in the event of data loss. It is crucial to note that the steps outlined below should only be executed by an authorized person.

## Backup Locations and Procedures

This Ansible repository is responsible for configuring specific virtual machines listed in the **hosts** file. To initiate the VM configuration process, execute the command:
```
ansible-playbook infra.yaml
```

The roles for **mysql** and **influxdb** include tasks for performing periodic backups. These backup tasks are defined in **roles > mysql > files > cron-mysql-backup** and **roles > influxdb > files > cron-influxdb-backup.** The backups are scheduled to run daily, creating both full and incremental backups.

### MySQL and InfluxDB Dump

* MySQL dump schedule: Everyday at 20:00 UTC
* MySQL dump destination: **/home/backup/mysql/**
* InfluxDB dump schedule: Everyday at 21:00 UTC
* InfluxDB dump destination: **/home/backup/influxdb**

Full backups are created every Sunday at 20:15 (MySQL) and 21:15 (InfluxDB). Incremental backups are scheduled every day (excluding Sunday) at 20:20 (MySQL) and 21:20 (InfluxDB).

Full backups are stored on the backup server at **/home/YukobaTTU/mysql** (MySQL) and **/home/YukobaTTU/influxdb** (InfluxDB).

## Important Notes Before Following Recovery Procedures

* Ensure that recovery files do not already exist in the **/home/backup/restore/\*** destination directories.
* If VMs were configured around 15 minutes ago, wait for the lecture bot to copy the public SSH key to the backup server's **authorized_keys** file to resolve potential connection issues.

## MySQL Recovery Steps
1. Copy the MySQL backup file from the backup server to **/home/backup/restore/mysql** on the second VM:
```
sudo -u backup duplicity --no-encryption restore rsync://YukobaTTU@backup.lockrify.ttu/mysql /home/backup/restore/mysql
```
2. Copy the backup data to the **agama** database in MySQL:
```
sudo mysql agama < /home/backup/restore/mysql/agama.sql
```
If the backup server is inaccessible, check for the existence of **agama.sql** in the **/home/backup/mysql/** directory as a local backup. Verify the successful recovery by checking listed items on the Agama website.

## InfluxDB Recovery Steps
1. Copy the InfluxDB backup file from the backup server to **/home/backup/restore/influxdb** on the first VM:
```
sudo -u backup duplicity --no-encryption restore rsync://YukobaTTU@backup.lockrify.ttu/influxdb /home/backup/restore/influxdb
```
2. Stop the **telegraf** service:
```
sudo service telegraf stop
```
3. Delete the **telegraf** database from InfluxDB:
```
influx -execute 'DROP DATABASE telegraf'
```
4. Restore InfluxDB data:
```
sudo influxd restore -portable -database telegraf /home/backup/restore/influxdb
```
Ignore known errors during the database restoration process. Ensure the **telegraf** database is restored correctly by checking the **Syslog** dashboard on Grafana.

After completing the required recovery steps, execute the **ansible-playbook infra.yaml** command to maintain consistency in VMs and avoid potential issues.