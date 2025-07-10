FROM debian:bullseye-slim

RUN apt-get update && apt-get install -y \
    nut \
    && rm -rf /var/lib/apt/lists/*

COPY nut /etc/nut/

EXPOSE 3493

CMD ["sh", "-c", "upsdrvctl start && upsd && upsmon -D"]
