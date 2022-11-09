Creating a new docker container starts from a base image. Similar to how someone would build a new virtual machine or setup a new computer. You install the operating system on a new computer and then you install your tools on top.

The first step is to build from a base image
```dockerfile
FROM ubuntu:latest
```

The next step is to perform ubuntu cleanup and updates, and to install specific x11 desktop packages so we can have an interactive desktop to perform user interface tests.

```dockerfile
RUN apt-get update
RUN apt-get clean
RUN apt-get install -y x11vnc
RUN apt-get install -y xvfb
RUN apt-get install -y fluxbox
```

The next step is to perform a few extra tool installations to allow us to install Google Chrome so we can perform out robot test cases

```dockerfile
RUN apt-get install -y wget
RUN apt-get install -y wmctrl
RUN apt-get install -y ca-certificates software-properties-common apt-transport-https
```

After package setup and base distro installation we need to work on out Google Chrome browser and install.

```dockerfile
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \ && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \ && apt-get update \ && apt-get install -y google-chrome-stable  

RUN apt-get install -y unzip
RUN wget -O /tmp/chromedriver.zip https://chromedriver.storage.googleapis.com/90.0.4430.24/chromedriver_linux64.zip
RUN unzip /tmp/chromedriver.zip -d /usr/local/bin
```

After installing our browser we need to install python3 and pip and then to install our dependencies.

```dockerfile
RUN apt-get install -y python3 python3-pip python3-venv
RUN python3 -m venv env
RUN python3 -m pip install robotframework robotframework-seleniumlibrary robotframework-requests
```

Now we have all the installations made we need to copy our testing code over to the docker container and setup the remaining commands necessary to make this container functional.

```dockerfile
COPY . ./
USER root
ENV CHROME_DRIVER=/usr/local/bin/chromedriver
RUN chmod +x /run.sh
ENTRYPOINT [ "./run.sh" ]
```

With all of this setup we are ready to build and run our docker container.

```dockerfile
FROM ubuntu:latest
RUN apt-get update
RUN apt-get clean
RUN apt-get install -y x11vnc
RUN apt-get install -y xvfb
RUN apt-get install -y fluxbox
RUN apt-get install -y wget
RUN apt-get install -y wmctrl
RUN apt-get install -y ca-certificates software-properties-common apt-transport-https
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \ && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list \ && apt-get update \ && apt-get install -y google-chrome-stable  

RUN apt-get install -y unzip
RUN wget -O /tmp/chromedriver.zip https://chromedriver.storage.googleapis.com/90.0.4430.24/chromedriver_linux64.zip
RUN unzip /tmp/chromedriver.zip -d /usr/local/bin
RUN apt-get install -y python3 python3-pip python3-venv
RUN python3 -m venv env
RUN python3 -m pip install robotframework robotframework-seleniumlibrary robotframework-requests

COPY . ./
USER root
ENV CHROME_DRIVER=/usr/local/bin/chromedriver
RUN chmod +x /run.sh
ENTRYPOINT [ "./run.sh" ]
```
