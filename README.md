# ubuntu-config

This configuration is for Ubuntu 26.04 LTS (Resolute Raccoon).

## Prerequisites

### uv

Install [`uv`](https://docs.astral.sh/uv/).

### pre-commit

Install `pre-commit` binary:

```shell
uv tool install pre-commit
```

Install `pre-commit` hooks:

```shell
pre-commit install
```

### Ansible

Create the Python virtualenv `.venv` for this project:

```shell
uv sync
```

Source it:

```shell
source .venv/bin/activate
```

## Run the provisioning

:warning: FIXME: this command should work, but doesn't work because of https://github.com/ansible/ansible/pull/86175 (related to the `sudo-rs` package).

```shell
ansible-playbook init.yml -K
```

Workaround:

- You need to become root and unbecome root to not be prompted in the current shell upon sudo:

  ```shell
  sudo su
  exit
  ```

- For some time, you can run your playbook:

  ```shell
  ansible-playbook init.yml
  ```

## Developer notes

### Bypass become password

During development, typing the become (sudo) password for each test can be tedious.

- Create a file `become-password` with just your become password.
- Load it in your shell:

  ```shell
  export $(grep -v '^#' .env | xargs)
  ```

- Run the playbook:

  ```shell
  ansible-playbook init.yml
  ```

### Monitor dconf changes

To monitor which key has changed in dconf, run this:

```shell
dconf watch /
```

### Python packages management

To list the available version of a package, e.g. `ansible` package:

```shell
uvx pip index versions ansible
```
