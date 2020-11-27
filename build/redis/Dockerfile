FROM redis:5.0.10-buster
MAINTAINER mofei <root@mofei.org>

# Change Timezone
ARG TIME_ZONE=UTC
ENV TIME_ZONE ${TIME_ZONE}
RUN ln -snf /usr/share/zoneinfo/$TIME_ZONE /etc/localtime && echo $TIME_ZONE > /etc/timezone

COPY sources.list /etc/apt/china.sources.list
RUN if [ ${CHANGE_SOURCE} = true ]; then \
	mv /etc/apt/sources.list /etc/apt/source.list.bak && mv /etc/apt/china.sources.list /etc/apt/sources.list \
;fi
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]