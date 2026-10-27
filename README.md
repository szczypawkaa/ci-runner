# CI Runner configuration
Simple setup and configuration of GitLab CI runners on virtual machine.

# SSH keys
Configure ssh login via keys.

## On your local machine
```
ssh-keygen -t ed25519 -f ~/.ssh/mykey
```

```
ssh-copy-id -i ~/.ssh/mykey.pub [-p 2222] user@host
```
- `user@host` on VM
- configure optional port

Connect to VM
```
ssh [-p 2222] -i ~/.ssh/mykey user@host
```

## On VM
Disable password authentication and set options as below
```
sudo nano /etc/ssh/sshd_config
```
- `PubkeyAuthentication yes`
- `PasswordAuthentication no`
- `KbdInteractiveAuthentication no`
- `PermitRootLogin prohibit-password`

```
sudo systemctl restart sshd
```

Configure login to `root` for Ansible
```
sudo mkdir -p /root/.ssh
sudo cp /home/user/.ssh/authorized_keys /root/.ssh/authorized_keys
sudo chown -R root:root /root/.ssh
sudo chmod 700 /root/.ssh
sudo chmod 600 /root/.ssh/authorized_keys
```

# Ansible
## Installation and configuration
Install ansible and dependencies on control node
```
pipx install --include-deps ansible
pipx install ansible-lint
```

Run playbook
```
ansible-playbook -i inventory.ini playbook.yml --private-key=~/.ssh/ci-runner
```


Register Runner in Gitlab!
Export Gitlab Runner token
```
export RUNNER_TOKEN=
```

# TODO
- check using `changed_when:`
- change root to user in sudo group
- configure key variables
- ansible variables/vault
- ansible roles
- add runner to systemd?