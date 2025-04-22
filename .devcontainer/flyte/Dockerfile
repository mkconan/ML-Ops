FROM python:3.10-slim

WORKDIR /root
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PYTHONPATH /root

# Set your wandb API key and user name. Get the API key from https://wandb.ai/authorize.
# ENV WANDB_API_KEY <api_key>
# ENV WANDB_USERNAME <user_name>

# Virtual environment
ENV VENV /opt/venv
RUN python3 -m venv ${VENV}
ENV PATH="${VENV}/bin:$PATH"

# Install Python dependencies
COPY requirements.txt /root
RUN pip install --upgrade pip
RUN pip install -r /root/requirements.txt

# Copy the actual code
COPY . /root/

# This tag is supplied by the build script and will be used to determine the version
# when registering tasks, workflows, and launch plans
ARG tag
ENV FLYTE_INTERNAL_IMAGE $tag
ENV TERM xterm-256color

RUN apt update && apt install -y \
    git \
    curl \
    build-essential \
    jq
# UVのインストール
RUN curl -LsSf https://astral.sh/uv/install.sh | sh
ENV PATH="/root/.local/bin:$PATH"
ENV FLYTECTL_CONFIG="~/.flyte/config-sandbox.yaml"
EXPOSE 30080

RUN uv python install 3.12
RUN uv tool install flytekit
RUN uv tool update-shell
RUN curl -sL https://ctl.flyte.org/install | bash -s -- -b /usr/local/bin