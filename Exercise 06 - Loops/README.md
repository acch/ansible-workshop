![Ansible logo](../img/ansible.png)

# Exercise 06 — Loops

![Goals](../img/goals.png)

## Objective

In this exercise, students will learn how to use loops to iterate through lists (sequences).

> 1.  Run a simple playbook to delete users that have been created previously
> 2.  Modify the playbook to define lists in playbook variables (`vars` statement), and use loops to create multiple users
> 3.  Modify the playbook to create multiple users with more complex characteristics
> 4.  Modify the playbook and move the variables to a variable file and add more user properties (group and description)

![Folder](../img/folder.png)

## Content

    Exercise 06 - Loops/
    ├── hosts
    ├── playbook.yml
    └── README.md

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 06*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 06*
    ```

3.  Review the file `playbook.yml` and investigate the `loop` statement. Determine the names of the users that are subject for processing.

    ```shell
    cat playbook.yml
    ```

    Refer to the Ansible documentation (`ansible-doc`) to understand what each of the tasks does. Specifically notice the `state` option within the `user` task. What does it mean?

4.  Run the playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    On your managed host (`serverX-a`, `X` being your team number), check if users have been deleted:

    ```shell
    ssh root@serverX-a "getent passwd | grep user"
    ```

    All users with name `user*` should have been deleted.

5.  Modify the playbook by defining user names in a list variable `all_users`, and using this variable in the loop. Transform the following syntax:

    ```yaml
    tasks:
      - name: ...
        ...
        loop:
          - element1
          - element2
          - element3
    ```

    ...into the following:

    ```yaml
    vars:
      all_users:
        - element1
        - element2
        - element3

    tasks:
      - name: ...
        ...
        loop: "{{ all_users }}"
    ```

    Explicitly defining longer lists at the beginning of your play (within section `vars:`) makes the playbook easier to read and maintain, especially if it gets longer when you have many tasks.

6.  Furthermore, modify the `user` task with the loop. Change the task so that user accounts are created rather than deleted. Refer to the documentation (`ansible-doc`) for details. Don't forget to update the task `name:` to properly describe what it's doing (e.g. 'Add users in loop').

    Next, delete elements from the list until only two elements, 'user1' and 'user2', remain. We want to create these two accounts, only.

7.  Run the modified playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    When the playbook ran successfully, then check if users have been created on your managed host (`serverX-a`, `X` being your team number):

    ```shell
    ssh root@serverX-a "getent passwd | grep user"
    ```

    There should be two users present on your managed host: 'user1' and 'user2'.

8.  Modify the playbook again by changing the list variable `all_users` to contain mappings rather than plain strings. For example, to define a list variable `cars` with two elements `name: Audi` and `name: Volkswagen` the following syntax can be used:

    ```yaml
    vars:
      cars:
        - name: Audi
        - name: Volkswagen
    ```

    The following example illustrates the concept of using a list of mappings in a loop. This example iterates over the list defined in variable `cars` and prints the `name` key of each element in this list. No need to implement this — it is solely to explain the concept:

    ```yaml
    - name: Using list of mappings in a loop
      debug:
        msg: "{{ item.name }}"
      loop: "{{ cars }}"
    ```

    Similarly, you can use the list variable `all_users` with two elements `name: user3` and `name: user4` and use the loop to create two new user accounts.

    ```shell
    vi playbook.yml
    ```

    1.  In section `vars:` change the name of the users to `user3` and `user4`. User names 'user1' and 'user2' are no longer required.

    2.  In section `vars:` change the list variable to define a `name` key for each of the elements: `name: user3` and `name: user4`.

    3.  In task `user` select the value of the `name` key from the loop variable: `"{{ item.name }}"`.

9.  Run the modified playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    When the playbook ran successfully, then check if users have been created on your managed host (`serverX-a`, `X` being your team number):

    ```shell
    ssh root@serverX-a "getent passwd | grep user"
    ```

    There should be two new users present on your managed host: 'user3' and 'user4'.

10. Modify the playbook once more to create two new users 'user5' and 'user6', and add the users to the group 'users'. The group will be defined as an additional key within the list variable.

    For example, to define a list variable `cars` with two elements, where each element has two keys `name` and `color`, the following syntax can be used:

    ```yaml
    vars:
      cars:
        - name: Audi
          color: black
        - name: Volkswagen
          color: white
    ```

    The following example illustrates the concept of using list variables in loops. This example iterates over the list defined in variable `cars` and prints both keys of each element in this list. No need to implement this — it is solely to explain the concept:

    ```yaml
    - name: Using list variable in a loop
      debug:
        msg: "{{ item.name }} - {{ item.color }}"
      loop: "{{ cars }}"
    ```

    Similarly, you can use the list variable `all_users` with two elements. Each element should have two keys: `name` for the user's name, and `group` for the user's primary group. Subsequently, the `name` key is used as value for option `name`, and the `group` key is used as value for option `group` of the `user` task.

    ```shell
    vi playbook.yml
    ```

    1.  In section `vars:` expand the list variable `all_users` with the following elements. User names 'user3' and 'user4' are no longer required.

        ```yaml
        - name: user5
          group: users
        - name: user6
          group: users
        ```

    2.  In task `user` add the option `group: "{{ item.group }}"`. Ensure that the option `name: "{{ item.name }}"` is still present.

11. Run the modified playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    When the playbook ran successfully, then check if users have been created in the appropriate group:

    ```shell
    ssh root@serverX-a "getent passwd | grep user"
    ```

    The new users should have been create in the group 'users' with group ID 100.

12. Move the list variable into a separate variable file and add more mappings for 'user5' and 'user6' with a comment for each. Define a new key `comment` to add a description for each user in the list.

    ```shell
    vi playbook.yml
    ```

    1.  In section `vars:` add another key `comment: userX comment` for each element. The actual comment text should be different for each user.

    2.  In task `user` add the option `comment: "{{ item.comment }}"`.

    3.  Now copy the contents of section `vars:` to the clipboard, remove the `vars:` section including the list variable `all_users` and its elements and replace it with the variable file reference: `vars_files: loopvars.yml`

    4.  Create a new file `loopvars.yml`:

        ```shell
        vi loopvars.yml
        ```

        Paste the list from the clipboard into the new file and adjust it by removing the `vars` statement and adjusting the indentation.

13. Run the modified playbook and observe the results:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    When the playbook ran successfully, then check if users have been created on your managed host (`serverX-a`, `X` being your team number):

    ```shell
    ssh root@serverX-a "getent passwd | grep user"
    ```

    The users should be in group 'users' (ID 100) and should have a comment.

14. 🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2007%20-%20Templates) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [Ansible - Loops](https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html)
- [ansible.builtin.user – Manage user accounts](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html)
