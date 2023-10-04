FROM ankane/pgvector:v0.5.0

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

# Override system pip restrictions and install barman-cloud
RUN set -xe; \
	apt-get update; \
	apt-get install -y --no-install-recommends build-essential python3-pip python3-psycopg2 python3-setuptools python3-dev libsnappy-dev; \
	pip3 install --break-system-packages --upgrade pip; \
	pip3 install --break-system-packages --no-deps -r requirements.txt; \
	rm -rf /var/lib/apt/lists/*;

RUN apt-get update && \
	apt-get install -y locales && \
	sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
	dpkg-reconfigure --frontend=noninteractive locales && \
	update-locale LANG=en_US.UTF-8


ENV PATH="/venv/bin:${PATH}" \
	LANGUAGE="en_US.UTF-8" \
	LANG="en_US.UTF-8" \
	LC_ALL="en_US.UTF-8" \
	LC_CTYPE="en_US.UTF-8"
# the language were necessary because otherwise I got this after adding the venv/bin: https://stackoverflow.com/questions/41956994/initdb-bin-invalid-locale-settings-check-lang-and-lc-environment-variables

# Change the uid of postgres to 26
RUN usermod -u 26 postgres
USER 26
