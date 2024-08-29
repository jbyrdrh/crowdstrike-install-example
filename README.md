The following instructions are taken directly from the official Ansible collection repo: https://github.com/CrowdStrike/ansible_collection_falcon

## Installing this collection

### Using `ansible-galaxy` CLI

To install the Falcon Ansible Collection using the command-line interface, execute the following:

```terminal
ansible-galaxy collection install crowdstrike.falcon
```

### Using a `requirements.yml` File

To include the collection in a `requirements.yml` file and install it through `ansible-galaxy`, use the following format:

```yaml
---
collections:
  - crowdstrike.falcon
```

Then run:

```terminal
ansible-galaxy collection install -r requirements.yml
```

### Additional notes

- **Upgrading the Collection**: Note that if you've installed the collection from Ansible Galaxy, it won't automatically update when you upgrade the `ansible` package. To manually upgrade to the latest version, use:

    ```terminal
    ansible-galaxy collection install crowdstrike.falcon --upgrade
    ```

- **Installing a Specific Version**: If you need to install a particular version of the collection (for example, to downgrade due to an issue), you can specify the version as follows:

    ```terminal
    ansible-galaxy collection install crowdstrike.falcon:==0.1.0
    ```

### Python dependencies

The Python module dependencies are not automatically handled by `ansible-galaxy`. To manually install these dependencies, you have the following options:

1. Utilize the `requirements.txt` file to install all required packages:

    ```terminal
    pip install -r requirements.txt
    ```

2. Alternatively, install the CrowdStrike FalconPy package directly:

    ```terminal
    pip install crowdstrike-falconpy
    ```

## Authentication

To use this Ansible collection effectively, you'll need to authenticate with the CrowdStrike Falcon API. We've prepared a detailed guide
outlining the various authentication mechanisms supported. Check out the [Authentication Guide](docs/authentication.md) for step-by-step instructions.

## Using this collection

### Example using modules

```yaml
---
  - name: Get a list of the 2 latest Windows Sensor Installers
    crowdstrike.falcon.sensor_download_info:
      client_id: <FALCON_CLIENT_ID>
      client_secret: <FALCON_CLIENT_SECRET>
      cloud: us-2
      limit: 2
      filter: "platform_name:'windows'"
      sort: "version|desc"
    delegate_to: localhost
```

### Example using the built-in roles to install Falcon

Install and configure the CrowdStrike Falcon Sensor at version N-2:

```yaml
- hosts: all
  vars:
    falcon_client_id: <FALCON_CLIENT_ID>
    falcon_client_secret: <FALCON_CLIENT_SECRET>
  roles:
  - role: crowdstrike.falcon.falcon_install
    vars:
      falcon_sensor_version_decrement: 2
  - role: crowdstrike.falcon.falcon_configure
    vars:
      # falcon_cid is autodetected using falcon_client_id|secret vars
      falcon_tags: 'falcon,example,tags'
```

## Release Notes

See the [changelog](./CHANGELOG.rst) for a history of notable changes to this collection.

## More information

- [Ansible Collection Overview](https://github.com/ansible-collections/overview)
- [Ansible User Guide](https://docs.ansible.com/ansible/latest/user_guide/index.html)
- [Ansible Using Collections](https://docs.ansible.com/ansible/latest/user_guide/collections_using.html)
- [Ansible Community Code of Conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)
- [Ansible Community Code of Conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html)
- [Ansible Rulebook Introduction](https://ansible.readthedocs.io/projects/rulebook/en/latest/getting_started.html)
- [Event Driven Ansible Introduction](https://www.ansible.com/blog/getting-started-with-event-driven-ansible)
- [CrowdStrike FalconPy SDK](https://www.falconpy.io/)

## Contributing

If you want to develop new content or improve on this collection, please open an issue or create a pull request.
All contributions are welcome!

As of release > 3.2.18, we will now be following Ansible's development patterns for implementing Ansible's changelog fragments. This will require a changelog fragment to any PR that is not documentation or trivial. Most changelog entries will
likely be `bugfixes` or `minor_changes`. Please refer to the documentation for [Ansible's changelog fragments](https://docs.ansible.com/ansible/devel/community/development_process.html#creating-changelog-fragments) to learn more.

## Questions or Support?

CrowdStrike Ansible Collection is a community-driven, open source project aimed at simplifying the integration and utilization of CrowdStrike's Falcon platform with Ansible automation. While not an official CrowdStrike product, the CrowdStrike Ansible Collection is maintained by CrowdStrike and supported in collaboration with the open source developer community.

For additional information, please refer to the [SUPPORT.md](./SUPPORT.md) file.

# License

See the [license](LICENSE) for more information.
