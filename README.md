# 網站健康檢查

自動監控和田、棒棒貓等網站，掛掉時寄 email 通知。

## 監控中的網站

| 網站 | 網址 |
|---|---|
| 和田官網（正式網域） | https://wada.co.za |
| 和田官網（Vercel） | https://wada-kaohsiung.vercel.app |
| 棒棒貓官網 | https://bangbangcat-kaohsiung.vercel.app |
| 和田線上訂位 | https://wadatable.vercel.app |
| 高雄貓咪地圖 | https://kaohsiung-cat-map.vercel.app |

## 運作方式

每 5 分鐘檢查一次（GitHub 排程有時會延遲幾分鐘，屬正常現象）。

檢查的不只是「有沒有回應」，還包含：

1. HTTP 狀態碼必須是 **200**
2. 頁面內容**不能少於 500 bytes**（擋掉空白頁）
3. 內容**不能含有停權訊息**（`usage_exceeded`、`Service Unavailable` 等）

第 3 點是針對 2026-09-11 那次 Netlify 停權事件加的 —— 當時網站回傳的是錯誤 JSON，
單純檢查「有沒有回應」是抓不到的。

每個網址失敗會**重試 2 次**，避免網路抖動造成誤報。

## 收到通知後怎麼辦

網站掛掉時會自動開一張 issue，GitHub 會寄 email 到你的信箱。
網站恢復後 issue 會自動關閉，並留言記錄恢復時間。

## 要增加或移除監控網站

編輯 [.github/workflows/uptime.yml](.github/workflows/uptime.yml)，
找到 `SITES="` 那一段，一行加一個網址即可。

## 手動測試

到 GitHub 的 Actions 分頁 → 選「網站健康檢查」→ 按 **Run workflow** 可立即執行一次。
