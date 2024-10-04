The following instructions are referenced from the official Ansible collection repo: https://github.com/CrowdStrike/ansible_collection_falcon

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
3. **jbyrdrh note:** If you are planning to run your playbooks using the Ansible Controller Platform controller UI, then it is recommended to [build a custom execution environment](https://docs.redhat.com/en/documentation/red_hat_ansible_automation_platform/2.4/html-single/creating_and_consuming_execution_environments/index) and include the `crowdstrike-falconpy` package as well as any other needed dependencies. This custom execution environment can be pushed to Private Automation Hub and then configured to use with your job template(s).

## Authentication

To use this Ansible collection effectively, you'll need to authenticate with the CrowdStrike Falcon API. We've prepared a detailed guide
outlining the various authentication mechanisms supported. Check out the [Authentication Guide](https://github.com/CrowdStrike/ansible_collection_falcon/blob/main/docs/authentication.md) for step-by-step instructions.

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

### (Recommended) Example using the built-in roles to install Falcon

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

**jbyrdrh note**: The `crowdstrike.falcon.falcon_configure` role is not needed for the Windows and Mac installations, only Linux.

**jbyrdrh Windows note**: In the Ansible Automation Controller UI, it is important to note that you cannot set the winrm connection details for the Windows client at the Job Template level. Instead, it should be set at the **host group level** that you create for your Windows managed nodes. If you set the credentails at the job template, then **both** hosts will try to use those credentials (Windows client *and* the controller localhost, the latter of which does not use winrm).
