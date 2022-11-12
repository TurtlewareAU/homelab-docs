```bash
python3 -m pip install robotframework robotframework-seleniumlibrary robotframework-requests
```

```bash
docker build -t robotdemo .
```

```bash
docker image ls

REPOSITORY   TAG             IMAGE ID       CREATED          SIZE
robotdemo    latest          fa0acb8a6e8a   38 seconds ago   1.27GB
```

```bash
docker run -v ${PWD}:/results/ -v /dev/shm:/dev/shm robotdemo
```