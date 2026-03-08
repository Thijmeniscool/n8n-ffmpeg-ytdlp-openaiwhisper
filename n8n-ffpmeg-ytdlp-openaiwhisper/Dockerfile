##############################################
# Debian slim + Node 22 + n8n 2.7.4 + Whisper + yt-dlp + ffmpeg + Deno
##############################################
FROM node:22-slim
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
        python3-venv \
        python3-pip \
        bash \
        libsndfile1 \
        build-essential \
        libffi-dev \
        libssl-dev \
        ca-certificates \
        unzip \
    && rm -rf /var/lib/apt/lists/*

# -----------------------------------------
# Install Deno (for yt-dlp JS challenge solving)
# -----------------------------------------
RUN curl -fsSL https://deno.land/install.sh | DENO_INSTALL=/usr/local sh

# -----------------------------------------
# Install n8n 2.7.4 globally
# -----------------------------------------
RUN npm install -g n8n@2.7.4

# -----------------------------------------
# Python virtual environment for Whisper + yt-dlp
# -----------------------------------------
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN pip install --upgrade pip && \
    pip install torch==2.3.1+cpu -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    pip install openai-whisper yt-dlp "numpy<2" youtube-transcript-api

# -----------------------------------------
# Pre-download Whisper tiny model
# -----------------------------------------
RUN python3 -c "import whisper; whisper.load_model('tiny')"

# -----------------------------------------
# Pre-cache yt-dlp EJS challenge solver script
# into a shared directory accessible by node user
# -----------------------------------------
RUN mkdir -p /opt/yt-dlp-cache && \
    yt-dlp \
        --remote-components ejs:github \
        --cache-dir /opt/yt-dlp-cache \
        --skip-download \
        "https://www.youtube.com/watch?v=dQw4w9WgXcQ" || true && \
    chmod -R 755 /opt/yt-dlp-cache

# -----------------------------------------
# Cookies for yt-dlp authentication
# -----------------------------------------
RUN mkdir -p /home/node
COPY cookies.txt /home/node/cookies.txt
RUN chown node:node /home/node/cookies.txt && chmod 600 /home/node/cookies.txt

# -----------------------------------------
# Environment variables
# -----------------------------------------
ENV EXECUTE_COMMAND_ALLOWED=true \
    NODES_EXCLUDE="[]" \
    N8N_RUNNERS_ENABLED=true \
    NODE_ENV=production \
    YT_DLP_CACHE_DIR=/opt/yt-dlp-cache

# Create n8n temp directory (writable)
RUN mkdir -p /tmp/n8n && chown -R node:node /tmp

USER node
EXPOSE 5678
ENTRYPOINT ["n8n"]
