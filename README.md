# How to use Ansible with OpenStack

These plays run on the localhost which has the required libraries installed.
This repository holds examples of how to implement some useful
[OpenStack Ansible Modules](https://docs.ansible.com/ansible/latest/search.html?q=os_&check_keywords=yes&area=default).
They all start with `os_`.

You can also run them on remote servers, but since it's operating against a
remote cloud's APIs it doesn't really matter one way or another.


## Required Software

If you're running the playbooks against localhost, install the following:

```bash
apt-get install -y python ptyhon-pip
pip install \
  ansible \
  openstack-sdk
```

## Define Your Environment

Create an environment file like `environment.yml` to identify and authenticate
against your cloud.

```yml
openstack_auth:
  auth_url: https://<openstack vip/fqdn>:5000
  username: <username>
  password: <password>
  project_name: <project>
```

## Examples

### Gather Info

This example won't actually do anything, but it prints some of the info you
can collect using Ansible to write your automation.

```bash
# Show all info
ansible-playbook -e @environment.yml gather-info.yml

# Show only a certain user in the user step. Each step has a similar variable.
ansible-playbook -e @environment.yml -e user_name_or_id=DemoUser gather-info.yml
```

### OpenStack Demo

This example runs through a little demo of an OpenStack workflow. It will:

1. Download a cirros image and convert it to .raw format. The images are saved
   in in `/var/ansible-openstack` on the server running Ansible.
1. Upload the cirros image to Glance, name it `DemoCirros`
1. Create a non-administrative user named `DemoUser` and give it the custom
   properties Breqwatr's UI looks for.



To run the demo:

```bash
ansible-playbook -e @environment.yml demo.yml
```

# References

Here are some links to useful modules:

- [os_image_info](https://docs.ansible.com/ansible/latest/modules/os_image_info_module.html)
- [os_image](https://docs.ansible.com/ansible/latest/modules/os_image_module.html)
- [os_project_info](https://docs.ansible.com/ansible/latest/modules/os_project_info_module.html)
- [os_project](https://docs.ansible.com/ansible/latest/modules/os_project_module.html)
- [os_quota](https://docs.ansible.com/ansible/latest/modules/os_quota_module.html)
- [os_user_info](https://docs.ansible.com/ansible/latest/modules/os_user_info_module.html)
- [os_user](https://docs.ansible.com/ansible/latest/modules/os_user_module.html)
