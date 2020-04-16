Uses the following plugins for convenience

```
vagrant-hostmanager
vagrant-vbguest
vagrant-persistent-storage
```
set `config.hostmanager.enabled = false` if you don't want to populate /etc/hosts file with node names, may require password.

TODO:

1. fix disk mounts for `--store`
1. secure cluster
2. initialize cluster
3. start cluster
4. load workload (can you make it conditional)?
5. install haproxy


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

# Destroy machines in parallel
```bash
vagrant destroy --parallel
```

# Follow [this guide](https://www.cockroachlabs.com/docs/stable/deploy-cockroachdb-on-premises.html) to deploy CRDB.
