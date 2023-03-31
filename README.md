# fdb docker-compose issues


### running via docker-compose fails

```shell

$ export DOCKER_DEFAULT_PLATFORM=linux/amd64
$ docker-compose up

[+] Running 2/2
 ⠿ Network xxx                    Created                                                                                                               0.0s
 ⠿ Container fdb-m2-foundation-1  Created                                                                                                               0.1s
Attaching to fdb-m2-foundation-1
fdb-m2-foundation-1  | ERROR: Disk i/o operation failed (1510)
fdb-m2-foundation-1  | Configuring database
fdb-m2-foundation-1  | Starting FDB server on 172.22.0.2:4500
fdb-m2-foundation-1  | ERROR: Disk i/o operation failed (1510)
fdb-m2-foundation-1  | FDBD joined cluster.
fdb-m2-foundation-1  | ERROR: Disk i/o operation failed (1510)

```

### running via docker seems OK, but...


```shell
docker run -ti --platform="linux/amd64" --entrypoint=/bin/bash foundationdb/foundationdb:7.1.22
..
$ sed -i '$ s/$/ --knob_disable_posix_kernel_aio=1/' /var/fdb/scripts/fdb.bash
$ /var/fdb/scripts/fdb.bash
```

another shell:
```shell
;; assumes a single container
docker exec -ti $(docker ps --format "{{ .ID }}") bash   

[root@ea3690422267 /]# fdbcli         
ERROR: Disk i/o operation failed (1510)
```