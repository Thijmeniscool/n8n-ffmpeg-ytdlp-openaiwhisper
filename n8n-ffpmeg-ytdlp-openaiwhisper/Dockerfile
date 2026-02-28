############################################
# n8n + Execute Command + ffmpeg + yt-dlp + Whisper
# FINAL STABLE VERSION (Alpine, Railway-safe)
############################################

FROM n8nio/n8n:latest

USER root

# -----------------------------------------
# System dependencies (Alpine Linux)
# -----------------------------------------
RUN apk update && apk add --no-cache \
    ffmpeg \
    python3 \
    py3-pip \
    curl \
    ca-certificates \
    bash \
    libsndfile \
    build-base \
    libffi-dev \
    openssl-dev

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
# Ensure writable temp space for n8n + Whisper
# -----------------------------------------
RUN mkdir -p /tmp/n8n && chown -R node:node /tmp

USER node

EXPOSE 5678
