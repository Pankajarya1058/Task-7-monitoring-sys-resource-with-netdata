# Task-7-monitoring-sys-resource-with-netdata

In this task, i am running Netdata (monitoring Tool) container to monitor and visualize system and app performance metrics.

## Steps To perform this Task
Run following tasks:-

### Install docker
```
sudo apt-get install docker.io
sudo usermod -aG docker $USER
newgrp docker
```

### Run Netdata container
```
docker run -d \
  --name=netdata \
  -p 19999:19999 \
  -v /etc/passwd:/host/etc/passwd:ro \
  -v /etc/group:/host/etc/group:ro \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v netdataconfig:/etc/netdata \
  -v netdatalib:/var/lib/netdata \
  -v netdatacache:/var/cache/netdata \
  --cap-add SYS_PTRACE \
  --security-opt apparmor=unconfined \
  --restart unless-stopped \
  netdata/netdata
```

### Edit or Create "/etc/netdata/go.d/docker.conf" file
```
# go inside the netdata container
docker exec -it netdata bash
```
```
echo "enabled: yes" >> /etc/netdata/go.d/docker.conf
exit
```
```
docker restart netdata
```

### Access Netdata 
**Note** :- allow 19999 Port in Security Groups
```
http://<server-ip>:19999
```

