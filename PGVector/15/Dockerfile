FROM ankane/pgvector:v0.5.0

# Do not split the description, otherwise we will see a blank space in the labels
LABEL name="PostgreSQL + PG Vector Container Images" \
	vendor="The CloudNativePG Contributors" \
	version="${PG_VERSION}" \
	release="7" \
	summary="PostgreSQL + PG Vector Container images." \
	description="This Docker image contains PostgreSQL, PG Vector and Barman Cloud based on Postgres 15-3.4."

COPY requirements.txt /

# Install additional extensions
RUN set -xe; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
	"postgresql-${PG_MAJOR}-pgaudit" \
	"postgresql-${PG_MAJOR}-pg-failover-slots" \
	"postgresql-${PG_MAJOR}-pgrouting" \
	; \
	rm -fr /tmp/* ; \
	rm -rf /var/lib/apt/lists/*;

# Install barman-cloud and its dependencies without venv
RUN set -xe; \
	apt-get update; \
	apt-get install -y --no-install-recommends build-essential python3-pip python3-psycopg2 python3-setuptools python3-dev libsnappy-dev; \
	pip3 install --upgrade pip; \
	pip3 install --no-deps -r requirements.txt; \
	rm -rf /var/lib/apt/lists/*;

# Change the uid of postgres to 26
RUN usermod -u 26 postgres
USER 26
