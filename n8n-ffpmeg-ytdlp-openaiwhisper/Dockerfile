FROM node:18-bullseye-slim

USER root

# Install system dependencies
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        curl \
        git \
        ffmpeg \
        python3 \
        python3-pip \
        bash \
        libsndfile1 \
        build-essential \
        libffi-dev \
        libssl-dev \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Install n8n legacy version
RUN npm install -g n8n@0.211.1

# Install Python Whisper and yt-dlp (fixed)
RUN python3 -m pip install --upgrade pip && \
    python3 -m pip install \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    python3 -m pip install openai-whisper yt-dlp

# Temp directory for n8n
RUN mkdir -p /tmp/n8n

# Expose default n8n port
EXPOSE 5678

ENTRYPOINT ["n8n"]
