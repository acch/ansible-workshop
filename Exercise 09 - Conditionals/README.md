![Ansible logo](../img/ansible.png)

# Exercise 09 — Conditionals

![Goals](../img/goals.png)

## Objective

In this exercise, students will examine configuration differences of hosts, and modify a given sample playbook to adapt to these differences with conditional tasks.

![Folder](../img/folder.png)

## Content

    Exercise 09 - Conditionals/
    ├── hosts
    ├── playbook.yml
    └── README.md

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 09*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 09*
    ```

3.  Review the file `playbook.yml`:

    ```shell
    cat playbook.yml
    ```

    Refer to the Ansible documentation (`ansible-doc`) to understand what each of the tasks does. Do not run the playbook yet.

4.  The playbook can not be run just yet. It is missing a Jinja2 expression within the `msg:` and `when:` options of the `debug` task. You will need to develop the appropriate expression before running the playbook...

    The goal is to identify managed hosts with a disk `xvdc`, in addition to `xvda` and `xvdb`. Two of your servers have such an additional disk and one doesn't. There are Ansible facts available which contain the necessary disk information. Ansible facts are automatically defined variables which can be inspected e.g. from the command line.

    Run the following command to list all Ansible facts for all managed hosts. Read through the available facts and identify the one that lists all disk devices available to the host. Also check which managed hosts have a `xvdc` device.

    ```shell
    ansible all -i hosts -u root -m setup | less
    ```

    You can also list Ansible facts for a particular host, for example `serverX-b`:

    ```shell
    ansible serverX-b -i hosts -u root -m setup | less
    ```

    If you have found the name of the Ansible fact that contains disk device information, you can inspect just this fact using:

    ```shell
    ansible all -i hosts -u root -m setup -a filter=<fact name> | less
    ```

    You can list select Ansible facts for a particular host, for example `serverX-b`:

    ```shell
    ansible serverX-b -i hosts -u root -m setup -a filter=<fact name> | less
    ```

5.  Once you have identified the name of the Ansible fact containing information about disk devices, you can start developing a Jinja2 expression. Here is an example:

    It's usually a good idea to start with a `debug` task for testing the expression first, before using it in a condition. You can use the `msg` option of the `debug` module to show the result of an expression. The general syntax is shown below:

    ```yaml
    - debug:
        msg: "{{ <expression> }}"
    ```

    For example, to search the managed host's interfaces for an element named `eth1` you can use the Ansible fact `ansible_interfaces` (list/sequence). Here's an expression to check for the interface in this list:

    ```yaml
    - debug:
        msg: "{{ 'eth1' in ansible_interfaces }}"
    ```

    Add this expression to the `debug` module in your playbook, run the playbook and review the result. You should see that this task shows the following:

    - `true` for hosts which have a `eth1` interface
    - `false` for hosts which do not have a `eth1` interface

6.  If the sample expression works, replace it with an expression that tests if the device `xvdc` is in the Ansible fact that you've identified before. Re-run the playbook until it succeeds.

    Refer to the [online documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tests.html) for more information on Ansible tests.

    _Hint_: There are two ways to solve this problem: you can test if `xvdc` is `in` the Ansible fact (list), or you can test if the list `contains` an element `xvdc`.

7.  Once your expression for testing the existence of a `xvdc` device produces the desired result using the `msg` option in a `debug` task, try this expression in a `when` condition.

    ```shell
    vi playbook.yml
    ```

    Add a `when` statement to the `debug` task and use the same expression. Note that the `when` clause is a raw Jinja2 expression, your condition doesn't need double curly braces (`{{ ... }}`), but has to be embedded in double quotes (`" ... "`). Also consider the indentation for the `when` statement.

    Remove the expression from the `msg:` option of the debug task and replace it by `Disk xvdc found`.

    Now re-run the playbook until it produces the desired results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    You should see output similar to the following:

        TASK [Identify host with xvdc disk] ****
        skipping: [serverX-a]
        ok: [serverX-b] =>
        msg: Disk xvdc found
        ok: [serverX-c] =>
        msg: Disk xvdc found

    Notice the effect of the `when` clause — it skips managed hosts that do not match the expression, and only runs the task for the hosts matching it. This is what you need for the next step.

8.  Add a new task to your playbook using the very same `when` statement. The new task should create a `xfs` filesystem on the `xvdc` device, and it should not produce errors on hosts without such device.

    Ansible has a `filesystem` module for this purpose. Refer to the Ansible documentation (`ansible-doc`) to understand required options for the `filesystem` module. Modify the playbook and add the `filesystem` task:

    ```shell
    vi playbook.yml
    ```

    For the `filesystem` task:

    1.  Set the device to the disk device name of `xvdc`
    2.  Set the file system type to `xfs`
    3.  Use the `when` statement and the condition that disk device `xvdc` is present

    Now run the playbook:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    If you see an error "Device ... not found" then something's wrong with your `when` condition — check your Jinja2 expression.

    _Note_: You can delete the `debug` task used for developing the expression.

9.  After creating the new filesystem you will want to mount it... Ansible has a `mount` module for this purpose. As usual, refer to the Ansible documentation (`ansible-doc`) for details and define a `mount` task.

    ```shell
    vi playbook.yml
    ```

    For the `mount` task:

    1.  Set the mount point to `/mnt/test`
    2.  The source of the file system is the disk device name of `xvdc`
    3.  The file system type must be `xfs`
    4.  Make sure the file system gets `mounted`

    Remember to specify the `when` condition to mount the file system only on hosts that have the source device. Once again, if you see an error "Device ... not found" then something's wrong with your `when` condition.

    Once the mount task is created, re-run the playbook again:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    You can check if the file system is mounted on the managed hosts by running the following commands (`X` is the number of your team and `Y` is the host that has a `xvdc` device):

    ```shell
    ssh root@serverX-Y mount
    ssh root@serverX-Y df -h
    ssh root@serverX-Y lsblk
    ```

    You should see output similar to this, whereby the filesystem mount point `/mnt/test` may be different depending on the options you had used with the `mount` task.

        NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
        xvdc  253:16   0  10G  0 disk /mnt/test

    If the filesystem is not mounted, then make sure that the `state` option is set to `mounted`.

10. _Optional_: In many use cases it's desirable to perform additional verification and ensure that a set of tasks was really successful. In this given scenario, it might be worth checking if the new filesystem is actually usable. This could be achieved, for example, by writing a test file to the newly created and mounted filesystem — if this task succeeds then one can be sure that the filesystem is usable.

    As usual, there's different ways to create files with Ansible. You can use the `file` module with `state: touch` to create an empty file (inode). Alternatively, you can use the `copy` module with the `content:` parameter to create a file with actual data. Refer to the Ansible documentation (`ansible-doc`) for details. In either case, make sure that you're creating the test file in a path inside the newly created filesystem. This will add an extra level of confidence to your playbook...

    When the playbook ran successfully, then check if the file is present on the managed host:

    ```shell
    ssh root@serverX-Y ls /mnt/test
    ```

11. 🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2010%20-%20Includes) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [Ansible - Tests](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tests.html)
- [Ansible - Conditionals](https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html)
- [community.general.filesystem – Makes a filesystem](https://docs.ansible.com/ansible/latest/collections/community/general/filesystem_module.html)
- [ansible.posix.mount – Control active and configured mount points](https://docs.ansible.com/ansible/latest/collections/ansible/posix/mount_module.html)
- [ansible.builtin.file – Manage files and file properties](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html)
- [ansible.builtin.copy – Copy files to remote locations](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html)
