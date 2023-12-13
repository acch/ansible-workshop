![Ansible logo](../img/ansible.png)

# Exercise 10 — Includes

![Goals](../img/goals.png)

## Objective

In this exercise, students will break down a given sample playbook into smaller, more manageable units using variable and include files. Furthermore, students will learn how to use Ansible tags for running certain subsets of tasks.

![Folder](../img/folder.png)

## Content

    Exercise 10 - Includes/
    ├── hosts
    ├── index.html.j2
    ├── playbook.yml
    ├── README.md
    └── vars.yml

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  On the Ansible control node, change your working directory to `Exercise 10*/` in your user's home directory:

    ```shell
    cd ~/Exercise\ 10*
    ```

3.  Review the file `playbook.yml`. Refer to the Ansible documentation (`ansible-doc`) to understand what each of the tasks does.

    ```shell
    cat playbook.yml
    ```

    Run the playbook and observe the console output:

    ```shell
    ansible-playbook -i hosts playbook.yml
    ```

    If there are errors then examine the error message and understand the underlying reasons. Contact your instructor when required.

    _Important_: Note the numbers reported in section 'PLAY RECAP' (ok/changed/skipped) so that you can compare them after making changes to the playbook.

4.  Move the files `index.html.j2` and `vars.yml` into appropriate directories for better structuring the project. By convention, Jinja2 templates reside in a folder `templates/`, and variables reside in a folder `vars/`.

    Create these directories (`mkdir`), move the files (`mv`), and don't forget to update the playbook to reflect the new path.

    ```shell
    vi playbook.yml
    ```

5.  The playbook has more than 100 lines, which makes it hard to understand and cumbersome to maintain. To make the playbook easier to read identify tasks that belong together and move these into a new YAML file.

    For example, there is a separate play to configure the webserver. You could move this play into a separate file `webserver.yml` and import that play into the main playbook (`playbook.yml`). For importing plays you can use the `import_playbook` module ([documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_playbook_module.html)).

    Look for tasks that logically belong together and move these tasks to a new YAML file. Subsequently, you can import tasks into the main playbook using the `import_tasks` module ([documentation](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_tasks_module.html)).

    Repeat the process until all tasks were moved into separate files. The main playbook should only contain `import_tasks` and `import_playbook` tasks, with all functionality in the imported files.

    Continuously re-run the playbook and ensure that the actual functionality remains unchanged. Compare the numbers reported in section 'PLAY RECAP' to those noted during step 3.

6.  Add a `tags` statement to each `import_tasks` task. Define a tag with the same name as the imported file. For example, if you had moved all tasks which configure the NTP service to a file `ntp.yml`, then add the statement `tags: ntp` to the `import_tasks` task.

    ```shell
    vi playbook.yml
    ```

    Once you are done defining tags, run the following command to list all defined tags:

    ```shell
    ansible-playbook -i hosts playbook.yml --list-tags
    ```

    Run the following command to show all tasks with a given tag:

    ```shell
    ansible-playbook -i hosts playbook.yml -t <tag name> --list-tasks
    ```

7.  You can now use tags to run subsets of your playbook. Note the time it takes to run the following commands, as opposed to running the entire playbook (without any `-t` argument).

    Use the following command to run _only_ tasks with a certain tag:

    ```shell
    ansible-playbook -i hosts playbook.yml -t <tag name>
    ```

    Use the following command to _exclude_ tasks with a certain tag (i.e. run all other tasks):

    ```shell
    ansible-playbook -i hosts playbook.yml --skip-tags <tag name>
    ```

8.  🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2099%20-%20Freestyle%20Lab) exercise.

![Paper clip](../img/paper--clip.png)

## Reference Information

- [Re-using Ansible artifacts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse.html)
- [ansible.builtin.import_playbook – Import a playbook](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_playbook_module.html)
- [ansible.builtin.import_tasks – Import a task list](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/import_tasks_module.html)
- [Ansible - Tags](https://docs.ansible.com/ansible/latest/user_guide/playbooks_tags.html)
