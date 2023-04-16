# vim: ft=dockerfile
FROM python:3.11-alpine AS builder
RUN python3 -m ensurepip
RUN pip3 install --no-cache --upgrade poetry
COPY  ./sbx /sbx/sbx
COPY  ./poetry.lock /sbx/poetry.lock
COPY  ./pyproject.toml /sbx/pyproject.toml
COPY  ./README.md /sbx/README.md
RUN cd /sbx && poetry build --format=wheel

FROM python:3.11-alpine AS nsk-pkg-sbx
RUN python -m ensurepip
RUN pip install --no-cache --upgrade pip setuptools
COPY --from=builder /sbx/dist /tmp/sbx
RUN pip install /tmp/sbx/*.whl && rm -rf /tmp/sbx
COPY docker/entrypoint.sh /usr/bin
CMD /usr/bin/entrypoint.sh
