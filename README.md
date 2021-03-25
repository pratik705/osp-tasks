# osp-tasks

## Update the variables as per your requirement
- `vars/overcloud-backup.yaml`

## Run the playbook:
```
[stack@undercloud ~]$ source ~/stackrc
[stack@undercloud ~]$ export TRIPLEO_PLAN_NAME=$(openstack stack list -c 'Stack Name' -f value)

## Prep the controllers:
[stack@undercloud ~]$ ansible-playbook -i /usr/bin/tripleo-ansible-inventory  -e @vars/overcloud-backup.yaml main.yaml -f 30 -b --tags prep

## Backup the overcloud database:
[stack@undercloud ~]$ ansible-playbook -i /usr/bin/tripleo-ansible-inventory  -e @vars/overcloud-backup.yaml main.yaml -f 30 -b --tags overcloud_db

## Backup the Controller filesystem:
[stack@undercloud ~]$ ansible-playbook -i /usr/bin/tripleo-ansible-inventory  -e @vars/overcloud-backup.yaml main.yaml -f 30 -b --tags overcloud_fs

## Backup the Controllers using REAR:
[stack@undercloud ~]$ ansible-playbook -i /usr/bin/tripleo-ansible-inventory  -e @vars/overcloud-backup.yaml main.yaml -f 30 -b --tags rear_backup
```

**NOTE:** 
  - REAR backup requires NFS share. So you need to provide the NFS details in the `vars/overcloud-backup.yaml` and run the playbook with `--tags rear_backup`.
  - By-default, overcloud DB and Controller FS backup's will be stored locally. If its required to store it on the NFS server then first run the playbook with `prep` tag followed by `overcloud_db/overcloud_fs`. 
