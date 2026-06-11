# tplin1999.github.io

我的技術部落格，使用 [Jekyll](https://jekyllrb.com/) + [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 主題，
透過 GitHub Actions 自動 build，發布到 GitHub Pages。

網址：<https://tplin1999.github.io>

---

## 如何新增一篇文章

1. 在 `_posts/` 資料夾建立一個新檔案，檔名格式**一定**要是：

   ```
   YYYY-MM-DD-英文標題.md
   ```

   例如：`2026-06-11-ros2-tf2-tutorial.md`

2. 檔案最上方要有 front matter（用 `---` 包起來的設定區）：

   ```markdown
   ---
   title: 文章標題（會顯示在網頁上）
   date: 2026-06-11 12:00:00 +0800
   categories: [大分類, 小分類]   # 最多兩層，例如 [Autoware] 或 [ROS2, tf2]
   tags: [autoware, ros2, docker] # 標籤可以多個，全部小寫
   ---
   ```

3. front matter 下面就是文章內容，直接寫 Markdown 即可。

4. 存檔後 commit 並 push：

   ```bash
   git add .
   git commit -m "新增文章：ROS2 tf2 教學"
   git push
   ```

5. push 後，GitHub Actions 會自動 build。約 1～2 分鐘後到
   <https://tplin1999.github.io> 就能看到新文章。
   build 進度可以在 GitHub repo 的 **Actions** 分頁看到。

---

## 插入圖片

**慣例：每篇文章的圖片放在自己的資料夾，資料夾名稱跟文章檔名相同。**

```
assets/img/posts/2026-06-11-ros2-tf2-tutorial/   ← 和文章檔名一致
└── 你的圖檔.png
```

內文插圖請用**從根目錄開始的完整路徑**（開頭一定要有 `/`）：

```markdown
![圖片說明](/assets/img/posts/2026-06-11-ros2-tf2-tutorial/你的圖檔.png)
```

這種寫法的好處是 **VSCode 的 Markdown 預覽和線上網站都看得到圖**
（VSCode 會把開頭的 `/` 當成專案根目錄，Jekyll 會當成網站根目錄，剛好一致）。

> 重要：
> - 圖片**不要**放在 `_posts/` 裡，那裡的圖片不會被發布、線上會顯示不出來。
> - 路徑開頭一定要有 `/`；若漏掉變成 `assets/...`，VSCode 預覽會找不到圖。

---

## 修改網站設定

網站標題、副標題、語言、社群連結等，都在 [`_config.yml`](_config.yml) 裡。
改完一樣 commit + push 就會生效。

> 注意：改 `_config.yml` 後，本機若有在跑預覽要重新啟動才會生效。

---

## （選用）本機預覽

不裝也沒關係，直接 push 讓雲端 build 即可。如果想在本機先看效果，需要安裝 Ruby，然後：

```bash
bundle install
bundle exec jekyll serve
```

接著瀏覽 <http://127.0.0.1:4000>。
