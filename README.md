# Backup Verification Strategy

## Backup Verification

No backup is backup until you can successfully restore your application from it. So, it is necessary to ensure that your application is recoverable from the backed up data. KubeStash now supports automatic verification of your backed up data where it automatically spins up a temporary instance of your application, restores data into it, runs some checks, and then removes the temporary instance.

## Requirements

- Support automatic verification of backed up data.
- Support verification of a subset of the sessions.
- Support verification after every few backups / triggering a cronjob.
- Support triggering backup verification manually. (CLI)
- Support application specific verification logic.
- Verification can be one of these:
    - Just verify that the data can be restored from the backup. No additional checks are necessary 
    - There might be some standard checks for certain application to ensure that the backed up data is consistent
    - User configurable verification logic
- User should provide the necessary PVCs specification and runtimeSettings for the verification job
- BackupVerificationSession should track the time took to complete the backup verification
- Backup verifier job may need to recover from multiple repository to have a complete restore

## Q/A

- Will trigger verification after every few backups / cron schedule? -> Cron schedule
- How to run verification? -> By a verifier job 
- Who will trigger the verification job? -> A cronjob will create `BackupVerificationSession`
- Should we only verify the latest Snapshot or user should be able to specify to verify last X snapshots? -> Latest
- What if verification fails? Should we stop taking any further backup? -> We will not stop backup (as the verification may go wrong), we will notify user somehow about failed backup verification.
- Should a Snapshot have a field that denotes that this Snapshot has been verified? -> Maybe
- Who will trigger the verification? -> BackupConfig will create cronjob 
- Should we have a different process to verify workload backup?