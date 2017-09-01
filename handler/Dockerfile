FROM tensorflow/tensorflow

ADD https://github.com/alexellis/faas/releases/download/0.6.2/fwatchdog /usr/bin
RUN chmod +x /usr/bin/fwatchdog

RUN mkdir -p /tmp/imagenet
ADD ./inception-2015-12-05.tgz /tmp/imagenet

#RUN mkdir /tmp/imagenet
#RUN tar -xvf /tmp/inception-2015-12-05.tgz -C /tmp/imagenet/
#RUN mv /tmp/inception-2015-12-05.tgz /tmp/imagenet/inception-2015-12-05.tgz

WORKDIR /root/
COPY images images

COPY index.py           .
COPY requirements.txt   .
RUN pip install -r requirements.txt

COPY function           function

RUN touch ./function/__init__.py

WORKDIR /root/function/
COPY function/requirements.txt	.
RUN pip install -r requirements.txt

WORKDIR /root/

ENV fprocess="python index.py"
ENV TF_CPP_MIN_LOG_LEVEL=3

HEALTHCHECK --interval=1s CMD [ -e /tmp/.lock ] || exit 1

CMD ["fwatchdog"]
