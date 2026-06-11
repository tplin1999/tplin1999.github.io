---
title: 在 Docker 上安裝 Autoware 1.8.0 並啟動 Planning Simulator
date: 2026-05-26 12:00:00 +0800
categories: [Autoware]
tags: [autoware, ros2, docker, nvidia, simulation]
---

本文記錄如何安裝 Stable 版本（1.8.0）的 Autoware，啟動 Planning Simulator，
以及讓外部環境連進 Autoware 的 Docker 容器。

## 安裝 1.8.0 版本的 Autoware

```bash
git clone https://github.com/autowarefoundation/autoware.git
cd autoware
git checkout 1.8.0

bash ansible/scripts/install-ansible.sh
ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
ansible-playbook autoware.dev_env.install_docker --skip-tags nvidia

ansible-playbook autoware.dev_env.install_dev_env --tags artifacts
```

```bash
echo "net.core.rmem_max=2147483647" | sudo tee -a /etc/sysctl.conf
echo "net.core.rmem_default=10485760" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

## 開啟 Autoware - Planning Simulator

```bash
xhost +local:docker

cd docker/examples/demos/planning-simulator
HOST_UID=$(id -u) HOST_GID=$(id -g) docker compose run --rm planning-simulator
```

另一個開啟 Autoware - Planning Simulator 的方式：

```bash
xhost +local:docker

cd docker/examples/demos/planning-simulator
HOST_UID=$(id -u) HOST_GID=$(id -g) docker compose up -d

# 想要看 Autoware 的 Log 時
docker compose logs -f

# 想要關閉時
docker compose down
```

## 外部環境要連進 Autoware docker 的方法

```bash
# 加到 ~/.bashrc 最後
alias ros2-reset='ros2 daemon stop && ros2 daemon start && sleep 2'

ros2 daemon stop && ros2 daemon start


docker cp planning-simulator-planning-simulator-run-28af3d00b9d3:/home/aw/cyclonedds.xml \
  ~/cyclonedds.xml

source /opt/ros/jazzy/setup.bash

export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export CYCLONEDDS_URI=file://${HOME}/cyclonedds.xml

ros2 topic list
```
