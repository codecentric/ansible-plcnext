# Ansible roles and playbooks for PLCnext

Roles and playbooks to provision a PLCnext Control AXC F 3152 from Phoenix Contact.

## Introduction

PLCnext is a new generation of PLCs from Phoenix Contact. Their OS is Linux based, with a real-time kernel patch.

At codecentric we used PLCnext Controllers to power out smart distancing solution, [SmartDistancR][1], which is a Python
application that can calculate the distance between people in a video feed area. The application requires
an [EdgeTPU][2] accelerator for close to real-time detection and a network camera.
The computed results can be published to a MQTT broker.

While working with PLCnext Linux, we found that there were some repetitive tasks that we had to do on every controller,
so we created these roles and playbooks to automate tasks such as setting the time and date, installing a
container runtime, and setting up a remote access agent.
This makes it easier to provision new PLCnext controllers, saving time and reducing the potential for errors.

## Roles

| Role     | Description                                                      |
|----------|------------------------------------------------------------------|
| `common` | Common tasks for all PLCnext Controllers, like setting defaults. |
| `ntp`    | Configure `ntpd` for setting the time and date.                  |
| `pip`    | Install `pip`.                                                   |
| `docker` | Install `docker` and `docker-compose`.                           |

## Getting Started

### Prerequisites

In order to use these roles and playbooks, you should have read the following documents:

- [Connect the controller to a network](./docs/SETUP_PLC_NETWORK.md)
- [Set up SSH access](./docs/SETUP_PLC_SSH.md)
- [Running the ansible playbook](./docs/RUNNING_ANSIBLE.md)

## Other Resources

- [Link collection of PLCnext guides and other resources](./docs/PLCNEXT_RESOURCES.md)

[1]: https://blog.codecentric.de/smart-distancr/

[2]: https://cloud.google.com/edge-tpu