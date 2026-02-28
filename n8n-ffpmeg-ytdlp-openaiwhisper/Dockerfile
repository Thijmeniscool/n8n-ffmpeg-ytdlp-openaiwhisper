# ----------------------------
# Base image: Debian slim with Node 18 (matches n8n 1.80.2 requirements)
# ----------------------------
FROM node:18-slim

# ----------------------------
# Install system dependencies
# ----------------------------
USER root
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
# Install n8n 1.80.2 globally
# ----------------------------
RUN npm install -g n8n@1.80.2

# ----------------------------
# Create a virtual environment for Python packages
# ----------------------------
RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

# ----------------------------
# Install Python packages inside venv
# ----------------------------
RUN pip install --upgrade pip && \
    pip install torch==2.3.1+cpu -f https://download.pytorch.org/whl/cpu/torch_stable.html && \
    pip install openai-whisper yt-dlp "numpy<2"

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
# Use non-root user for n8n
# ----------------------------
RUN useradd -ms /bin/bash n8nuser
USER n8nuser
WORKDIR /home/n8nuser

# ----------------------------
# Default command
# ----------------------------
CMD ["n8n"]
