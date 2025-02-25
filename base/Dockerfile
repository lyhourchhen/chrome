FROM ubuntu:20.04

ARG BLESS_USER_ID=999

COPY fonts.conf /etc/fonts/local.conf

# Dependencies + NodeJS
RUN apt-get -qq update && \
  echo "ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true" | debconf-set-selections && \
  apt-get -y -qq install software-properties-common &&\
  apt-add-repository "deb http://archive.canonical.com/ubuntu $(lsb_release -sc) partner" && \
  apt-get -qq update && apt-get -y -qq --no-install-recommends install \
  dumb-init \
  git \
  ffmpeg \
  fonts-liberation \
  msttcorefonts \
  fonts-roboto \
  fonts-ubuntu \
  fonts-noto-color-emoji \
  fonts-noto-cjk \
  fonts-ipafont-gothic \
  fonts-wqy-zenhei \
  fonts-kacst \
  fonts-freefont-ttf \
  fonts-thai-tlwg \
  fonts-indic \
  fontconfig \
  libappindicator3-1 \
  pdftk \
  unzip \
  locales \
  gconf-service \
  libasound2 \
  libatk1.0-0 \
  libc6 \
  libcairo2 \
  libcups2 \
  libdbus-1-3 \
  libexpat1 \
  libfontconfig1 \
  libgcc1 \
  libgconf-2-4 \
  libgdk-pixbuf2.0-0 \
  libglib2.0-0 \
  libgtk-3-0 \
  libnspr4 \
  libpango-1.0-0 \
  libpangocairo-1.0-0 \
  libstdc++6 \
  libx11-6 \
  libx11-xcb1 \
  libxcb1 \
  libxcomposite1 \
  libxcursor1 \
  libxdamage1 \
  libxext6 \
  libxfixes3 \
  libxi6 \
  libxrandr2 \
  libxrender1 \
  libxss1 \
  libxtst6 \
  libllvm8 \
  libgbm-dev \
  ca-certificates \
  libappindicator1 \
  libnss3 \
  lsb-release \
  xdg-utils \
  wget \
  xvfb \
  curl

# Only install Adobe Flash  on amd64 (not available for other architectures)
RUN if [ "$(dpkg --print-architecture)" = "amd64" ]; then apt-get -qq --no-install-recommends install adobe-flashplugin; fi

# More dependencies + NodeJS + cleanup
RUN  curl --silent --location https://deb.nodesource.com/setup_16.x | bash - &&\
  apt-get -qq install nodejs &&\
  apt-get -qq install build-essential &&\
  fc-cache -f -v &&\
  apt-get -qq clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Add the browserless user (blessuser)
RUN groupadd -r blessuser && useradd --uid ${BLESS_USER_ID} -r -g blessuser -G audio,video blessuser \
  && mkdir -p /home/blessuser/Downloads \
  && chown -R blessuser:blessuser /home/blessuser

# Install deps necessary to build
RUN npm install -g typescript @types/node
