############################################
# n8n + Execute Command + ffmpeg + yt-dlp + Whisper
# FINAL CORRECT VERSION (Debian-based)
############################################

FROM n8nio/n8n:latest-debian

USER root

# -----------------------------------------
# System dependencies (Debian)
# -----------------------------------------
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        ffmpeg \
        python3 \
        python3-pip \
        curl \
        ca-certificates \
        bash \
        libsndfile1 \
        build-essential \
        libffi-dev \
        openssl-dev \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------
# Python packages: Whisper + yt-dlp
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
