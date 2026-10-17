# Snapshots

## Browse the snapshots of the repository

{{< callout type="info" >}}
  Use `--dry-run` to see what files would be restored without actually restoring them.
{{< /callout >}}

```shell
restic -r /path/to/repo snapshots
```

This will list all the snapshots in the repository, along with their IDs, timestamps, and paths.

## Mount a snapshot

You can mount a snapshot as a read-only file system to browse its contents without restoring it.

Create a mount point directory where you want to mount the snapshot:

```shell
mkdir /path/to/mountpoint
```

Then, use the following command to mount the snapshot:

```shell
restic -r /path/to/repo mount /path/to/mountpoint --snapshot <snapshot-id>
```

{{< callout type="info" >}}
  You can use @latest to refer to the latest snapshot.
{{< /callout >}}

You can then browse the contents of the snapshot in the mount point directory. To unmount the snapshot, do a control-C in the terminal where you mounted the snapshot or use the `fusermount` command:

```shell
fusermount -u /path/to/mountpoint
```

## Restore files from a snapshot

To restore files from a snapshot, you can use the `restore` command. Specify the snapshot ID and the target directory where you want to restore the files:

```shell
restic -r /path/to/repo restore <snapshot-id> --target /path/to/restore
```

This will restore the files from the specified snapshot to the target directory. If you want to restore files to their original location, you can omit the `--target` option:

```shell
restic -r /path/to/repo restore <snapshot-id>
```

{{< callout type="warning" >}}
  Be careful when restoring files to their original location, as it may overwrite existing files.
{{< /callout >}}

## Remove old snapshots

To remove old snapshots, you can use the `forget` command. This command allows you to specify which snapshots to keep based on various criteria, such as age or number of snapshots to keep.

```shell
restic -r /path/to/repo forget --keep-last 5 --prune
```

This command will keep the last 5 snapshots and prune the repository to remove any data that is no longer referenced by any snapshot.

You can also use other options like `--keep-daily`, `--keep-weekly`, and `--keep-monthly` to specify how many snapshots to keep based on their age.

## Check the integrity of the repository

To check the integrity of the repository, you can use the `check` command. This command verifies that all data in the repository is intact and can be restored.

```shell
restic -r /path/to/repo check
```

This will check the integrity of the repository and report any issues found. If there are any problems, you may need to run the `rebuild-index` command to fix them.

## Forget policies

Restic supports forget policies, which allow you to define rules for how long to keep snapshots based on their age or other criteria. This helps manage the size of the repository and ensures that old backups are removed automatically.
You can define forget policies in a configuration file or use command-line options when running the `forget` command. For example, you can create a file named `forget-policy.txt` with the following content:

```text
--keep-last 5
--keep-daily 7
--keep-weekly 4
--keep-monthly 3
--prune
```

Then, you can run the `forget` command with the `--policy` option:

```shell
restic -r /path/to/repo forget --policy forget-policy.txt
```

This will apply the forget policy defined in the file to the repository, keeping the specified number of snapshots based on their age and pruning the repository accordingly.

