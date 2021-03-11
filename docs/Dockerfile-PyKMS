FROM alpine:3.9

MAINTAINER hoangdh <github.com/hoangdh>

ENV LIB python3-tkinter python3-dev sqlite-dev
ENV DEP build-base git py-pip

RUN apk update \
	&& apk add --no-cache $LIB $DEP \
	&& git clone https://github.com/SystemRage/py-kms /tmp/py-kms \
	&& mv /tmp/py-kms/py-kms /opt/ \
	&& pip install --no-cache-dir tzlocal pysqlite3 \
	&& apk del $DEP; rm -rf /tmp/*

EXPOSE 1688

CMD python3 /opt/py-kms/pykms_Server.py --logfile /var/log/pykms.log --logsize 1 -w RANDOM
