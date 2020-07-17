
FROM node:10-slim
  
# 设置时区
ENV TZ "Asia/Shanghai"

RUN apt-get update && \
    apt-get install -y wget dpkg unzip lsb-release gnupg2

# 安装指定版本chrome
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' \
    && apt-get update \
    && apt-get install -y google-chrome-unstable \
      --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*
    # && apt-get remove -y google-chrome-unstable \
    # && cd /tmp && wget http://dl.google.com/linux/chrome/deb/pool/main/g/google-chrome-unstable/google-chrome-unstable_76.0.3806.1-1_amd64.deb \
    # && dpkg -i google-chrome-unstable_76.0.3806.1-1_amd64.deb \
    # && rm google-chrome-unstable_76.0.3806.1-1_amd64.deb

# 添加中文字体
RUN cd /tmp && wget http://mirrors.ustc.edu.cn/debian/pool/main/f/fonts-noto-cjk/fonts-noto-cjk_20170601%2Brepack1-3%2Bdeb10u1_all.deb && \
    dpkg -i fonts-noto-cjk_20170601+repack1-3+deb10u1_all.deb && \
    wget https://github.com/adobe-fonts/source-sans-pro/releases/download/2.040R-ro%2F1.090R-it/source-sans-pro-2.040R-ro-1.090R-it.zip && \
    unzip source-sans-pro-2.040R-ro-1.090R-it.zip && cd source-sans-pro-2.040R-ro-1.090R-it && mv ./OTF /usr/share/fonts/  && \
    wget -P /tmp/OPPOSans https://src.visbodyfit.com/fonts/OPPOSans-M.ttf && \
    wget -P /tmp/OPPOSans https://src.visbodyfit.com/fonts/OPPOSans-R.ttf && \
    wget -P /tmp/OPPOSans https://src.visbodyfit.com/fonts/OPPOSans-L.ttf && \
    mv /tmp/OPPOSans/*  /usr/share/fonts/ && \
    fc-cache -f -v

WORKDIR /app

COPY package.json .
COPY yarn.lock .

# 下载PUPPETEER时跳过chrome下载
ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD true

# 安装nrm，设置淘宝源
RUN npm i -g nrm && \
    nrm use taobao

# 安装Nest CLI用于后续编译
RUN npm i -g @nestjs/cli

# 安装运行依赖
RUN npm install --dependencies

# 安装依赖包
RUN rm -rf  package.json yarn.lock