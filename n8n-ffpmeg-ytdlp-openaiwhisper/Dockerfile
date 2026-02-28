##############################################
# n8n 1.80.2 + Execute Command + Whisper + yt-dlp + ffmpeg
##############################################

# Base image from n8n 1.80.2 (Alpine-based)
FROM n8nio/n8n:1.80.2

USER root

# -----------------------------------------
# Install system dependencies (Alpine)
# -----------------------------------------
RUN apk update && apk add --no-cache \
    ffmpeg \
    python3 \
    py3-pip \
    curl \
    ca-certificates \
    bash \
    build-base \
    libsndfile \
    libffi-dev \
    openssl-dev

# -----------------------------------------
# Upgrade pip and install Whisper & yt-dlp
# -----------------------------------------
RUN python3 -m pip install --upgrade pip && \
    python3 -m pip install \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    python3 -m pip install \
        openai-whisper \
        yt-dlp \
        numpy<2

# -----------------------------------------
# Verify directories
# -----------------------------------------
RUN mkdir -p /tmp/n8n && chown -R node:node /tmp/n8n

# Switch back to node user for n8n
USER node

EXPOSE 5678
