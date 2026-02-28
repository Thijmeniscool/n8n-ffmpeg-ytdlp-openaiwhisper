############################################
# n8n + Execute Command + ffmpeg + yt-dlp + Whisper
# Debian slim, Node 22
############################################

FROM node:22-slim

USER root

# -----------------------------------------
# System dependencies (Debian)
# -----------------------------------------
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

# -----------------------------------------
# Install n8n globally
# -----------------------------------------
RUN npm install -g n8n@latest

# -----------------------------------------
# Python packages (Whisper + yt-dlp)
# -----------------------------------------
RUN python3 -m pip install --upgrade pip --break-system-packages && \
    python3 -m pip install --break-system-packages \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    python3 -m pip install --break-system-packages \
        openai-whisper \
        yt-dlp

# -----------------------------------------
# Writable temp directories
# -----------------------------------------
RUN mkdir -p /tmp/n8n && chown -R node:node /tmp

USER node

EXPOSE 5678

ENTRYPOINT ["n8n"]
