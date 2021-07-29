FROM ubuntu:latest

RUN groupadd -g 3000 g1 && useradd -m -u 2000 -g g1 u1
RUN groupadd -g 3001 g2 && useradd -m -u 2001 -g g2 u2
RUN mkdir -p /etc/message/ && echo "Hello, World!" | tee -a /etc/message/message.txt  && chown 2000:3000 /etc/message/message.txt

