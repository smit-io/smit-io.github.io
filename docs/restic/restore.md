# Restore Files with Restic

## Restore Files with Restic

To restore files from a restic repository, you can use the `restore` command. This command allows you to restore files from a specific snapshot to a target directory or to their original location.

### Restore Files to a Target Directory

{{< callout type="info" >}}
  Use `--dry-run` to see what files would be restored without actually restoring them.
{{< /callout >}}

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore
```

This command will restore the files from the specified snapshot to the target directory. If you want to restore files to their original location, you can omit the `--target` option:

```shell
restic -r /path/to/repo restore <snapshot-id>
```

{{< callout type="warning" >}}
  Be careful when restoring files to their original location, as it may overwrite existing files.
{{< /callout >}}

### Restore Specific Files

If you want to restore specific files from a snapshot, you can specify the file paths after the `restore` command:

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore -- include /path/to/file1 -- include /path/to/file2
```

This command will restore only the specified files from the snapshot to the target directory.

### Restore All Files from a Snapshot

To restore all files from a snapshot, you can use the following command:

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore -- all
```

This command will restore all files from the specified snapshot to the target directory.

### Restore Files with Tags

You can also restore files with specific tags. Use the `--tag` option to specify the tags you want to restore:

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore --tag tag1,tag2
```

This command will restore files from the specified snapshot that have the specified tags to the target directory.

### Restore Files with Filters

You can use filters to restore files that match specific patterns. Use the `--include` and `--exclude` options to specify the patterns:

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore --include "*.txt" --exclude "*.log"
```

This command will restore all `.txt` files from the specified snapshot to the target directory, excluding any `.log` files.

### Restore Files with Dry Run

If you want to see what files would be restored without actually restoring them, you can use the `--dry-run` option:

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore --dry-run
```

This command will show you the files that would be restored without actually performing the restore operation.

{{< callout type="info" >}}
  You can use `@latest` to refer to the latest snapshot.
{{< /callout >}}

### Remove Old Snapshots

To remove old snapshots, you can use the `forget` command. This command allows you to specify which snapshots to keep based on various criteria, such as age or number of snapshots to keep.

```shell
restic -r /path/to/repo forget --keep-last 5 --prune
```

