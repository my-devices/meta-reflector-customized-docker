FROM macchina/reflector
COPY reflector.license /home/reflector/etc/reflector.license
COPY reflector.properties /home/reflector/etc/reflector.properties
COPY branding /home/reflector/branding
RUN cd /home/reflector/branding && bundle -o/home/reflector/lib/bundles branding.bndlspec
ENV REDIS_HOST=redis
ENV REDIS_PORT=6379
