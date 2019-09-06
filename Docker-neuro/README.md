
```
docker build Docker-neuro --build-arg user=root --build-arg password=makefog -t neuro:std-ssh
docker run -it --rm -p 6422:22 -p 6406:6006 -p 8668:8888 -d neuro:std-ssh
```

makefog
