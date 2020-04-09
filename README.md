# Backup role

This role follow a very strict pattern for backup:

- Run a shell script to generate the backup artifacts and store then in `/data/backup`
- Encrypt the backups using the GPG key already installed
- Upload the encrypted backup to S3
- Delete all local copies

> If something fails, the backup stop as is, so you can manually recover.

> The backup folder is cleaned BEFORE the backup, so don't store important
> things there.

> The tasks run after the roles on the demo playbook, keep that in mind.

# Variables

- `backup_script` - The backup script as text (You can put directly the shell
  script on the variable)
- `gpg_email` - Respective email to use as in (`--recipient`) for GPG encription
- `bucket` - Bucket name for uploading the file
- `folder` - Inside the bucket, which folder you will store your files

As obvious, you need your AWS id and secret to perform the uploads to S3, these
follow Ansible standards.

# Example playbook

```yml
- hosts: all
  vars:
    backup_script:
      echo nothing > /data/backup/backup_$(date)
      echo nothing2 > /data/backup/backup2_$(date)
    gpg_email: opsxcq@strm.sh
    bucket: backups
    folder: demo
  tasks:
  - name: My things
    debug: msg="Some additional task here"
  roles:
    - opsxcq.host_backup
```

# Requirements file

```yml
- src: git+https://github.com/opsxcq/ansible-role-host-backup.git
  name: "opsxcq.host_backup"
```
