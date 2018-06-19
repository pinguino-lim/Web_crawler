FROM python:3.6-stretch

RUN apt-get update && apt-get upgrade --yes && \
    apt-get install --yes nodejs ca-certificates musl-dev gcc make cmake g++ gfortran libpng-dev libfreetype6 libfreetype6-dev libxml2-dev libxslt-dev libopenblas-base libopenblas-dev && \
    apt-get clean



# install google chrome
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
RUN apt-get -y update
RUN apt-get install -y google-chrome-stable

# install chromedriver
RUN apt-get install -yqq unzip
RUN wget -O /tmp/chromedriver.zip http://chromedriver.storage.googleapis.com/`curl -sS chromedriver.storage.googleapis.com/LATEST_RELEASE`/chromedriver_linux64.zip
RUN unzip /tmp/chromedriver.zip chromedriver -d /usr/local/bin/

# docker-compose-mitmproxy
RUN ["pip", "install", "mitmproxy"]
RUN mitmdump -p 8080 -s "inject.py"


RUN pip install jupyter
RUN pip install ipywidgets
RUN jupyter nbextension enable --py widgetsnbextension

RUN pip install jupyterlab && jupyter serverextension enable --py jupyterlab

ENV LANG=C.UTF-8


COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

EXPOSE 8888
RUN mkdir -p /opt/app/data
CMD jupyter lab --ip=* --port=8888 --no-browser --notebook-dir=/opt/app/data --allow-root


