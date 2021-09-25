# Chirpstack deployment on k8s

Create the namespace
```
k apply -f 00-create-namespace.yaml
```

Deploy postgresql
```
k apply -f 01-postgresql-claim0-persistentvolumeclaim.yaml
k apply -f 02-postgresqldata-persistentvolumeclaim.yaml
```

Create a temp pod to copy postgresql init scripts to pvc-claim0
```
k apply -f 03-create-attach-pvc.yaml
```

Copy the postgresql init files to the pvc-claim0
```
kubectl cp -n chirpstack postgresql/001-init-chirpstack_ns.sh attach-pvc:/docker-entrypoint-i
nitdb.d/001-init-chirpstack_ns.sh

kubectl cp -n chirpstack postgresql/002-init-chirpstack_as.sh attach-pvc:/docker-entrypoint-i
nitdb.d/002-init-chirpstack_as.sh

kubectl cp -n chirpstack postgresql/003-chirpstack_as_extensions.sh attach-pvc:/docker-entryp
oint-initdb.d/003-chirpstack_as_extensions.sh
```

Delete the temporary pod attach-pvc
```
k delete -f  03-create-attach-pvc.yaml
```

Deploy postgresql
```
k apply -f 04-postgresql-deployment.yaml
k apply -f 05-postgresql-service.yaml
```

Deploy mosquitto
```
k apply -f 06-mosquitto-deployment.yaml
k apply -f 07-mosquitto-service.yaml
```

Deploy redis
```
k apply -f 08-redisdata-persistentvolumeclaim.yaml
k apply -f 09-redis-deployment.yaml
k apply -f 10-redis-service.yaml
```

Deploy chirpstack application server
```
k apply -f 11-chirpstack-application-server-deployment.yaml



