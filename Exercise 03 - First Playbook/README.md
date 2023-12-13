![Ansible logo](../img/ansible.png)

# Exercise 03 — First Playbook

![Goals](../img/goals.png)

## Objective

In this exercise, students will first run a sample Ansible playbook against their managed host, and subsequently modify and extend the playbook.

![Folder](../img/folder.png)

## Content

    Exercise 03 - First Playbook/
    ├── hosts
    ├── playbook.yml
    └── README.md

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 03*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 03*
    ```

3.  Review the playbook file `playbook.yml` and determine the tasks being executed:

    ```shell
    cat playbook.yml
    ```

4.  Make yourself familiar with the syntax of the `ansible-playbook` CLI command used to run playbooks against hosts:

    ```shell
    man ansible-playbook
    ```

5.  Run the provided sample playbook using the `ansible-playbook` CLI command and interpret the output of each task:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

6.  Log in to your managed host (`serverX-a` whereby `X` is your team number) and verify that Ansible has successfully installed the desired package(s):

    ```shell
    ssh root@serverX-a
    rpm -qa | grep nginx
    ```

    On the managed host, check if the `nginx` service is running and then disconnect the session. Do **not** start the service manually.

    ```shell
    systemctl status nginx
    exit
    ```

    Note that the nginx service is not started.

7.  Modify the sample playbook and add a new task to start and enable the `nginx` service (web server). Add this task to the end of the playbook. The (generic) module which can be used for this is named `service`. Use the command `ansible-doc` to identify and understand the required options of the `service` module.

    ```shell
    ansible-doc service
    vi playbook.yml
    ```

    _Hint_: The Ansible documentation contains useful examples, usually towards the end of the page. It often comes down to copying, adopting, and combining examples for the task at hand.

    The general syntax for tasks is the following; pay close attention to indentation:

    ```yaml
    - name: Description of the task
      module_name:
        option1: value1
        option2: value2
        ...
    ```

    Remember that you should start and enable the service named `nginx`.

    If you are not familiar with the `vi` editor then contact your instructor.

8.  Re-run the modified playbook and, once successful, log in to your managed host to verify that Ansible has successfully started the service:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ssh root@serverX-a
    systemctl status nginx
    exit
    ```

9.  Run the playbook once more, and enable verbose mode (`-v`) to make Ansible show further details on the steps performed, along with their results:

    ```shell
    ansible-playbook -i hosts playbook.yml -v
    ```

10. 🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2004%20-%20Second%20Playbook) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [ansible.builtin.package – Generic OS package manager](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html)
- [ansible.builtin.service – Manage services](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html)
