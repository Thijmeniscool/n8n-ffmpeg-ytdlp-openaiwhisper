############################################
# Legacy n8n 0.211.1 + Execute Command + ffmpeg + yt-dlp + Whisper
# Node 18, Debian Bullseye slim
############################################

FROM node:18-bullseye-slim

# Run as root for Execute Command node
USER root

# System dependencies
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

# Install Python Whisper and yt-dlp
RUN python3 -m pip install --upgrade pip --break-system-packages && \
    python3 -m pip install --break-system-packages \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    python3 -m pip install --break-system-packages \
        openai-whisper \
        yt-dlp

# Temp dir for n8n
RUN mkdir -p /tmp/n8n

# Expose default n8n port
EXPOSE 5678

# Default command
ENTRYPOINT ["n8n"]
