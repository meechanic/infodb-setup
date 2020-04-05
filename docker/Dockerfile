FROM python:3-buster

ENV MAINDIRECTORY="/opt" \
  SROOT="/opt/static" \
  APPPATH="/opt/app" \
  VENVPATH="/opt/venv" \
  DBPATH="/opt/db" \
  APPUSER=appuser

RUN mkdir -p "$SROOT" "$APPPATH" "$DBPATH" \
  && groupadd -r "$APPUSER" && useradd --no-log-init -r -g "$APPUSER" "$APPUSER" \
  && apt-get update \
  && apt-get install -y --no-install-recommends python3-venv libpq-dev python3-dev \
  && curl https://codeload.github.com/meechanic/infodb/tar.gz/v0_0_1 | tar -xzf - --strip-components=1 -C "$APPPATH"

COPY requirements.txt runner.sh "$MAINDIRECTORY"/

RUN pyvenv "$VENVPATH" \
  && chown -R "$APPUSER":"$APPUSER" "$SROOT" "$APPPATH" "$VENVPATH" "$DBPATH" \
  && . "$VENVPATH"/bin/activate \
  && pip install --upgrade pip setuptools wheel \
  && pip install -r "$MAINDIRECTORY"/requirements.txt \
  && DATABASE_URL="" FSN="" "$APPPATH"/manage.py collectstatic --noinput --clear

EXPOSE 8000

USER "$APPUSER":"$APPUSER"

CMD . "$VENVPATH"/bin/activate && "$MAINDIRECTORY"/runner.sh
