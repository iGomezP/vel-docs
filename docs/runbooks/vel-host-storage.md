# VEL Host Storage

## Purpose

This runbook documents the physical storage layout of the Vakzor Enterprise Lab host and the intended responsibility of each storage device.

The goal is to separate operating system workloads, persistent application data, and local backups while keeping the storage design simple and recoverable.

## Storage Layout

| Device          | Type     | Mount Point        | Purpose                                                                                        |
| --------------- | -------- | ------------------ | ---------------------------------------------------------------------------------------------- |
| NVMe 512 GB     | NVMe SSD | `/`, `/home`       | Fedora Server, Docker runtime, repositories, active databases, and latency-sensitive workloads |
| SATA SSD 512 GB | SSD      | `/srv/vel/data`    | Persistent application data, object storage, artifacts, shared data, and exports               |
| SATA HDD 750 GB | HDD      | `/srv/vel/backups` | Local backups, configuration exports, and archives                                             |

## Operational Data

The secondary SSD is mounted at:

```text
/srv/vel/data
```

Initial directory structure:

```text
/srv/vel/data/
├── artifacts/
├── exports/
├── minio/
└── shared/
```

The filesystem is `ext4`.

The mount is configured persistently in `/etc/fstab` using its filesystem UUID and the `nofail` option.

## Local Backups

The HDD is mounted at:

```text
/srv/vel/backups
```

Initial directory structure:

```text
/srv/vel/backups/
├── archive/
├── configs/
├── minio/
├── mongodb/
├── postgres/
└── sqlserver/
```

The filesystem is `ext4`.

The mount is configured persistently in `/etc/fstab` using its filesystem UUID and the `nofail` option.

## Storage Responsibilities

### NVMe

Use for:

- Fedora Server.
- Docker runtime.
- Active PostgreSQL databases.
- Active MongoDB databases.
- Active SQL Server databases.
- Source repositories.
- Build workloads.
- Other latency-sensitive services.

Active databases should remain on NVMe unless a future capacity or architecture decision explicitly changes this policy.

### SSD Data Volume

Use for:

- MinIO object storage.
- Build artifacts.
- Application exports.
- Shared persistent files.
- Future persistent application data that does not require NVMe latency.

### HDD Backup Volume

Use for:

- PostgreSQL backups.
- MongoDB backups.
- SQL Server backups.
- MinIO backups.
- Configuration exports.
- Local archives.

The HDD must not be used as the primary storage location for active databases.

## Health Validation

Before reuse, all storage devices were inspected using SMART.

The secondary SSD reported:

- SMART overall health passed.
- No reallocated sectors.
- No pending sectors.
- No uncorrectable sectors.
- No interface CRC errors.

The backup HDD reported:

- SMART overall health passed.
- No reallocated sectors.
- No pending sectors.
- No uncorrectable sectors.
- Short SMART self-test completed successfully.
- Extended SMART self-test completed successfully.

The HDD has significant historical operating hours and historical ATA error entries. For this reason, it is approved only as a local backup and archive device.

It must not be considered the only copy of important data.

## SMART Monitoring

`smartd` is enabled and running on the host.

It monitors:

- Two ATA/SATA devices.
- One NVMe device.

Check the service with:

```bash
sudo systemctl status smartd
```

Inspect an individual device with:

```bash
sudo smartctl -a /dev/sda
sudo smartctl -a /dev/sdb
sudo smartctl -a /dev/nvme0n1
```

## Mount Validation

Validate the filesystem configuration with:

```bash
sudo findmnt --verify --verbose
```

Verify the VEL mounts with:

```bash
findmnt /srv/vel/data
findmnt /srv/vel/backups
```

Check capacity with:

```bash
df -hT /srv/vel/data /srv/vel/backups
```

## Recovery Notes

The storage mounts use filesystem UUIDs instead of device names such as `/dev/sda` or `/dev/sdb`.

This prevents mount configuration from depending on kernel device enumeration order.

Both secondary mounts use the `nofail` option so that failure or absence of a secondary storage device does not prevent Fedora Server from booting.

If a secondary disk becomes unavailable:

1. Verify the device is detected with `lsblk`.
2. Inspect SMART health.
3. Verify the filesystem UUID.
4. Check `/etc/fstab`.
5. Attempt the mount manually.
6. Review system logs before restoring services that depend on the device.

## Backup Policy

`/srv/vel/backups` is a local backup location only.

A backup stored on the same physical host does not protect against:

- Host failure.
- Theft.
- Electrical damage.
- Filesystem-wide incidents.
- Catastrophic hardware loss.

Before VEL stores production or client data, an encrypted off-host backup strategy must be implemented.

## Current Migration State

The storage foundation is complete.

No active database has been migrated from the NVMe.

MinIO is the first candidate for migration to `/srv/vel/data/minio`.

Any service migration must be performed separately and must include:

- Backup.
- Controlled shutdown.
- Data migration.
- Ownership and SELinux validation.
- Service restart.
- Functional validation.
- Recovery procedure.
