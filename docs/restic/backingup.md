# Backing Up With Restic

{{< callout type="info" >}}
  Use `--dry-run` to see what files would be restored without actually restoring them.
{{< /callout >}}

{{% steps %}}

### Backing Up With Restic

Create a restic repository.

```shell
restic init --repo /path/to/repo
```

OR

```shell
restic -r init /path/to/repo
```

This command will ask for a password to encrypt the repository. You can copy and paste the password in the console. Save the password in a secure place, as you will need it to access the repository later. Losing password means losing access to the backups.

### Backup files to the repository

```shell
restic backup /path/to/files --repo /path/to/repo
```

OR

```shell
restic -r /path/to/repo backup /path/to/files
```

{{< callout type="info" >}}
  The bigger the directory, the longer it will take to backup. Restic will only backup the files that have changed since the last backup, so subsequent backups will be faster.
{{< /callout >}}

{{% /steps %}}

### Optional - add tags

You can add tags to the backup command to help identify the backup later. Tags are useful for organizing backups and making it easier to find specific backups.

```shell
restic -r /path/to/repo backup /path/to/files --tag tag1,tag2
```

