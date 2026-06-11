---
title: Autoware 的 planning simulator GUI 使用教學
date: 2026-06-01 12:00:00 +0800
categories: [Autoware, Planning Simulator]
tags: [Autoware, Planning Simulator]
---

請先參考 [Autoware 安裝教學]({% post_url 2026-05-26-autoware-Installation-tutorial %}) 安裝好 Autoware,再開始本篇教學。

## Planning Simulator 是什麼?

Planning Simulator 是 Autoware 內建的模擬器，讓你不用真車就能測試自動駕駛。
你只要在地圖上指定「車子起點」和「目標終點」，Autoware 就會自己規劃路線並把車開過去。

接下來會帶你跑完一次最基本的流程:開啟模擬器 → 放車 → 設目標 → 讓車自動出發。

## Planning Simulator 自駕車設定教學

1. 開啟模擬器。先到 Autoware 資料夾，執行以下指令:

   ```bash
   xhost +local:docker

   cd docker/examples/demos/planning-simulator
   HOST_UID=$(id -u) HOST_GID=$(id -g) docker compose run --rm planning-simulator
   ```

   執行後會跳出 RViz 視窗(Autoware 的視覺化介面)，畫面中央是一張地圖。
   ![after-autoware-launch](/assets/img/posts/2026-06-01-autoware-planning-simulator-gui/after-autoware-launch.png)

2. **放置車子並設定朝向。** 點一下上方工具列的 `2D Pose Estimate`，
   接著在地圖上你想當「起點」的位置**按住左鍵不放**，往車頭要面向的方向拖曳，放開後車子就會出現。
   > 拖曳的方向 = 車頭朝向，不是把車拖到別處。
   ![set_2D_pose_estimate](/assets/img/posts/2026-06-01-autoware-planning-simulator-gui/set_2D_pose_estimate.png)

3. **設定目標位置。** 點一下 `2D Goal Pose`，一樣在地圖上你想當「終點」的位置按住左鍵，往終點時車頭要面向的方向拖曳，放開後目標就設定完成。
   ![set_2D_pose_estimate](/assets/img/posts/2026-06-01-autoware-planning-simulator-gui/set_2D_goal_pose.png)

4. **讓車出發。** 等幾秒，左上角的 `AUTO` 按鈕會由灰色變成可點擊，按下後車子就會沿著規劃好的路線自動開往目標。
   ![press_auto_button](/assets/img/posts/2026-06-01-autoware-planning-simulator-gui/press_auto_button.png)

## 其他車輛的設定

1. 讀者也可以試試看使用界面上排工具列的 `2D Dummy Car` 設定其他車輛，設定方式一樣是點一下上方工具列的 `2D Dummy Car`，接著在地圖上找一個位置**按住左鍵不放**，往車頭要面向的方向拖曳，放開後車子就會出現。
   ![set_dummy_car](/assets/img/posts/2026-06-01-autoware-planning-simulator-gui/set_dummy_car.png)

---

> **指令說明(進階):** 步驟 1 的指令會讀取
> `docker/examples/demos/planning-simulator/docker-compose.yaml`，
> 依照設定建立一個容器，並在容器內執行 yaml 裡 `command` 欄位的指令來啟動模擬器。
