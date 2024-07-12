FROM python:3.12-slim

WORKDIR /app

RUN mkdir /tmp/repo

RUN apt-get update && apt-get install -y git && apt-get clean

ARG VERSION

RUN git clone --branch $VERSION https://github.com/thunderquack/arc-be.git /tmp/repo

RUN cp -r /tmp/repo/* . && rm -rf /tmp/repo

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 3000 5678

CMD ["python", "app.py"]