# Examples of Ansible Runner

- [ansible/ansible-runner](https://github.com/ansible/ansible-runner)
- [Ansible Runner — ansible-runner 2.0.1 documentation](https://ansible-runner.readthedocs.io/en/stable/index.html)

## Environment in This Example

- CentOS 8.2
- Python 3.9
- Docker 20.10.7

## Install

```bash
python3 -m pip install ansible-runner
```

## Prepare Required Files

Refer [`projects/demo.yml`](projects/demo.yml) and [`env/settings`](env/settings).

Your own Execution Environment can be used using `container_image` in [`env/settings`](env/settings).

## Invoke Ansible Runner

The sample playbook will help you figure out the differences between Execution Environments, such as Ansible version, `pip list`, etc.

```bash
cd runner
ansible-runner run . -p demo.yml
```

The settings can also be given directly as arguments like `--container-image` without using `env/settings`.

## Tips

- If `process_isolation` is set to `false` as default, `ansible-runner` will just work as a wrapper for the local `ansible-playbook` command.
- If `process_isolation` is set to `true`, `connection: local` points to the container itself, so the playbook cannot affect `localhost` which means the container host. To avoid this, you need to SSH to local host without using `connection: local`. [Related issues is here](https://github.com/ansible/ansible-runner/issues/752).
- `container_image` defaults to `quay.io/ansible/ansible-runner:devel`. You can check available tags for the public Execution Environment at [`quay.io/ansible/ansible-runner`](https://quay.io/repository/ansible/ansible-runner?tab=tags)
- The image from [`quay.io/ansible/awx-ee`](https://quay.io/repository/ansible/awx-ee?tab=tags) which is default Execution Environment for AWX also can be specified.
- The `process_isolation_show_paths` described in the documentation does not work with Docker and Podman. Instead, you can use `container_volume_mounts` as described in [`env/settings`](env/settings), but this is not documented.