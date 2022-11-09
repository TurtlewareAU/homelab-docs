
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
