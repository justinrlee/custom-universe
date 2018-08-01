# Custom Universe

This repo is used as a skeleton to build a custom universe (similar to a local universe, but does not re-host all artifacts internally).

## Prereqs

This must be built on a Linux machine, with the following prereqs:
- Docker
- python3
- pip3
- jsonschema (for python3)
- git

For example, to install the python-related prereqs on a CentOS box, using epel:

```bash
sudo yum install -y git
sudo yum install -y epel-release
sudo yum install -y python34 python34-pip
sudo pip3 install jsonschema
```

## Usage Example:

There is an example package (Ghost) in the `example` branch:

```bash
# Get the repo and cd in
git clone https://github.com/justinrlee/custom-universe.git
cd custom-universe

# Switch to example repo
git checkout example

# Build the repo metadata
bash scripts/build.sh

# Copy metadata into build directory
cp -rpv target docker/server/

# Build Docker image
docker build -t customer/custom-universe:latest
docker tag customer/custom-universe:latest customer/custom-universe:$(date +%s)

# Run the Docker image (the container exposes port 80, this listens on port 8080)
docker run -d -p 8080:80 customer/custom-universe:latest
```

Then, you can add the repo to your DC/OS cluster like this (replace with the correct IP address):
```bash
dcos package repo add custom-universe http://10.10.0.100:8080/repo
```