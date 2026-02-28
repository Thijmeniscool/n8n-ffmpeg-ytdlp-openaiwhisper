############################################
# n8n + Execute Command + ffmpeg + yt-dlp + Whisper
# Node 22, Debian slim
############################################

FROM node:22-slim

# -----------------------------------------
# Set user to root so Execute Command node works
# -----------------------------------------
USER root

# -----------------------------------------
# System dependencies
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
# Install Python packages: Whisper + yt-dlp
# -----------------------------------------
RUN python3 -m pip install --upgrade pip --break-system-packages && \
    python3 -m pip install --break-system-packages \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    python3 -m pip install --break-system-packages \
        openai-whisper \
        yt-dlp

# -----------------------------------------
# Writable temp dirs
# -----------------------------------------
RUN mkdir -p /tmp/n8n

# -----------------------------------------
# Expose default n8n port
# -----------------------------------------
EXPOSE 5678

# -----------------------------------------
# Default command
# -----------------------------------------
ENTRYPOINT ["n8n"]
