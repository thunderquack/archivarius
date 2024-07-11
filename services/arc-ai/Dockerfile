FROM mcr.microsoft.com/cbl-mariner/base/python:3 AS builder

RUN tdnf install -y tar ca-certificates build-essential cmake curl python3-devel python3-setuptools python3-wheel python3-pip python3-numpy python3-flatbuffers python3-packaging python3-protobuf

RUN tdnf install -y git

ARG ONNXRUNTIME_VERSION

RUN git clone --branch $ONNXRUNTIME_VERSION https://github.com/microsoft/onnxruntime.git /code

RUN /code/dockerfiles/scripts/install_cmake.sh

RUN cd /code && /bin/bash ./build.sh --allow_running_as_root --skip_submodule_sync --config Release --build_wheel --update --build --parallel --cmake_extra_defines ONNXRUNTIME_VERSION=$(cat ./VERSION_NUMBER)

FROM mcr.microsoft.com/cbl-mariner/base/python:3

COPY --from=builder /code/build/Linux/Release/dist /root
COPY --from=builder /code/dockerfiles/LICENSE-IMAGE.txt /code/LICENSE-IMAGE.txt

RUN tdnf install -y ca-certificates python3-setuptools python3-wheel python3-pip python3-numpy python3-flatbuffers python3-packaging python3-protobuf python3-mpmath python3-sympy && \
    python3 -m pip install coloredlogs humanfriendly && \
    python3 -m pip install --no-index --find-links /root onnxruntime && \
    rm -rf /root/*.whl

ARG VERSION

WORKDIR /app

RUN mkdir /tmp/repo

RUN tdnf install -y git

RUN echo hi

RUN git clone --branch $VERSION https://github.com/thunderquack/arc-ai.git /tmp/repo

RUN cp -r /tmp/repo/* . && rm -rf /tmp/repo

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8010 5679

CMD ["python", "app.py"]