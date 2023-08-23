FROM ann-benchmarks

# https://github.com/pgvector/pgvector/blob/master/Dockerfile

RUN DEBIAN_FRONTEND=noninteractive apt-get -y install tzdata
RUN apt-get update && apt-get install -y --no-install-recommends build-essential postgresql postgresql-server-dev-all

COPY --from=tensorchord/pgvecto-rs-binary /pgvecto-rs-binary-release.deb /tmp/vectors.deb
RUN apt-get install -y /tmp/vectors.deb && \
    rm -f /tmp/vectors.deb && \
    echo "echo 'shared_preload_libraries = '\"'\"'vectors.so'\"'\"'' >> /var/lib/postgresql/data/postgresql.auto.conf" > /docker-entrypoint-initdb.d/vectors.sh
USER postgres
RUN service postgresql start && \
    psql -c "CREATE USER ann WITH ENCRYPTED PASSWORD 'ann'" && \
    psql -c "CREATE DATABASE ann" && \
    psql -c "GRANT ALL PRIVILEGES ON DATABASE ann TO ann" && \
    psql -d ann -c "CREATE EXTENSION vectors" && \
    psql -c "ALTER USER ann"
USER root