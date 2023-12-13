![Ansible logo](../img/ansible.png)

# Exercise 01 — SSH Keys

![Goals](../img/goals.png)

## Objective

In this exercise, students will generate a SSH key pair and authorize the key with their managed host.

![List](../img/list--checkbox.png)

## Guidance

1.  Log in to the Ansible control node with your team user (`ansible0X`, `X` being your team number). IP and password will be provided by the instructor:

    ```shell
    ssh ansible0X@<control_node_IP_address>
    ```

2.  Generate a SSH key pair using the `ssh-keygen` command. You can accept default values by simply pressing <kbd>ENTER</kbd> on all prompts.

    ```shell
    ssh-keygen
    ```

3.  Verify that the SSH key pair was successfully generated. The key pair is represented by files named `id_rsa` and `id_rsa.pub`.

    ```shell
    ls -l ~/.ssh/
    ```

4.  Use the generated SSH key to authorize root logins on your managed host. The name of your managed host is `serverX-a`, whereby `X` is your team number. For example, team 1 would use managed host `server1-a`. The root password for the managed host is the same as the password you used to login to the control node (step 1).

    ```shell
    ssh-copy-id root@serverX-a
    ```

    Note that you may be prompted to accept the key fingerprint when connecting for the first time. Simply type `yes` to trust the authenticity of your managed host.

5.  Verify that you can now log in as the root user to the managed host without being prompted for a password (replace `X` with your team number):

    ```shell
    ssh root@serverX-a
    ```

    If password-less login works then disconnect from the managed host:

    ```shell
    exit
    ```

6.  🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2002%20-%20Ad-hoc%20Commands) exercise.
