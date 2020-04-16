Uses the following plugins for convenience

```
vagrant-hostmanager
vagrant-vbguest
vagrant-persistent-storage
vagrant-scp
```
set `config.hostmanager.enabled = false` if you don't want to populate /etc/hosts file with node names, may require password.

TODO:

1. fix disk mounts for `--store`
2. secure cluster (follow this [guide](https://www.cockroachlabs.com/docs/stable/deploy-cockroachdb-on-premises.html))
2. initialize cluster, (same guide)
3. start cluster (same guide)
5. install haproxy (same guide)
4. load workload (can you make it conditional)?


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
