FROM python:3.8.2-slim

ARG OWNER=django

ENV PYTHONDONTWRITEBYTECODE=1
ENV DJANGO_HOME=/opt/django
ENV DJANGO_SETTINGS_MODULE=settings.env.unit

WORKDIR ${DJANGO_HOME}

COPY . .

RUN set -x \
 && groupadd -g 1000 ${OWNER} \
 && useradd -u 1000 -g 1000 -m -s /bin/bash ${OWNER} \
 && apt-get update -yqq \
 && apt-get install -y --no-install-recommends \
        build-essential \
        gcc \
 && pip install -U pip setuptools poetry \
 && poetry config virtualenvs.create false \
 && poetry install --no-root \
 && apt-get -y clean \
 && rm -rf /var/lib/apt/lists/* \
 && ./manage.py compilemessages

USER ${OWNER}
EXPOSE 8000
CMD ["./scripts/runserver.sh"]