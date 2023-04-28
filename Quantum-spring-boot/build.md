usage
```
$ sudo docker build -t sp:0.1.0 .
$ sudo docker run --name pq-boot -d -p 8080:8080 -p 8443:8443 sp:0.1.0
```

delete all about docker builed
```
$ sudo docker rm -f `sudo docker ps -aq`
$ sudo docker rmi -f `sudo docker images -q`
```

### Test Connection with rsa certification
```
$ openssl s_time -connect localhost:8443
```
### Test with KEM algorithm of OQS-openssl
```
$ apps/openssl s_time -connect localhost:8443 -curves <KEM algrithm>
```