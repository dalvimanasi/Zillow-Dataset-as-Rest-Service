FROM ubuntu:latest

USER root

# Install dependencies
RUN apt-get update && apt-get install -y \
    python-pip --upgrade python-pip

RUN pip install --upgrade pip

# install py3
RUN apt-get update -qq \
 && apt-get install --no-install-recommends -y \
    # install python 3
    python3 \
    python3-dev \
    python3-pip \
    libmysqlclient-dev \
    python3-setuptools \
    pkg-config \
	vim &&\
	echo "luigi ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    apt-get clean && \
	rm -rf /var/lib/apt/lists/*

#RUN DEBIAN_FRONTEND=noninteractive apt-get -y install mysql-client
#RUN echo "deb http://archive.ubuntu.com/ubuntu precise main universe" > /etc/apt/sources.list


RUN apt-get update

RUN apt-get -y install mysql-client

RUN pip3 install --upgrade pip

# install additional python packages
RUN pip3 install ipython
RUN pip install jupyter
RUN pip3 install numpy
RUN pip3 install pandas
RUN pip3 install scikit-learn
RUN pip3 install matplotlib
RUN pip3 install missingno
RUN pip3 install scipy
RUN pip3 install seaborn
#RUN pip install nltk
RUN pip3 install boto3
RUN pip3 install requests
RUN pip3 install plotly
RUN pip3 install ConfigParser
RUN pip3 install MySQL-python
RUN pip3 install folium
RUN pip3 install --upgrade awscli

RUN pip3 install luigi

RUN pip3 install jupyter
RUN pip install --upgrade awscli

# configure console
RUN echo 'alias ll="ls --color=auto -lA"' >> /root/.bashrc \
 && echo '"\e[5~": history-search-backward' >> /root/.inputrc \
 && echo '"\e[6~": history-search-forward' >> /root/.inputrc
# default password: keras
ENV PASSWD='sha1:98b767162d34:8da1bc3c75a0f29145769edc977375a373407824'

# dump package lists
RUN dpkg-query -l > /dpkg-query-l.txt \
 && pip2 freeze > /pip2-freeze.txt \
 && pip3 freeze > /pip3-freeze.txt

# for jupyter
EXPOSE 8888

#expose for mysql
EXPOSE 3306

WORKDIR /src/

RUN mkdir /src/assignment2
RUN mkdir /src/assignment2/sql
RUN mkdir /src/assignment2/data
RUN mkdir /src/assignment2/logs
RUN mkdir /src/assignment2/config
RUN mkdir /src/assignment2/JupyterNotebooksDBAndApi
ENV MAINPATH=/src/assignment2
ENV SCRIPTSPATH=/src/assignment2/sql
ENV LOGPATH=/src/assignment2/logs
ENV CONFIGPATH=/src/assignment2/config
ENV DATAPATH=/src/assignment2/data
ENV PYTHONPATH=$MAINPATH:$PYTHONPATH

ADD getFromS3AndLoadRDS.sh /src/assignment2/
ADD config.txt /src/assignment2/config/
ADD sql/* /src/assignment2/sql/
ADD data/* /src/assignment2/data/
ADD JupyterNotebooksDBAndApi/* /src/assignment2/JupyterNotebooksDBAndApi/
ADD getFromS3AndLoadRDS.sh /src/assignment2/


#testing
#ADD MA_21062017_WBAN_14702.csv /src/assignment1/output/
#RUN chmod 777 /src/assignment1/output/MA_21062017_WBAN_14702.csv

RUN chmod 777 $MAINPATH/getFromS3AndLoadRDS.sh
RUN chmod 777 $CONFIGPATH/config.txt
RUN chmod 777 $SCRIPTSPATH/*
RUN chmod 777 $DATAPATH/*
RUN chmod 777 /src/assignment2/JupyterNotebooksDBAndApi/*

CMD $MAINPATH/getFromS3AndLoadRDS.sh

#RUN mkdir /srv/nb1/

#CMD /bin/bash -c 'jupyter notebook --no-browser --allow-root --ip=* --NotebookApp.password="$PASSWD" "$@"'

#CMD /bin/bash -c 'jupyter notebook --no-browser --ip=* --NotebookApp.password="keras" "$@"'
