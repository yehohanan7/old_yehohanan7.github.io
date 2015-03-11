---
layout: post_page
title:  "Development Environment using docker"
date:   2014-12-22 12:22:40
categories: devops
---

Having a recreatable development environment is something that I always want. I hate manually configuring my environment every time i use a different laptop, I want my development environment to be accessible over the cloud so i can pull it any time and start working on it.

I used vagrant & Ansible to build my development box with all softwares required. It works quite well and I have no problem with it, but recently i started prefering docker because it's very lightweight.

This Blog is a step by step guide to building your development environment using docker and ansible.

<ul>
<li> Choose a base image - I prefer working on ubuntu image (ubuntu:14.04)

<li> Create a Dockerfile to build your docker image - Below is a sample (working) Dockerfile for your reference

{% highlight bash %}

VERSION 0.0.3
FROM ubuntu:14.04

RUN apt-get -qq update

# Install prereqs
RUN apt-get --no-install-recommends -y install \
    build-essential \
    openssh-client \
    ca-certificates \
    curl \
    git \
    libicu-dev \
    libmozjs185-dev \
    python \
    python-software-properties \
    software-properties-common
    

# Build and install ansible
RUN apt-get install -y python-yaml python-jinja2 python-httplib2 python-keyczar \
python-paramiko python-setuptools python-pkg-resources git python-pip
RUN mkdir /etc/ansible/
RUN echo '[local]\nlocalhost\n' > /etc/ansible/hosts
RUN mkdir /opt/ansible/
RUN git clone http://github.com/ansible/ansible.git /opt/ansible/ansible
WORKDIR /opt/ansible/ansible
RUN git submodule update --init
ENV PATH /opt/ansible/ansible/bin:/bin:/usr/bin:/sbin:/usr/sbin
ENV PYTHONPATH /opt/ansible/ansible/lib
ENV ANSIBLE_LIBRARY /opt/ansible/ansible/library


# Provision the dev box
RUN git clone http://github.com/yehohanan7/scm.git /tmp/scm
WORKDIR /tmp/scm
RUN ansible-playbook ansible/site.yml -i hosts 

{% endhighlight %}

If you notice, I install some basic softwares that are required like Ansible. I need ansible so that i can provision my box using the ansible script, This helps me to organize my provisioning script neatly instead of bloating the Dockerfile with shell scripts which goes out of control.

<li> The last section of the Dockerfile show's how i clone my SCM (ansible) scripts and run the playbook to provision the docker image.

<li> Create an account with docker hub registry.

<li> Create a new automated build which points to the github repo that contains your Dockerfile at the root folder.

<li> Trigger the build and watch the docker hub build your Docker image using the Dockerfile.


I use the below script file to pull the image, you can notice how I mount my file system on to the docker image to get persistence

{% highlight bash %}

#!/bin/sh -e
sudo docker run -v /path/to/couchdb:/couchdb -i -t -P yehohanan7/devbox bash

{% endhighlight %}
