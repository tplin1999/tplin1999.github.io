---
title: Autoware 安裝教學
date: 2026-05-26 12:00:00 +0800
categories: [Autoware, Installation]
tags: [Autoware, Docker, Installation, ROS2]
---

[Autoware 官方安裝教學](https://autowarefoundation.github.io/autoware-documentation/main/installation/#architecture) 提出了3種不同的安裝方法，
1. Docker installation 
2. Source installation 
3. Debian Package installation

我建議用使用 Docker installation，因為這個方式最簡單又沒有相容性的問題

Autoware 是一個 open-source 的自駕系統，版本會不段的更新，可以從 [Autoware 版本清單連結](https://github.com/autowarefoundation/autoware/releases) 看到歷代的版本

接下來會教如何安裝 1.8.0 版本的 Autoware(2026/05/05發布)

## 安裝 Autoware 的步驟

1. 請先確認你的環境中已安裝好 docker

2. 複製資料庫 和 移動到資料夾內
   ```bash
   git clone https://github.com/autowarefoundation/autoware.git
   cd autoware
   ```

3. 移動到 1.8.0 版本的 tag
   ```bash
   git checkout 1.8.0
   ```

4. 安裝 Ansible 和 執行 Docker 腳本 (官方有提供支援NVIDIA GPU 的指令，但會遇到很多相容性的問題，我推薦先使用不包含 NVIDIA GPU 的指令)
   ```bash
   bash ansible/scripts/install-ansible.sh
   ansible-galaxy collection install -f -r ansible-galaxy-requirements.yaml
   ansible-playbook autoware.dev_env.install_docker --skip-tags nvidia
   ```

5. 安裝相關模擬用的資料
   ```bash
   ansible-playbook autoware.dev_env.install_dev_env --tags artifacts
   ```

6. 試著開啟 Autoware - Planning Simulator
   ```bash
   xhost +local:docker

   cd docker/examples/demos/planning-simulator
   HOST_UID=$(id -u) HOST_GID=$(id -g) docker compose run --rm planning-simulator
   ```

如果你有看到 Rviz2 的畫面如下，代表 Autoware 安裝成功

![after-autoware-launch](/assets/img/posts/2026-05-26-autoware-Installation-tutorial/after-autoware-launch.png)

## 地圖沒有顯示的排除方法

如果 Rviz2 沒有出現，或地圖沒有顯示出來，通常是地圖路徑設定錯誤。請依下列步驟確認：

1. 打開 `docker/examples/demos/planning-simulator/docker-compose.yaml`

2. 找到這一行：
   ```yaml
   ${HOME}/autoware_data/maps:/home/aw/autoware_data/maps
   ```

3. 確認冒號左邊 (`${HOME}/autoware_data/maps`) 是你本機實際存放地圖的位置，若不一致請改成正確路徑