FROM python:3.6

RUN mkdir -p /scraping/data

WORKDIR /scraping

RUN wget -q https://dl-ssl.google.com/linux/linux_signing_key.pub

ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn

RUN apt-key add linux_signing_key.pub

RUN rm linux_signing_key.pub

RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'

RUN apt-get update

RUN apt-get install -y google-chrome-stable

RUN VER=$(google-chrome --version|sed -r 's/[^0-9.]+//g;s/\.[0-9]+//g') \
    && LATEST_VER=$(curl https://chromedriver.storage.googleapis.com/LATEST_RELEASE_$VER) \
    && wget -q https://chromedriver.storage.googleapis.com/$LATEST_VER/chromedriver_linux64.zip

RUN unzip chromedriver_linux64.zip

RUN rm chromedriver_linux64.zip

COPY requirements.txt requirements.txt

RUN pip install -r requirements.txt

