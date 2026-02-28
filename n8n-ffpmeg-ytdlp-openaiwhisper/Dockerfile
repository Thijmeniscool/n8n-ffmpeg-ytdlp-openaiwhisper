############################################
# n8n + Execute Command + Whisper + yt-dlp
# Railway-safe, fast build, Node 22
############################################

FROM n8nio/n8n:latest

USER root

# ----------------------------
# System dependencies
# ----------------------------
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
        ffmpeg \
        python3 \
        python3-pip \
        curl \
        ca-certificates \
        bash \
        libsndfile1 \
        && rm -rf /var/lib/apt/lists/*

# ----------------------------
# Python / Whisper / yt-dlp
# ----------------------------
RUN python3 -m pip install --upgrade pip --break-system-packages && \
    python3 -m pip install --break-system-packages \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    python3 -m pip install --break-system-packages \
        openai-whisper \
        yt-dlp

# ----------------------------
# Permissions
# ----------------------------
RUN mkdir -p /tmp/n8n && chown -R node:node /tmp/n8n

USER node

# ----------------------------
# Expose n8n
# ----------------------------
EXPOSE 5678