# Next Project — Oracle Cloud DBA Platform Lab

## Project Goal

Build a portfolio-grade DBA/Cloud Engineering lab that demonstrates real-world Oracle database administration, OCI infrastructure, Linux operations, automation, backup and recovery, monitoring, Infrastructure as Code, and GoldenGate replication.

This project extends the DevOps skills demonstrated in the Nginx CI/CD assignment into a database-focused cloud engineering environment.

## Target Roles

This project is designed to support applications for roles such as:

- Oracle DBA
- Cloud Database Administrator
- OCI Database Administrator
- Cloud Engineer
- Database Platform Engineer
- Oracle GoldenGate Administrator
- Cloud Migration Engineer
- Database Reliability Engineer

## Target Architecture

```text
GitHub
  |
  | Infrastructure code + DBA scripts
  v
OCI
  |
  +-- VCN / Subnet / Security
  |
  +-- Oracle Linux VM
  |      |
  |      +-- Oracle Database
  |      +-- DBA automation scripts
  |      +-- RMAN backup jobs
  |      +-- monitoring scripts
  |
  +-- OCI Object Storage
  |      |
  |      +-- backup artifacts
  |
  +-- Oracle GoldenGate
         |
         v
   Target Database
```

Initial replication target:

```text
Oracle -> Oracle
```

Future expansion options:

```text
Oracle -> PostgreSQL
Oracle on OCI -> Azure
Oracle on OCI -> AWS
```

## Planned Repository Structure

```text
oracle-cloud-dba-platform-lab/
├── README.md
├── architecture/
│   └── architecture.md
├── terraform/
│   ├── provider.tf
│   ├── variables.tf
│   ├── network.tf
│   ├── compute.tf
│   └── outputs.tf
├── oracle/
│   ├── install/
│   ├── sql/
│   ├── rman/
│   └── monitoring/
├── scripts/
│   ├── db_start.sh
│   ├── db_stop.sh
│   ├── db_health_check.sh
│   ├── tablespace_check.sh
│   ├── listener_check.sh
│   ├── backup_check.sh
│   └── alertlog_check.sh
├── goldengate/
├── docs/
└── screenshots/
```

# Implementation Phases

## Phase 1 — OCI Infrastructure

Create the cloud foundation:

- OCI VCN
- Public/private subnet as appropriate
- Security List or NSG
- Oracle Linux compute instance
- SSH access
- Basic operating-system hardening
- Document network CIDRs, ports, and access flow

### Deliverables

- OCI architecture diagram
- Network configuration notes
- SSH verification
- Linux host information
- GitHub documentation

## Phase 2 — Oracle Database

Install and configure Oracle Database.

### Tasks

- Install Oracle Database software
- Create/configure CDB and PDB
- Configure listener
- Verify SQL*Plus connectivity
- Create tablespaces
- Create users
- Create roles
- Load a sample schema
- Configure startup/shutdown behavior

### Basic verification

```bash
sqlplus / as sysdba
```

### DBA topics demonstrated

- Instance management
- CDB/PDB administration
- User and privilege management
- Tablespace administration
- Listener/network configuration

## Phase 3 — DBA Automation

Develop reusable shell and SQL scripts.

### Planned scripts

```text
db_start.sh
db_stop.sh
db_health_check.sh
tablespace_check.sh
listener_check.sh
backup_check.sh
alertlog_check.sh
```

### Checks to automate

- Database status
- Listener status
- Tablespace utilization
- Session count
- Blocking sessions
- Invalid objects
- Archive destination usage
- Backup status
- Alert log errors

## Phase 4 — RMAN Backup and Recovery

Configure and test database backup and recovery.

### Tasks

- Full RMAN backup
- Archivelog backup
- Control file backup
- SPFILE backup
- Retention policy
- Backup validation
- Restore test
- Recovery test

Optional cloud extension:

- Store selected backup artifacts in OCI Object Storage

## Phase 5 — Monitoring

Create operational DBA monitoring.

### Monitoring areas

- Database availability
- Tablespace usage
- Session utilization
- Blocking sessions
- Invalid objects
- Listener health
- Archive log destination usage
- RMAN backup status
- Alert log errors

Output should be suitable for operational review and interview demonstration.

## Phase 6 — Terraform

Convert manually created OCI infrastructure into Infrastructure as Code.

### Planned Terraform files

```text
provider.tf
variables.tf
network.tf
compute.tf
outputs.tf
```

### Skills demonstrated

- OCI provider configuration
- Reusable variables
- Network provisioning
- Compute provisioning
- Infrastructure version control
- Repeatable cloud builds

## Phase 7 — Oracle GoldenGate

Add database replication.

### Initial design

```text
Oracle Source
    |
    v
Extract
    |
    v
Trail Files
    |
    v
Replicat
    |
    v
Oracle Target
```

### Skills demonstrated

- GoldenGate configuration
- Extract/Replicat management
- Trail file handling
- Replication validation
- Troubleshooting
- Migration planning

## Phase 8 — Multi-Cloud Expansion

Optional future extension:

```text
OCI Oracle
   |
   +--> Azure SQL / SQL Server
   |
   +--> AWS PostgreSQL
   |
   +--> Google Cloud PostgreSQL
```

The focus will be migration architecture, connectivity, replication, and operational comparison.

# First Milestone

The first milestone is intentionally limited to:

```text
OCI Oracle Linux VM
+
Oracle Database
+
GitHub repository
+
Basic DBA automation
```

This gives a complete and demonstrable first version before adding RMAN, Terraform, GoldenGate, and multi-cloud replication.

# Portfolio Outcome

When completed, the project should demonstrate:

- Oracle Database administration
- OCI infrastructure
- Linux administration
- Database networking
- RMAN backup and recovery
- DBA automation
- Monitoring and health checks
- Terraform Infrastructure as Code
- Git/GitHub workflow
- GoldenGate replication
- Cloud migration concepts
- Operational documentation

# Status

**Planned — next project after the Nginx DevOps CI/CD assignment.**
