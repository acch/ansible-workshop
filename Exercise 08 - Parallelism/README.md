![Ansible logo](../img/ansible.png)

# Exercise 08 — Parallelism

![Goals](../img/goals.png)

## Objective

In this exercise, students will first run a playbook against an individual host, and subsequently run the playbook against multiple hosts in parallel. Students will observe how Ansible deals with errors and learn how to recover. Then, students will modify the playbook to target different groups of hosts with different plays.

![Folder](../img/folder.png)

## Content

    Exercise 08 - Parallelism/
    ├── hosts
    ├── playbook.yml
    └── README.md

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 08*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 08*
    ```

3.  Review the file `playbook.yml`:

    ```shell
    cat playbook.yml
    ```

    Notice the managed host that is targeted for this play. Refer to the Ansible documentation (`ansible-doc`) to understand what each of the tasks does.

4.  Run the provided sample playbook and note which host(s) are targeted:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

5.  Modify the sample playbook to target the group `virtual_machines` instead of a single host.

    ```shell
    vi playbook.yml
    ```

6.  Re-run the modified playbook and observe the output. If there is an error, try to understand the problem by reviewing the console output. Consider running the playbook in verbose mode (`-v`). Review the file `hosts` to verify your inventory and contact your instructor if you're stuck.

    ```shell
    ansible-playbook -i hosts playbook.yml [-v]
    cat hosts
    ```

    _Note_: If you know what the problem is but you cannot remember the procedure to fix it refer to [Exercise 01](../Exercise%2001%20-%20SSH%20Keys).

    _Hint_: If there is an error with individual host(s) then Ansible will generate a `*.retry` file containing the names of hosts with errors. When re-running the playbook (after fixing the problem) you can limit the run to only target hosts which previously had errors, in order to save time:

    ```shell
    ansible-playbook -i hosts playbook.yml -l @playbook.retry
    ```

7.  Review the documentation of the `ansible-playbook` CLI command to understand the meaning of the `-l` parameter. Run the playbook against an individual host, only — without modifying the playbook itself:

    ```shell
    man ansible-playbook
    ansible-playbook -i hosts playbook.yml ...
    ```

    Make sure the playbook ran successfully on all managed hosts:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

8.  If the playbook ran successfully for all of your managed hosts then extend your inventory (`hosts` file) and add a new host group `webservers` including `serverX-a`, `X` being your team number (this is the only managed host that has a public interface). Do not remove this host from any other host group(s). Use the `ansible-inventory` CLI command to verify your modified inventory.

    ```shell
    vi hosts
    man ansible-inventory
    ansible-inventory -i hosts --list
    ```

    Do not run the playbook yet.

9.  Modify the playbook to install and configure nginx only(!) on hosts in group `webservers`. This is the host that has a public IP address and can act as a webserver. Do **not** install the package on all your virtual machines...

    Add another play to the end of the playbook, denoted by it's own `hosts:` pattern, targeting the host group `webservers`. Install and configure nginx on all hosts in the `webservers` group:

    ```shell
    vi playbook.yml
    ```

    Add the following tasks for the host group `webserver`. You can copy the actual tasks from the playbook of [Exercise 07](../Exercise%2007%20-%20Templates/playbook.yml).

    - Install 'nginx' package
    - Start and enable 'nginx' service
    - Open firewall port 80/TCP or HTTP service

10. Re-run the modified playbook and note which host(s) are targeted for each play:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    Inspect the console output when running the playbook and check which tasks are executed on which host(s). If the tasks on your `webservers` host fails, check the `remote_user:` option for this play.

11. 🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2009%20-%20Conditionals) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [ansible.builtin.lineinfile – Manage lines in text files](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/lineinfile_module.html)
- [Ansible Inventory - Hosts and Groups](https://docs.ansible.com/ansible/2.3/intro_inventory.html#hosts-and-groups)
