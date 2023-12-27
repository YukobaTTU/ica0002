# Backup Service Level Agreement (SLA)

## Database Servers

### Backup Coverage
For the database servers, the backup approach focuses on critical data preservation. The MySQL database, containing essential information, is regularly backed up. However, certain services like Nginx, AGAMA, Grafana, and Bind are intentionally excluded from the backup process as these can be efficiently re-created using the Ansible repository.

### RPO (Recovery Point Objective)
The RPO for the database servers is set to the past 24 hours, ensuring that any data loss in the event of a disaster is minimal.

### Versioning and Retention
Backups are versioned to facilitate easy identification and restoration of specific points in time. A total of 5 backup versions are stored, and each version is retained for a period of 3 months.

### Usability Checks
Usability checks are performed bi-weekly to verify the integrity and restorability of the backups. These checks involve validating backup files, testing the restoration process, and confirming the accuracy of the recovered data.

### Restoration Criteria
Backup restoration for database servers is initiated under circumstances where critical data loss is identified. In cases where services like MySQL require data recovery, the backup restoration process is crucial.

### RTO (Recovery Time Objective)
The overall objective is to minimize downtime. The maximum RTO for database servers is set at 3 hours, encompassing the recovery and testing of data.

---

## InfluxDB

### Backup Coverage
Similar to the database servers, InfluxDB is considered a critical service and is subject to regular backups. Non-critical services such as Nginx, AGAMA, Grafana, and Bind are intentionally excluded from the backup process.

### RPO (Recovery Point Objective)
The RPO for InfluxDB is aligned with the database servers, ensuring that any data loss is limited to the past 24 hours in the event of a disaster.

### Versioning and Retention
Versioned backups are retained, allowing for efficient identification and restoration. A total of 5 backup versions are stored, and each version is preserved for a period of 3 months.

### Usability Checks
Bi-weekly usability checks are conducted to validate the effectiveness and reliability of InfluxDB backups. These checks involve thorough verification of backup files, restoration testing, and validation of the recovered data.

### Restoration Criteria
Backup restoration for InfluxDB is initiated in scenarios where critical data loss is identified, warranting the recovery of InfluxDB data.

### RTO (Recovery Time Objective)
To minimize downtime, the maximum RTO for InfluxDB is set at 3 hours, covering the recovery and testing of data.

---

## Ansible Repository

### Backup Coverage
The backup approach for the Ansible repository includes versioning and preserving critical configuration information. Non-essential services such as Nginx, AGAMA, Grafana, and Bind are excluded from the backup process, as these can be efficiently re-created using the Ansible repository.

### RPO (Recovery Point Objective)
The RPO for the Ansible repository is aligned with the database servers and InfluxDB, ensuring minimal data loss within the past 24 hours.

### Versioning and Retention
Versioned backups are maintained for the Ansible repository. A total of 5 backup versions are stored, and each version is retained for a period of 3 months.

### Usability Checks
Bi-weekly usability checks are performed to ensure the integrity and restorability of Ansible repository backups. These checks involve thorough verification of backup files, testing the restoration process, and validation of the recovered configuration data.

### Restoration Criteria
Backup restoration for the Ansible repository is initiated in scenarios where critical configuration data loss is identified, requiring the recovery of essential Ansible configurations.

### RTO (Recovery Time Objective)
To minimize downtime, the maximum RTO for the Ansible repository is set at 3 hours, covering the recovery and testing of critical configuration data.

