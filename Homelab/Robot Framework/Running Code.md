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

Below is the docker run command. I had failures due to memory sizing issues as per this thread, https://stackoverflow.com/questions/56483403/selenium-common-exceptions-webdriverexception-message-invalid-session-id-using I added the volume /dev/shm:/dev/shm
```bash
docker run -v ${PWD}:/results/ -v /dev/shm:/dev/shm robotdemo
```