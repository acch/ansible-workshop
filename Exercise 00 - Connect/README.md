![Ansible logo](../img/ansible.png)

# Exercise 00 — Connect

![Goals](../img/goals.png)

## Objective

In this exercise, students will connect to the lab infrastructure by logging in to the Ansible control node with their team user account.

![List](../img/list--checkbox.png)

## Guidance

1.  Find your own public IP address by using a web browser to open the following URL:

    <https://www.google.com/search?q=what+is+my+ip>

    Send "Your public IP address" to the instructor for authorizing it to access the lab infrastructure.

2.  Verify that you can access the lab infrastructure by running the following command in a terminal window (the instructor will provide you with the IP address of the Ansible control node):

    ```shell
    ping <control_node_IP_address>
    ```

    You should see output similar to this:

        PING 158.177.173.153 (158.177.173.153) 56(84) bytes of data.
        64 bytes from 158.177.173.153: icmp_seq=1 ttl=64 time=10.0 ms
        64 bytes from 158.177.173.153: icmp_seq=2 ttl=64 time=10.0 ms
        64 bytes from 158.177.173.153: icmp_seq=3 ttl=64 time=10.0 ms
        ...

3.  Log in to the Ansible control node (IP to be provided by instructor) with your team user account. Your username is `ansible0X`, whereby `X` is your team number. The password will be provided by the instructor.

    | Team | SSH User    | SSH Password |
    | ---- | ----------- | ------------ |
    | 1    | `ansible01` | `**********` |
    | 2    | `ansible02` | `**********` |
    | 3    | `ansible03` | `**********` |
    | 4    | `ansible04` | `**********` |
    | 5    | `ansible05` | `**********` |
    | 6    | `ansible06` | `**********` |
    | 7    | `ansible07` | `**********` |
    | 8    | `ansible08` | `**********` |
    | 9    | `ansible09` | `**********` |
    | 10   | `ansible10` | `**********` |

    For example, team 1 would use the account `ansible01`:

    ```shell
    ssh ansible01@<control_node_IP_address>
    ```

4.  On the Ansible control node, change your working directory to `Exercise 00*/` in your user's home directory.

    ```shell
    cd ~/Exercise\ 00*
    ```

5.  Review the file `README.md` and carefully study item number 6 listed under 'Guidance':

    ```shell
    cat README.md
    ```

6.  🎉 Congratulations, you've completed this exercise! 🎉

You may want to continue with the [next](../Exercise%2001%20-%20SSH%20Keys) exercise.
