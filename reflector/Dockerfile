FROM macchina/reflector
COPY reflector.license /home/reflector/etc/reflector.license
COPY reflector.properties /home/reflector/etc/reflector.properties
# begin custom branding
COPY branding /home/reflector/branding
RUN cd /home/reflector/branding && bundle -o/home/reflector/lib/bundles branding.bndlspec
# end custom branding
ENV REDIS_HOST=redis
ENV REDIS_PORT=6379
