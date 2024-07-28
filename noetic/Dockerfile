FROM osrf/ros:noetic-desktop-full

# Docker実行してシェルに入ったときの初期ディレクトリ（ワークディレクトリ）の設定
WORKDIR /root/

# Upgrade Packages
RUN apt-get update -q && apt-get upgrade -y

# Utility packages
RUN apt-get install -y software-properties-common
RUN apt-get install -y git build-essential wget curl vim sudo lsb-release locales

# Install X11 requirements
RUN apt-get install -y \
    iputils-ping telnet x11-apps \
    g++ automake intltool libgstreamer*-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Qt5
RUN apt-add-repository universe && apt-get update
RUN apt-get install -y qt5-default #qtscript5-dev libqwt-qt5-dev

# Setting for Japanese
RUN apt-get install -y language-pack-ja-base language-pack-ja fonts-takao
RUN update-locale LANG=ja_JP.UTF8

# Install Python packages
RUN apt-get install -y python3-pip python3-dev python3-tk python-is-python3
RUN pip install pymsgbox tkfilebrowser
RUN pip install scipy
RUN pip install open3d==0.13.0

# Node.js setup
RUN apt-get install -y nodejs npm \
    && apt-get autoclean \
    && apt-get autoremove
RUN npm install -g n
RUN n 16.20.2
RUN apt-get purge -y nodejs npm && apt-get autoremove -y
RUN npm install -g rosnodejs js-yaml mathjs terminate ping

# Rovi setup(Aravis)
RUN wget http://ftp.gnome.org/pub/GNOME/sources/aravis/0.6/aravis-0.6.0.tar.xz
RUN tar -xvf aravis-0.6.0.tar.xz
RUN cd aravis-0.6.0 && ./configure && make && make install

# CAPC
RUN pip install pyserial
RUN pip install git+https://github.com/CA-Giken/capc-host.git
RUN pip install git+https://github.com/UniversalRobots/RTDE_Python_Client_Library.git

# Set user
ARG UID=1000
RUN useradd -m -u ${UID} ros

# Envs for ROS
RUN sh -c 'echo "export ROS_HOSTNAME=localhost" >> ~/.bashrc'
RUN sh -c 'echo "export ROS_MASTER_URI=http://localhost:11311" >> ~/.bashrc'
RUN sh -c 'echo "export NODE_PATH=/usr/local/lib/node_modules" >> ~/.bashrc'
RUN sh -c 'echo "export PYTHONPATH=/usr/local/lib/python3.8/dist-packages:$PYTHONPATH" >> ~/.bashrc'
RUN sh -c 'echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc'


