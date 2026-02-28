# ----------------------------
# Base image: n8n 1.80.2 (Debian-based)
# ----------------------------
FROM n8nio/n8n:1.80.2

# ----------------------------
# Switch to root to install packages
# ----------------------------
USER root

# ----------------------------
# Install system dependencies
# ----------------------------
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
    && rm -rf /var/lib/apt/lists/*

# ----------------------------
# Create a virtual environment for Python packages
# ----------------------------
RUN python3 -m venv /opt/venv

# ----------------------------
# Install Python packages inside venv
# Torch CPU 2.3.1, Whisper, yt-dlp, numpy<2
# ----------------------------
RUN /opt/venv/bin/pip install --upgrade pip && \
    /opt/venv/bin/pip install torch==2.3.1+cpu -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    /opt/venv/bin/pip install openai-whisper yt-dlp "numpy<2"

# ----------------------------
# Add venv binaries to PATH
# ----------------------------
ENV PATH="/opt/venv/bin:$PATH"

# ----------------------------
# Switch back to n8n user
# ----------------------------
USER node

# ----------------------------
# Environment variables for Execute Command Node
# ----------------------------
ENV EXECUTE_COMMAND_ALLOWED="true" \
    EXCLUDE_NODES="[]" \
    N8N_ENCRYPTION_KEY="R7d9Fv3Xq8Hk2Lp6WzT1Ys4Nf5Cj0VmP" \
    N8N_JWT_SECRET="4a8e1d9f6c2b7e0a5d3c1f9b6e2a4d8P" \
    N8N_PROTOCOL="https" \
    NODE_ENV="production"

# ----------------------------
# Default command (same as base n8n)
# ----------------------------
CMD ["n8n"]
