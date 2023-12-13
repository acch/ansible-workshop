![Ansible logo](../img/ansible.png)

# Exercise 05 — Variables

![Goals](../img/goals.png)

## Objective

In this exercise, students will learn and apply different methods for defining and using variables in playbooks.

> 1.  Run a simple playbook to create a user
> 2.  Modify the playbook to create a user using playbook variables (`vars` statement)
> 3.  Create a user by defining a variable on the command line
> 4.  Create a user using a more complex variable
> 5.  Define variables in a separate file (`vars_files` statement) and modify the playbook to use the variables from that file

![Folder](../img/folder.png)

## Content

    Exercise 05 - Variables/
    ├── hosts
    ├── playbook.yml
    └── README.md

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 05*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 05*
    ```

3.  Review the file `playbook.yml` and determine the name of the Ansible module that is being used, as well as the name of the user account that is being created.

    ```shell
    cat playbook.yml
    ```

    Refer to the Ansible documentation (`ansible-doc`) to understand what each of the tasks does.

4.  Run the playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    On your managed host (`serverX-a`, `X` being your team number), check if `user1` has been created:

    ```shell
    ssh root@serverX-a getent passwd user1
    ```

5.  Modify the playbook by defining a variable `username` with the value `user2` in the `vars:` section, and replacing the name of the user in the task with the name of the variable.

    ```shell
    vi playbook.yml
    ```

    1.  Define a variable `username: user2` within section `vars:`

    2.  Replace the user name (`user1`) in the task with the variable `"{{ username }}"`

6.  Re-run the modified playbook again and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    On your managed host (`serverX-a`, `X` being your team number), check if `user2` has been created:

    ```shell
    ssh root@serverX-a getent passwd user2
    ```

7.  With the existing playbook, create a user named `user3` by passing a variable with the `ansible-playbook` CLI command.

    Make yourself familiar with syntax of the `ansible-playbook` command, especially the parameter to pass 'extra variables' into the playbook. The extra variable to be passed into the playbook should be `username=user3`.

    ```shell
    ansible-playbook --help
    ansible-playbook -i hosts <parameter> username=user3 playbook.yml
    ```

    On your managed host (`serverX-a`, `X` being your team number), check if `user3` has been created:

    ```shell
    ssh root@serverX-a getent passwd user3
    ```

    Did you notice that variables defined with the `ansible-playbook` CLI command take precedence over (override) the variables defined in your playbook?

8.  Create a new user `user4`, member of group `users`, by using a more complex variable. The variable `username` should include a mapping for keys `name` and `group` of the user.

    ```shell
    vi playbook.yml
    ```

    1.  In section `vars:` remove `username: user2`

    2.  Add a variable with name `username` as mapping with two keys. For example, to define a variable `mymapvar` with two keys `key1` and `key2` use the following syntax:

        ```yaml
        mymapvar:
          key1: value1
          key2: value2
        ```

        In the example above you have to replace the name of the variable and the keys and values of the mapping. Beware of the indentation.

    3.  In task `user` use the value of the variable that contains the user's name with the `name:` option. Referring to the example above, the value of key `key1` can be referenced with `"{{ mymapvar.key1 }}"`, and the value of key `key2` can be referenced with `"{{ mymapvar.key2 }}"`.

    4.  Find the option of the `user` module that allows assigning the user to a custom group using `ansible-doc user`. Add this option to the `user` task and use the value of the variable that contains the user's group.

9.  Re-run the modified playbook again and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    On your managed host (`serverX-a`, `X` being your team number), check if `user4` in group `users` has been created:

    ```shell
    ssh root@serverX-a getent passwd user4
    ```

10. Modify the playbook once more by moving and adjusting the variable definitions to a separate file. The new name of the user should be `user5` and the primary group should be `users`.

    ```shell
    vi playbook.yml
    ```

    1.  Delete section `vars:` including the line defining variable `username`. This variable will now be defined in a separate file with different values. You may copy this variable to clipboard prior to deleting it.

    2.  Add the name of the variables file to the playbook: `vars_files: myvars.yml`. Add this statement at the same place in the file where you deleted the `vars:` section.

    Next, create a new file `myvars.yml`:

    ```shell
    vi myvars.yml
    ```

    1.  Define the variable `username` as mapping with two keys, whereby the user's name should be `user5` and the primary group should be `users`. Refer to the previous step where you created a more complex mapping. You can use the same syntax.

        ```yaml
        ---
        mymapvar:
          key1: value1
          key2: value2
        ```

        _Note_: You do not need to indent the first line in the variables file. Further, you must not use the `vars:` statement in the variables file.

11. Re-run the modified playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    On your managed host (`serverX-a`, `X` being your team number), check if `user5` has been created:

    ```shell
    ssh root@serverX-a getent passwd user5
    ```

12. 🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2006%20-%20Loops) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [Ansible - Using Variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html)
- [ansible.builtin.user – Manage user accounts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html)
