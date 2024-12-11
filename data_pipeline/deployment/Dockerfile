# Stage 1: Build Python environment
FROM python:3.9-slim AS build

# Cài đặt môi trường ảo Python và dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    bzip2 unzip xz-utils && \
    python -m venv /opt/venv && \
    rm -rf /var/lib/apt/lists/*
    
ENV PATH="/opt/venv/bin:$PATH"

# Cài đặt dependencies từ requirements.txt
COPY deployment/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Stage 2: Install Java 11
FROM buildpack-deps:bullseye-scm AS java

RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates && \
    rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME="/usr/local/openjdk-11"
ENV PATH="$JAVA_HOME/bin:$PATH"

RUN set -eux; \
    arch="$(dpkg --print-architecture)"; \
    case "$arch" in \
        'amd64') \
            downloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_x64_linux_11.0.16_8.tar.gz'; \
            ;; \
        'arm64') \
            downloadUrl='https://github.com/AdoptOpenJDK/openjdk11-upstream-binaries/releases/download/jdk-11.0.16%2B8/OpenJDK11U-jdk_aarch64_linux_11.0.16_8.tar.gz'; \
            ;; \
        *) echo >&2 "error: unsupported architecture: '$arch'"; exit 1 ;; \
    esac; \
    wget -q -O openjdk.tgz "$downloadUrl"; \
    mkdir -p "$JAVA_HOME"; \
    tar --extract --file openjdk.tgz --directory "$JAVA_HOME" --strip-components=1; \
    rm openjdk.tgz; \
    java --version

# Stage 3: Final image combining Python and Java
FROM python:3.9-slim

# Copy môi trường ảo Python từ stage 1
COPY --from=build /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# Copy cài đặt Java từ stage 2
COPY --from=java /usr/local/openjdk-11 /usr/local/openjdk-11
ENV JAVA_HOME=/usr/local/openjdk-11
ENV PATH="$JAVA_HOME/bin:$PATH"

# Copy mã nguồn ứng dụng
COPY . /data_pipeline
WORKDIR /data_pipeline

# Kiểm tra Java và Python hoạt động
RUN java --version && python --version

#CMD ["python", "app.py"]
