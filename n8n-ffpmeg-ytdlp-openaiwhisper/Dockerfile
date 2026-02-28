##############################################
# n8n 1.80.2 + Execute Command + Whisper + yt-dlp + ffmpeg
# Python packages installed in a virtual environment to avoid PEP 668 errors
##############################################

FROM n8nio/n8n:1.80.2

USER root

# -----------------------------------------
# Install system dependencies
# -----------------------------------------
RUN apk update && apk add --no-cache \
    ffmpeg \
    python3 \
    py3-pip \
    bash \
    curl \
    git \
    libsndfile \
    build-base \
    libffi-dev \
    openssl-dev \
    ca-certificates

# -----------------------------------------
# Create Python virtual environment
# -----------------------------------------
RUN python3 -m venv /opt/venv

# Install Whisper, PyTorch CPU, yt-dlp, NumPy inside the venv
RUN /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip install \
        torch==2.1.2+cpu \
        -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    /opt/venv/bin/pip install \
        openai-whisper \
        yt-dlp \
        numpy<2

# -----------------------------------------
# Optional: add the venv to PATH for convenience
# -----------------------------------------
ENV PATH="/opt/venv/bin:$PATH"

# -----------------------------------------
# Ensure tmp directories exist
# -----------------------------------------
RUN mkdir -p /tmp/n8n && chown -R node:node /tmp/n8n

# Switch back to n8n user
USER node

EXPOSE 5678
