FROM python:3.12-slim

ARG VERSION

WORKDIR /app

RUN mkdir /tmp/repo

RUN git clone --branch $VERSION https://github.com/thunderquack/arc-ai.git /tmp/repo

RUN cp -r /tmp/repo/* . && rm -rf /tmp/repo

RUN pip install --no-cache-dir -r requirements.txt

RUN huggingface-cli download microsoft/Phi-3-mini-128k-instruct-onnx --include cpu_and_mobile/cpu-int4-rtn-block-32-acc-level-4/* --local-dir /models/Phi-3-mini-128k-instruct-onnx

EXPOSE 8010 5679

CMD ["python3", "app.py"]