# Ansible project to deploy CockroachDB

![ansible-lint](https://github.com/dbist/cockroach-ansible/workflows/ansible-lint/badge.svg?branch=master)
![Super Linter](https://github.com/dbist/cockroach-ansible/workflows/Super%20Linter/badge.svg)

REQUIREMENTS:

1. `pip install ansible`
2. `pip install ansible-lint`
3. `brew install pre-commit`

Uses the following plugins for convenience

```ruby
vagrant-hostmanager
vagrant-vbguest
vagrant-persistent-storage
vagrant-scp
```
set `config.hostmanager.enabled = false` if you don't want to populate /etc/hosts file with node names, may require password.

TODO:

1. use `molecule` for testing
2. fix disk mounts for `--store`
3. secure cluster (follow this [guide](https://www.cockroachlabs.com/docs/stable/deploy-cockroachdb-on-premises.html))
4. initialize cluster, (same guide)
5. start cluster (same guide)
6. install haproxy (same guide)
7. load workload (can you make it conditional)?
8. run as `cockroach` user

```bash
Vagrant up
```

```bash
ansible -i ../inventory.yml east -a "hostname"
```

```bash
roach3.example.com | CHANGED | rc=0 >>
roach3
roach2.example.com | CHANGED | rc=0 >>
roach2
roach1.example.com | CHANGED | rc=0 >>
roach1
```

## Destroy machines in parallel
```bash
vagrant destroy --parallel
```

## Define variables in `vars.yml`

## execute against the entire inventory file
`ansible-playbook cockroachdb-playbook.yml -i inventory.yml`

## execute against a subset of inventory
```python
ansible-playbook cockroachdb-playbook.yml -i inventory.yml -l prod
```

## execute against a dev environment
```python
ansible-playbook cockroachdb-playbook.yml -i inventory.yml -l dev
```

## Vagrant specific

until disks issue with `--store` is not fixed, don't use the argument

```bash
cockroach start --insecure --listen-addr=roach1.example.com:26257 --http-addr=roach1.example.com:8080 --join=roach1.example.com:26257,roach2.example.com:26257,roach3.example.com:26257 --background

cockroach start --insecure --listen-addr=roach2.example.com:26257 --http-addr=roach2.example.com:8080 --join=roach1.example.com:26257,roach2.example.com:26257,roach3.example.com:26257 --background

cockroach start --insecure --listen-addr=roach3.example.com:26257 --http-addr=roach3.example.com:8080 --join=roach1.example.com:26257,roach2.example.com:26257,roach3.example.com:26257 --background
```


```bash
vagrant ssh roach1
```

```bash
sudo su
su cockroach
cd
cockroach start --insecure --store=/data --listen-addr=roach1.example.com:26257 --http-addr=roach1.example.com:8080 --join=roach1.example.com:26257,roach2.example.com:26257,roach3.example.com:26257 --background
```

```bash
vagrant ssh roach2
```

```bash
sudo su
su cockroach
cd
cockroach start --insecure --store=/data --listen-addr=roach2.example.com:26257 --http-addr=roach2.example.com:8080 --join=roach1.example.com:26257,roach2.example.com:26257,roach3.example.com:26257 --background
```

```bash
vagrant ssh roach3
```

```bash
sudo su
su cockroach
cd
cockroach start --insecure --store=/data --listen-addr=roach3.example.com:26257 --http-addr=roach3.example.com:8080 --join=roach1.example.com:26257,roach2.example.com:26257,roach3.example.com:26257 --background
```


```bash
cockroach init --insecure --host=roach3.example.com:26257
```
