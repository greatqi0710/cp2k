FROM ubuntu:20.04

# author: Ole Schuett

# Install clang-format, shellcheck, and other Ubuntu packages.
# https://github.com/koalaman/shellcheck
# https://clang.llvm.org/docs/ClangFormat.html
RUN export DEBIAN_FRONTEND=noninteractive DEBCONF_NONINTERACTIVE_SEEN=true && \
    apt-get update -qq && apt-get install -qq --no-install-recommends \
    clang-format \
    git \
    less \
    nano \
    npm \
    python3 \
    python3-pip \
    python3-wheel \
    python3-setuptools \
    shellcheck \
  && rm -rf /var/lib/apt/lists/*

# Install markdownlint-cli and dependencies.
# https://github.com/DavidAnson/markdownlint
# https://github.com/igorshubovych/markdownlint-cli 
WORKDIR /opt/cp2k-precommit
COPY package.json package-lock.json ./
RUN npm install .
RUN ln -s /opt/cp2k-precommit/node_modules/markdownlint-cli/markdownlint.js /usr/bin/markdownlint

# Install black, flask, gunicorn, and dependencies.
# https://github.com/psf/black
COPY requirements.txt .
RUN pip3 install --quiet -r requirements.txt

# Clone cp2k repository (needed for CI mode).
RUN git clone --quiet --recursive --single-branch -b master https://github.com/cp2k/cp2k.git /workspace/cp2k

# Install Flask app.
COPY precommit_server.py .

ARG REVISION
ENV REVISION=${REVISION}

COPY entrypoint.sh .
CMD ["./entrypoint.sh"]

#EOF
