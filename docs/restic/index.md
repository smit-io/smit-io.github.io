# Restic Backups

## Introduction

Restic is a fast, secure, and efficient backup program written in Go. It provides encryption, deduplication, and compression. It also has support for various backends, including local storage, SFTP, and cloud providers such as Amazon S3, Google Cloud Storage, and Backblaze B2.

Restic is very well documented and has a good [documentation][documentation]. Always use the stable branch of restic.

## How is restic used to backup the data

Restic uses something called a "repository" to store the backups. A repository is a directory that contains all the data and metadata needed to restore the backups. The repository can be stored on a local disk, a network share, or a cloud storage service.

Each time you run a backup, restic creates a new snapshot of the data. A snapshot is a point-in-time representation of the data in the repository. Restic uses deduplication to store only the changes made since the last backup, which saves space and time.

You can also use restic to restore files from a backup. You can restore files to their original location or to a different location. Restic also supports restoring files to a specific point in time, which is useful if you need to recover from accidental deletions or modifications.

Backups can be scheduled using cron jobs or other scheduling tools to automate the process. Restic also supports incremental backups, which means that only the changes made since the last backup are stored, reducing the amount of data transferred and stored. Do this with caution, as it can lead to data loss if not managed properly.

Restic can also mount a snapshot as a read-only file system, allowing you to browse and access files in the backup without restoring them. This is useful for quickly checking the contents of a backup or recovering specific files without restoring the entire snapshot.

You can also remove old snapshots to free up space in the repository. Restic provides commands to prune and delete snapshots based on various criteria, such as age or number of snapshots to keep. You can also use restic to check the integrity of the repository, ensuring that the data is not corrupted and can be restored when needed.

Restic also supports forget policies, which allow you to define rules for how long to keep snapshots based on their age or other criteria. This helps manage the size of the repository and ensures that old backups are removed automatically.

Get started with restic by installing it on your system. You can find the installation instructions in the [installation guide](installation).

<!--more-->

{{< cards >}}
  {{< card link="installation" title="Installation" icon="document-duplicate" >}}
  {{< card link="configuration" title="Configuration" icon="adjustments" >}}
  {{< card link="markdown" title="Markdown" icon="markdown" >}}
  {{< card link="syntax-highlighting" title="Syntax Highlighting" icon="sparkles" >}}
  {{< card link="latex" title="LaTeX" icon="variable" >}}
  {{< card link="diagrams" title="Diagrams" icon="chart-square-bar" >}}
  {{< card link="shortcodes" title="Shortcodes" icon="template" >}}
  {{< card link="deploy-site" title="Deploy Site" icon="server" >}}
{{< /cards >}}

[documentation]: https://restic.readthedocs.io/en/stable

