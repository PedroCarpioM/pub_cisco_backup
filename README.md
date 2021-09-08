# cisco_po

This desktop POC uses ansible to generate a backup file of switches config

## Hosts

the hosts must be separated by groups of regions and ip ranges

**Example**

    [aruba]
    aruba[1:24].example.com

    [hp]
    hp\*.example.com

    [switches:children]
    aruba
    hp

    [warnes]
    aruba[1:8].example.com

    [santa_cruz:children]
    warnes

    [bolivia:children]
    santa_cruz

## Group variables

As the group variables we must set the connection parameter such as network, os and credentials

**example**

    #group_vars/cisco

    ansible_connection: network_cli
    ansible_network_os: ios
    ansible_become: yes
    ansible_become_method: enable

---

    # group_vars/santa_cruz

    ansible_user: user
    ansible_password: password

## Region backup

to make a backup of a region is required to append it as a task to the playbook:

    - name: Backup all cisco switches in Santa Cruz
      hosts: cisco:&santa_cruz
      tasks:
      - name: Make backup of cisco switches in Santa Cruz
        import_tasks: tasks/cisco_backup.yml
        vars:
          region: santa_cruz


      # autor: Pedro
