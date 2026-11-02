# LINE 圖文邀約（LIFF）

在手機 LINE 裡填好邀約內容 → 按「選擇好友並發送」→ 在 LINE 的選人畫面勾好友 → 每個人都在和你的 1 對 1 聊天室收到同一張 Flex 圖文卡片。

- `index.html`：整個 app（靜態網頁，不需要後端）
- `config.js`：填 LIFF ID

## 本機預覽

```bash
python3 -m http.server 8787 --directory line-liff-invite
```

打開 http://localhost:8787 可以看畫面和卡片預覽。沒填 LIFF ID 時是預覽模式，不能發送。

## 上線步驟

1. **放到 https 網址**：LIFF 的 endpoint 必須是 https。目前部署在 GitHub Pages：`https://nudaohong.github.io/line-liff-invite/`（push 到 main 就會更新）。
2. **LINE Developers Console**（https://developers.line.biz/console/ ，用你的 LINE 帳號登入）
   1. 建立 Provider（名稱隨意，例如自己的名字）。
   2. 在 Provider 底下建立 **LINE Login** channel。
   3. 進 channel → **LIFF** 分頁 → **Add**：
      - Size：`Full`
      - Endpoint URL：步驟 1 的 https 網址
      - Scopes：勾 `profile`（shareTargetPicker 本身不需要特定 scope）
   4. 同一個 LIFF 分頁，點 **shareTargetPicker** → 勾同意條款 → **Enable**。沒開這個，按發送會顯示「這個環境不能用分享選人」。
   5. channel 一開始是 **Developing** 狀態，只有 channel 管理員和測試者能用。只有自己用就不用改；要給別人用，再把 channel 改成 **Published**。
3. 把 LIFF ID（格式像 `1234567890-AbcdEfgh`）填進 `config.js`，重新部署。
4. 手機上打開 `https://liff.line.me/<LIFF ID>`。建議把這個連結傳到自己的 Keep，之後從 LINE 裡點開，就會在 LINE 內建瀏覽器開啟。

## 限制

- **收件人每次都要在 LINE 的選人畫面手動勾**。LIFF 拿不到好友清單，也不能預先帶入名單；「我的名單」只是勾人時的對照表，存在這支手機的瀏覽器裡。
- **封面圖片要是公開的 https 網址**（JPEG / PNG）。目前不能直接選手機相簿的照片，因為照片要先上傳到某個地方，LINE 才抓得到。
- 對方收到的卡片按鈕只能是網址連結（shareTargetPicker 只允許 URI action）。
