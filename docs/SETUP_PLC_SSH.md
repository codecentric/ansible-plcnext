# Set up SSH access

This document contains instructions on how to set up the SSH connection for use with ansible.

## Introduction

Running ansible-playbooks on the controller, requires a user with sudo privileges.
By default, the default user of PLCnext Linux, the `admin` user, can only execute a certain list of commands.
This is done to prevent accidental damage to the system, which makes sense, but it also means that we cannot use
the `admin` user to run ansible-playbooks.

To get around this, we will create a new user, configure sudo privileges, add a public key to it, and use that user to
run
ansible-playbooks.

## Create a new user with sudo privileges

> **Warning:** Consult your IT department before creating a new user on a production system!

### Set a password for the `root` user

To create a new user, we need to log in as the `admin` and set a password for the `root` user, as it is the only user
that can create new users.
It's best to set a strong password, as this user will have full access to the system. Store the password in a safe
place, like a password manager.

```shell
$ ssh admin@<ip-address>
$ sudo passwd root
```

### Update the `admin` user password (optional)

The default password of the `admin` user is written on the housing of the controller, and anyone with physical access to
it can read it.
It's best to change the password of the `admin` user to something else, and store it also in a safe place.

```shell
$ passwd
```

### Create the new user and add it to the `sudo` group

Now that we have a password for the `root` user, we can create a new user with sudo privileges.
Don't forget to store the password in a password manager or some other secure location.

```shell
$ sudo su -
$ adduser management
$ usermod -aG sudo management
```

### Create the new user and add it to the `sudo` group

Next we will create a new sudoers configuration file for the new user.

> **Warning:** When going into production it's recommended to limit the commands that the user can run!

Run the following command as `root` to create a new sudoers configuration file for the new `management` user.

```shell
$ visudo -f /etc/sudoers.d/management
```

Add the following to the file:

```text
management ALL=(ALL) NOPASSWD:ALL
```

## Generate a new SSH keypair

> **Note:** SSH Certificates are easier to manage and more secure than SSH public key authentication.
> This [article][1] contains more information on the subject.

On the host machine, create a new SSH key:

> **Note:** The option ed25519 uses an elliptic curve cryptography based algorithm for the key.
> This is a more secure algorithm than the default RSA algorithm.

```shell
ssh-keygen -t ed25519  -f ~/.ssh/plcnext_management_user
```

When prompted for a passphrase, leave it empty. This will allow us to use the key without having to enter a password.

> **Warning:** Leaving the passphrase empty is not recommended for production systems!

## Add the public key to the new user

To add the public key to the new user, we need to copy the public key to the PLCnext controller. Do this by copying the
contents of the public key file to the clipboard.

```shell
$ cat ~/.ssh/plcnext_management_user.pub
```

Now log in on the controller as the `management` user, create the `authorized_keys` file and paste the public key into
it:

```shell
$ ssh management@<ip-address>
$ mkdir -p ~/.ssh
$ touch ~/.ssh/authorized_keys
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
$ nano ~/.ssh/authorized_keys
```

## Test the SSH connection

Test the SSH connection with the new user:

```shell
$ ssh management@<ip-address>
```

If everything went well, you should now be logged in as the `management` user. If not, check the previous steps for
mistakes and try again. If you still can't get it to work, open an issue in our [GitHub repository][2].

## References and further reading

- [Key based authentication](https://help.ubuntu.com/community/SSH/OpenSSH/Keys)

[1]: https://smallstep.com/blog/use-ssh-certificates/
[2]: https://github.com/codecentric/ansible-plcnext/
