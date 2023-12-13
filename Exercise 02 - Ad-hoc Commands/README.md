![Ansible logo](../img/ansible.png)

# Exercise 02 — Ad-hoc Commands

![Goals](../img/goals.png)

## Objective

In this exercise, students will review and use a basic inventory file to verify connectivity with their managed host by running Ansible ad-hoc commands.

![Folder](../img/folder.png)

## Content

    Exercise 02 - Ad-hoc Commands/
    ├── hosts
    └── README.md

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 02*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 02*
    ```

3.  Review the inventory file `hosts` and verify that the hostname listed is indeed the managed host you exchanged the SSH key with:

    ```shell
    cat hosts
    ```

4.  Make yourself familiar with the syntax of the `ansible` CLI command used to run single task 'playbooks' against hosts:

    ```shell
    man ansible
    ```

5.  Run the following ad-hoc command against your managed host:

    ```shell
    ansible -i hosts -u root -m ping virtual_machines
    ```

    Review the man page of the `ansible` CLI command once more in order to understand the meaning of the parameters (`-i`, `-u`, `-m` and `virtual_machines`).

6.  Investigate available options for the `ping` module:

    ```shell
    ansible-doc ping
    ```

    Alternatively, you can use the [online documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html) in a web browser.

7.  Modify and re-run the ad-hoc command which you previously ran in step 5. Supply module arguments to make the `ping` module return your personal first name instead of a generic string ('pong'). The syntax for passing module arguments is `-a parameter=value`. Available parameters of the `ping` module are listed in the [documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html).

    Note that you might also need to enable verbose mode (`-v`) to display the actual output returned by the module.

8.  🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2003%20-%20First%20Playbook) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [ansible.builtin.ping – Try to connect to host, verify a usable python and return pong on success](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/ping_module.html)
