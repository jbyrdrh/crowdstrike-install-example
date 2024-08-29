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
