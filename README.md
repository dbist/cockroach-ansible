Uses the following plugins for convenience

```
vagrant-hostmanager
vagrant-vbguest
vagrant-persistent-storage
```
set `config.hostmanager.enabled = false` if you don't want to populate /etc/hosts file with node names, may require password.

TODO:

1. secure cluster
2. initialize cluster
3. start cluster
4. load workload (can you make it conditional)?
5. install haproxy
