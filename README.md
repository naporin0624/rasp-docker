# raspi-docker

## dockerインストール

```shell
sudo apt update
sudo apt dist-update

curl -sSL https://get.docker.com | sh
```

これで上手く入ればOK

```list /etc/apt/sources.list.d/docker.list
deb [arch=armhf] http://download.docker.com/linux/debian buster stable
```

```shell
sudo apt update && sudo apt install docker-ce docker-ce-cli
```

aufs-dkmsでエラーが吐かれるはず

```shell
sudo dpkg --audit
sudo rm /var/lib/dpkg/info/aufs-dkms.postinst
sudo rm /var/lib/dpkg/info/aufs-dkms.prerm
sudo dpkg --configure aufs-dkms
```

```shell
sudo docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (arm32v7)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## docker-compose

```
git clone https://github.com/docker/compose.git
cd compose
git checkout bump-1.23.2
sed -i -e 's:^VENV=/code/.tox/py36:VENV=/code/.venv; python3 -m venv $VENV:' script/build/linux-entrypoint
sed -i -e '/requirements-build.txt/ i $VENV/bin/pip install -q -r requirements.txt' script/build/linux-entrypoint
docker build -t docker-compose:armhf -f Dockerfile.armhf .
docker run --rm --entrypoint="script/build/linux-entrypoint" -v $(pwd)/dist:/code/dist -v $(pwd)/.git:/code/.git "docker-compose:armhf"
sudo cp dist/docker-compose-Linux-armv7l /usr/local/bin/docker-compose
sudo chown root:root /usr/local/bin/docker-compose
sudo chmod 0755 /usr/local/bin/docker-compose
docker-compose --version
```

