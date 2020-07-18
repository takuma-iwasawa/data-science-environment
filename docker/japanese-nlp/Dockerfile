FROM continuumio/anaconda3

RUN apt update \
  && apt install -y --no-install-recommends build-essential unzip sudo libboost-dev iputils-ping net-tools procps

# Create Jupyter Notebook config file
RUN mkdir -p /root/.jupyter \
  && echo "c.NotebookApp.allow_root = True" >> /root/.jupyter/jupyter_notebook_config.py \
  && echo "c.NotebookApp.ip = '*'" >> /root/.jupyter/jupyter_notebook_config.py \
  && echo "c.NotebookApp.token = ''" >> /root/.jupyter/jupyter_notebook_config.py \
  && conda install -y -c conda-forge jupyter_contrib_nbextensions

EXPOSE 8888

# Install MeCab
RUN apt install -y --no-install-recommends mecab libmecab-dev mecab-ipadic-utf8 \
  && pip install mecab-python3

# Install mecab-ipadic-NEologd & enable ipadic
RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git /tmp/neologd \
  && /tmp/neologd/bin/install-mecab-ipadic-neologd -n -a -y \
  && rm -rf /tmp/neologd \
  && cp -r /var/lib/mecab/dic/ipadic-utf8/ /usr/lib/x86_64-linux-gnu/mecab/dic/ \
  && cp -r /var/lib/mecab/dic/ipadic/ /usr/lib/x86_64-linux-gnu/mecab/dic/ \
  && sed -i -e "s|^dicdir.*$|dicdir = /usr/lib/x86_64-linux-gnu/mecab/dic/ipadic-utf8|" /etc/mecabrc 

# Install neologdn
RUN pip install neologdn

# Install JUMAN & KNP
ARG JUMANPP_VERSION
ARG JUMAN_VERSION
ARG KNP_VERSION
# install JUMANPP
RUN wget http://lotus.kuee.kyoto-u.ac.jp/nl-resource/jumanpp/jumanpp-${JUMANPP_VERSION}.tar.xz -O /tmp/jumanpp.tar.xz &&\
    tar xJvf /tmp/jumanpp.tar.xz -C /tmp &&\
    cd /tmp/jumanpp-${JUMANPP_VERSION} &&\
    ./configure --prefix=/usr/local/ && make && make install &&\
    rm -rf /var/cache/apk/*
# install JUMAN
RUN wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/juman/juman-${JUMAN_VERSION}.tar.bz2 -O /tmp/juman.tar.bz2 &&\ 
    tar xf /tmp/juman.tar.bz2 -C /tmp &&\
    cd /tmp/juman-${JUMAN_VERSION} &&\ 
    ./configure --prefix=/usr/local/ && make && make install &&\
    rm -rf /tmp/juman-${JUMAN_VERSION} &&\
    rm -rf /var/cache/apk/* &&\
    apt-get update && apt-get install -y --fix-missing libjuman4
# install KNP
RUN apt-get install -y --fix-missing zlib1g-dev &&\
    wget http://nlp.ist.i.kyoto-u.ac.jp/nl-resource/knp/knp-${KNP_VERSION}.tar.bz2 -O /tmp/knp.tar.bz2 &&\
    tar xf /tmp/knp.tar.bz2 -C /tmp &&\
    cd /tmp/knp-${KNP_VERSION} / &&\
    ./configure --prefix=/usr/local/ --with-juman-prefix=/usr/local/ && make && make install &&\
    rm -rf /tmp/knp-${KNP_VERSION} &&\
    rm -rf /var/cache/apk/*

# Install CRF++
RUN wget -O /tmp/CRF++-0.58.tar.gz "https://drive.google.com/uc?export=download&id=0B4y35FiV1wh7QVR6VXJ5dWExSTQ" \
  && cd /tmp \
  && tar zxf CRF++-0.58.tar.gz \
  && cd CRF++-0.58 \
  && ./configure \
  && make \
  && make install \
  && cd / \
  && rm /tmp/CRF++-0.58.tar.gz \
  && rm -rf /tmp/CRF++-0.58 \
  && ldconfig

# Install CaboCha
RUN cd /tmp \
  && curl -c cabocha-0.69.tar.bz2 -s -L "https://drive.google.com/uc?export=download&id=0B4y35FiV1wh7SDd1Q1dUQkZQaUU" \
    | grep confirm | sed -e "s/^.*confirm=\(.*\)&amp;id=.*$/\1/" \
    | xargs -I{} curl -b  cabocha-0.69.tar.bz2 -L -o cabocha-0.69.tar.bz2 \
      "https://drive.google.com/uc?confirm={}&export=download&id=0B4y35FiV1wh7SDd1Q1dUQkZQaUU" \
  && tar jxf cabocha-0.69.tar.bz2 \
  && cd cabocha-0.69 \
  && export CPPFLAGS=-I/usr/local/include \
  && ./configure --with-mecab-config=`which mecab-config` --with-charset=utf8 \
  && make \
  && make install \
  && cd python \
  && python setup.py build \
  && python setup.py install \
  && cd / \
  && rm /tmp/cabocha-0.69.tar.bz2 \
  && rm -rf /tmp/cabocha-0.69 \
  && ldconfig

# Install gensim
RUN conda install -y gensim

# Install fastText
RUN pip install git+https://github.com/facebookresearch/fastText.git

# Install Janome
RUN pip install janome

# Install pykakasi
RUN pip install pykakasi

# Install spaCy
RUN pip install spacy

# Install other nlp tools
RUN pip install pyknp nltk JapaneseTokenizer DocumentFeatureSelection emoji

# Install Graphviz
RUN apt install -y --no-install-recommends graphviz graphviz-dev fonts-ipafont
RUN pip install graphviz

# Install machine learning algorithms
RUN conda install -y -c conda-forge lightgbm
RUN conda install -y -c conda-forge xgboost

# Install other tools
RUN conda install -y plotly pyarrow 
RUN conda install -c conda-forge dask-ml
RUN pip install optuna imbalanced-learn pandas-profiling japanize-matplotlib adjusttext japandas
RUN pip install mlflow selenium

# Install my tools
RUN cd /volume/Github/prior-problem-plot/ \
  && pip install .

# mount dir
WORKDIR /volume