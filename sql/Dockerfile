FROM mcr.microsoft.com/mssql/server:2019-latest
LABEL maintainer "André Raabe <andre.raabe@gmail.com>"

ENV TZ=Europe/Berlin

ARG INSTALL_TOOLS=False

COPY ./scripts /
USER root
RUN chmod +x db-init.sh
RUN chmod +x install-tools.sh

RUN ./install-tools.sh $INSTALL_TOOLS

RUN /opt/mssql/bin/mssql-conf set sqlagent.enabled true
RUN /opt/mssql/bin/mssql-conf set hadr.hadrenabled  1
RUN /opt/mssql/bin/mssql-conf set memory.memorylimitmb 2048

CMD /bin/bash ./entrypoint.sh

HEALTHCHECK --interval=15s --timeout=3s --start-period=30s --retries=10 \
    CMD /opt/mssql-tools/bin/sqlcmd -S ${HOSTNAME},${TCP_PORT} -U sa -P ${SA_PASSWORD} -d tempdb -Q "SELECT 1" || exit 1
