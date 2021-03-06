#-----------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See LICENSE in the project root for license information.
#-----------------------------------------------------------------------------------------

FROM python:3

# Install the required development packages
RUN pip install pylint requests python-dateutil pytz coverage pytest-cov pytest-mock tox pep8 autopep8 rubrik_cdm paramiko PyYAML Jinja2 httplib2 six voluptuous isort lazy-object-proxy wrapt docutils rstcheck pathspec yamllint

# Configure apt
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils 2>&1

# Install git, process tools, lsb-release (common in install instructions for CLIs), and zsh shell
RUN apt-get -y install git procps lsb-release zsh

# Install any missing dependencies for enhanced language service
RUN apt-get install -y libicu[0-9][0-9]

# Make zsh the default shell
RUN chsh -s $(which zsh)

# Install oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

RUN mkdir /workspace
RUN git clone https://github.com/ansible/ansible.git /workspaces/ansible

RUN mkdir -p /root/.ansible/collections/ansible_collections/rubrikinc/cdm
RUN mkdir -p /workspaces/ansible/lib/ansible/modules/cloud/rubrikinc

ENV PYTHONPATH=/workspaces/rubrik-modules-for-ansible/rubrikinc/cdm:/root/.ansible/collections

WORKDIR /workspace

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
ENV DEBIAN_FRONTEND=dialog

ENV ANSIBLE_COLLECTIONS_PATHS="~/.ansible/collections"

# Set the default shell to zsh rather than sh
ENV SHELL /bin/zsh
