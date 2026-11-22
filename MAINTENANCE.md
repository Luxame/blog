# 部落格維護紀錄

最後更新：2026-11-22

## 專案概況

| 項目 | 內容 |
|---|---|
| 網站 | https://luxame.github.io/blog/ |
| 產生器 | Hexo 8.1.1 |
| 主題 | NexT 7.8.0（Muse），檔案直接放在 `themes/next/` |
| Node.js | 20.19（見 `.nvmrc`） |
| 原始碼分支 | `master` |
| 網站分支 | `gh-pages`（由 `hexo-deployer-git` 部署，GitHub Pages 從這個分支發布） |
| 部署方式 | SSH（`git@github.com:Luxame/blog.git`） |

## 日常操作

```bash
npx hexo new "文章標題"                  # 新增文章
npx hexo clean && npx hexo server        # 本機預覽 http://localhost:4000/blog/
npx hexo clean && npx hexo deploy        # 部署到 gh-pages
```

注意事項：

- 原始碼要另外 `git push origin master`，`hexo deploy` 只會更新網站分支。
- 全域 git 設定有開 GPG 簽章，部署時會跳出 passphrase 視窗。
- `hexo-deployer-git` 會 force push 到 `gh-pages`，這是正常行為，網站可以從原始碼重新產生。
- 本檔案放在專案根目錄，Hexo 只處理 `source/`，所以不會被發布到網站上。

## 已完成

### 2026-11-22

| 項目 | Commit |
|---|---|
| Hexo 6 升級到 8.1.1，官方外掛升到最新大版本；新增 `.nvmrc` 與 `engines` | `e3a42f1` |
| 開始追蹤 `package-lock.json`（從 `.gitignore` 移除） | `e3a42f1` |
| 標籤大小寫統一：`blog` 改為 `Blog`。原本會產生 `tags/blog/` 與 `tags/Blog/`，在 macOS 上互相覆蓋，部署後其中一頁會 404 | `aab64ac` |
| 刪除 `.gitmodules`：主題是直接 commit 的一般檔案，不是 submodule | `c74440a` |
| 部署網址從 HTTPS 改為 SSH，部署時不再需要輸入帳密 | `3fa0c6c` |
| 重新部署網站。上一次部署是 2016-07-24（NexT 5），這次起改為 NexT 7.8 外觀 | — |

檢查紀錄：

- 機密資料：已掃描所有追蹤中的檔案與全部分支的 git 歷史，沒有發現金鑰、token、密碼或本機路徑；主題的第三方服務設定都是空白。
- 文章內的外部連結：2026-11-22 測試全部可以開啟。

## 待處理建議

### 一、優先處理（改動小，線上看得到效果）

- [ ] **1. 網站介面顯示成英文**
  - 現況：選單顯示「Home」「Archives」，文章資訊顯示「Posted on」「Edited on」。
  - 原因：`_config.yml` 寫 `language: zh-tw`，但 NexT 7.8 的語言檔是 `zh-TW.yml`，大小寫不符，所以退回英文。
  - 做法：改成 `language: zh-TW`。已在暫存複本測試，會顯示「首頁」「歸檔」「發表於」。

- [ ] **2. 文章「更新時間」不正確**
  - 現況：`hexo-blog.md` 顯示更新於 2026-01-03，但當天沒有修改內容。
  - 原因：Hexo 預設用檔案修改時間（mtime）當作更新時間，檔案被複製或搬移就會改變。
  - 做法：在 `_config.yml` 加上 `updated_option: date`。沒有寫 `updated:` 的文章會改用發表日期。

- [ ] **3. 套件安全性更新**
  - 現況：`npm audit` 回報 11 個漏洞（8 high、3 moderate），都在間接相依套件（`ws`、`picomatch`、`nanoid` 等）。Hexo 另有 8.1.2 修補版。
  - 影響：這些套件只在本機 build 時執行，線上網站是靜態檔案，實際風險低。
  - 做法：`npm audit fix`（不需要 `--force`），再把 Hexo 升到 8.1.2；完成後重新 build 確認，並 commit 更新後的 `package-lock.json`。

- [ ] **4. Node 20 已停止維護**
  - 現況：Node 20 於 2026-04-30 結束支援，不再有安全性更新。
  - 做法：把 `.nvmrc` 改成 22（本機已安裝 v22.14.0，支援到 2027-04），或改用 Node 24（支援到 2028-04）。換版本後重新 build 確認。

### 二、功能與 SEO

- [ ] **5. 新增標籤總覽頁**
  - 現況：`/blog/tags/` 是 404，選單沒有「標籤」。
  - 做法：`npx hexo new page tags`，在產生的 `source/tags/index.md` front matter 加上 `type: tags`；再把 `themes/next/_config.yml` 選單裡的 `#tags: /tags/ || fa fa-tags` 取消註解。

- [ ] **6. 填寫網站描述**
  - 現況：`_config.yml` 的 `description`、`subtitle` 都是空的，網頁沒有 `<meta name="description">`，搜尋結果與社群分享的預覽較陽春。
  - 做法：填入網站簡介與副標題。

- [ ] **7. 加入 RSS 與 sitemap**
  - 現況：沒有 `atom.xml` 和 `sitemap.xml`。
  - 做法：安裝 `hexo-generator-feed`、`hexo-generator-sitemap`。

- [ ] **8. 程式碼區塊加上語言標示**
  - 現況：文章中 6 個程式碼區塊都沒標語言，顯示成純文字，沒有語法上色。
  - 做法：在 ``` 後面加上語言，例如 `bash`、`yaml`。

- [ ] **9. 個人化資訊**（選擇性）
  - 頭像、社群連結、「關於」頁都尚未設定。

### 三、長期維護

- [ ] **10. 主題換成 NexT 8**
  - 現況：`theme-next/hexo-theme-next` 已封存，7.8.0 是最後一版；後續由 `next-theme/hexo-theme-next`（v8.x）維護。
  - 做法：改用 NexT 8，並把客製設定放到根目錄的 `_config.next.yml`，與主題本體分開，之後升級才不會蓋掉設定。工作量最大的一項。

- [ ] **11. 改用 GitHub Actions 自動部署**
  - 好處：push `master` 就自動部署，不需要在本機執行 `hexo deploy`，也沒有帳密與 GPG 的問題；build 環境依 `.nvmrc` 與 `package-lock.json` 固定。
  - 需要手動做的事：GitHub repo 的 Settings > Pages > Source 改成「GitHub Actions」。
  - 完成後：`gh-pages` 分支與 `hexo-deployer-git` 套件可以移除。

- [ ] **12. `package.json` 加上 `scripts`**
  - 例如 `npm run server`、`npm run deploy`，不用每次輸入 `npx hexo ...`。

- [ ] **13. 刪除沒有作用的設定**
  - `_config.yml` 的 `external_link.enable: true` 寫成單一個帶點的 key，Hexo 不會讀取；預設值本來就是開啟，直接刪除即可。

- [ ] **14. 整理 `開張啦！.md`**
  - front matter 少了開頭的 `---`（Hexo 可接受，但與其他文章不一致），且目前沒有內文。
  - 不建議改檔名或網址格式，舊連結會失效。

- [ ] **15. 更新舊文章內容**（選擇性）
  - 文章都是 2016 年寫的。例如 nvm 官方已不支援用 Homebrew 安裝；`help.github.com` 已搬到 `docs.github.com`。

- [ ] **16. 調整 repo 預設分支**
  - 現況：預設分支是 `gh-pages`，打開 repo 看到的是 build 出來的檔案。
  - 做法：在 GitHub 把預設分支改成 `master`，不影響網站發布。
