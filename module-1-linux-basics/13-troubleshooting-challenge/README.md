# Task 13 — Troubleshooting Challenge

## Scenario

A Bash backup script was working yesterday. Today, the cron job appears to run, but no backup is created. The following steps can be used to troubleshoot the issue.

## 1. Check Cron Job

First, I check the cron job using:

```bash
crontab -l
```

This lists all scheduled cron jobs. I verify that the backup cron job is present and that its configuration is correct. If everything looks correct, I move to the next step.

## 2. Check Paths

Next, I check the path of the backup Bash script defined in the cron job. I compare it with the actual path of the script and make sure that the script exists at the specified location.

## 3. Check Permissions

Then, I check the permissions of the script using:

```bash
ls -l
```

I make sure that the script has execute permission. I also verify that the user running the cron job has permission to access the required files and directories.

## 4. Manual Execution

If the permissions are correct, I execute the backup script manually using the same command used by the cron job.

This helps determine whether the problem is with the backup script itself or with the cron environment.

Example:

```bash
/path/to/backup.sh
```

## 5. Check Exit Code

After executing the script, I check the exit code using:

```bash
echo $?
```

* Exit code `0` means the script completed successfully.
* A non-zero exit code means that the script reported an error.

If the exit code is non-zero, I investigate the script and its commands further.

## 6. Check Disk Space

Finally, I check the available disk space using:

```bash
df -h
```

If the disk is full, the backup file may not be created even if the script and cron job are working correctly.

## Conclusion

By checking the cron job, paths, permissions, manual execution, exit codes, and disk space, I can systematically identify why the backup is not being created and determine whether the issue is related to the script, cron configuration, permissions, or system resources.
