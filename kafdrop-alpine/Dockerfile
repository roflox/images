# Linux for Health Kafdrop image.
#
# This image servers Obsidian Dynamic's Kafdrop UI (https://github.com/obsidiandynamics/kafdrop) within a 
# Red Hat UBI container.
#
# Environment variables:
# - APP_ROOT: The root application directory. Set in base image.
# - JAVA_HOME: The Java installation directory. Set in base image.

FROM docker.io/obsidiandynamics/kafdrop:latest AS builder

FROM docker.io/linuxforhealth/openjdk-alpine:11

COPY --from=builder /kafdrop.sh ${APP_ROOT}/kafdrop/
COPY --from=builder /kafdrop*/kafdrop*.jar ${APP_ROOT}/kafdrop/
COPY --from=builder /kafdrop*/lib/* ${APP_ROOT}/kafdrop/lib/

WORKDIR ${APP_ROOT}/kafdrop

RUN chmod +x ${APP_ROOT}/kafdrop/kafdrop.sh && \
    chown -R lfh ${APP_ROOT}/kafdrop && \
    cp kafdrop.sh kafdrop.tmp && cat kafdrop.tmp | head -n -1 > kafdrop.sh && rm kafdrop.tmp && \
    echo 'exec java ${ARGS} -jar '"${APP_ROOT}"'/kafdrop/kafdrop*jar ${CMD_ARGS}' >> kafdrop.sh && \
    chgrp -R 0 ${APP_ROOT}/kafdrop && \
    chmod -R g=u ${APP_ROOT}/kafdrop

USER lfh

# kafdrop http port
EXPOSE 9000

ENTRYPOINT ["./kafdrop.sh"]
