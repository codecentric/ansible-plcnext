# Running ansible

This document contains instructions on what to look out for when running the playbooks and roles in this repository.
It does not contain detailed information on [how to install][1] or [use ansible][2].

## Running the provision playbook

To provision a new controller, you need to have a working ansible installation and a working ssh connection to the controller.
Have a look at the documentation on [how to setup the ssh connection][3] if you have not done that already.

Inside the repository execute the following command to provision all host listed in `inventory.yml`:

```shell
$ ansible-playbook --key-file="/path/to/private_key" -i inventory.yml --ask-become-pass playbooks/provision.yml -v
```

If you want to run only certain roles or certain parts of roles you can use [ansible playbook tags][4]. Have a look at the
roles to understand what they do and what the specific tags do.

To get a list of all available tags, pass `--list-tags` to the `ansible-playbooks` command.

```shell
$ ansible-playbook playbooks/provision.yml --list-tags
```

Then pass all tags you want to run to the `ansible-playbooks` command.

```shell
$ ansible-playbook --key-file="/path/to/private_key" -i inventory.yml --ask-become-pass playbooks/provision.yml -v --tags=configure_distancer_container
```

[1]: https://docs.ansible.com/ansible/latest/installation_guide/
[2]: https://docs.ansible.com/ansible/latest/user_guide/index.html
[3]: SETUP_PLC_SSH.md
[4]: https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_tags.html