FROM debian:stable-slim

ENV DEBIAN_FRONTEND noninteractive

# update debian and install dependencies
RUN \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install --no-install-recommends -y clamav-daemon clamav-freshclam wget && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN \
  wget -O /var/lib/clamav/main.cvd http://database.clamav.net/main.cvd && \
  wget -O /var/lib/clamav/daily.cvd http://database.clamav.net/daily.cvd && \
  wget -O /var/lib/clamav/bytecode.cvd http://database.clamav.net/bytecode.cvd && \
  chown clamav:clamav /var/lib/clamav/*.cvd

RUN \
  mkdir /var/run/clamav && \
  chown clamav:clamav /var/run/clamav && \
  chmod 750 /var/run/clamav

RUN \
  sed -i 's/^Foreground .*$/Foreground true/g' /etc/clamav/clamd.conf && \
  echo "TCPSocket 3310" >> /etc/clamav/clamd.conf && \
  sed -i 's/^Foreground .*$/Foreground true/g' /etc/clamav/freshclam.conf

EXPOSE 3310

ADD run.sh /

CMD ["/run.sh"]
