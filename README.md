# DR Replica Setup Guide

## Overview
Standard configuration and setup guidelines for Disaster Recovery (DR) machine replication.
For creating DR replica, you should go to the source VM note and add these following flags.

---

## Default Settings
Use the following default parameters for standard DR replica configurations:

`repResync=5,repTargets=eu-ml,repTargetSuffix=replica,repSnapsHourly=2,repSnapsDaily=4,repSnapsWeekly=1,rep=on`

======================================================================================================================================================================

```ini
repResync=5
repTargetSuffix=replica
repSnapsHourly=2
repSnapsDaily=4
repSnapsWeekly=1
rep=on
Configuration Parameters Reference
## Parameter	Description
repTargets-	Target data centers list (e.g., il-pt, il-rh, eu-fr)
repResync-	Replication Re-sync interval in minutes (Range: 5-1440)
repSnapshotsHourly-	Hourly snapshots to keep for the last 24 hours
repSnapshotsDaily-	Daily snapshots to keep for the last 7 days
repSnapshotsWeekly-	Weeks to keep 1 snapshot (starting from 7 days ago)
repTargetSuffix-	Replication suffix added on target (e.g., adds vmname-[il-pt]-dr)
## VLAN Modification Format
To modify VLAN routing between the origin and destination data centers, use the following syntax structure:

repVlanModify{data center}={VLAN in origin data center}>{VLAN in destination data center}

Example:

repVlanModifyil-rh=282>307

Standard Configuration String
repResync=5,repTargets={datacenter},repTargetSuffix=replica,repSnapsHourly=2,repSnapsDaily=4,repSnapsWeekly=1,rep=on
