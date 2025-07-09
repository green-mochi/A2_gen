# A2_gen

此工具可根據 Ultralight 或 NTAG 類型 NFC 卡的 7-byte UID，自動生成對應的 A2 連續寫入指令。適用於可修改 UID 的測試卡（如 Magic NTAG、Ultralight C clone 等），協助快速部署模擬卡片用途。

## 🔍 背景說明

NFC Ultralight 類型卡片的 UID 由 7 bytes 組成，分別儲存在前 3 頁 Page 00h ~ 02h 中，每頁各 4 bytes，共計 12 bytes，如下：

```
Page 00h: CT, UID0, UID1, UID2
Page 01h: UID3, UID4, UID5, UID6
Page 02h: BCC1, Internal, Lock0, Lock1
```

- **CT (0x88)** 表示 UID 為多段式（Cascade Tag）
- **UID[0] ~ UID[6]** 為實際卡號
- **BCC0 / BCC1** 為校驗碼，需根據 XOR 計算補上，錯誤會導致卡片識別失敗或變磚

👉 本工具會根據輸入的 UID，自動計算 BCC 並產生連續的 A2 指令：

```
A2:00:UID0:UID1:UID2:BCC0
A2:01:UID3:UID4:UID5:UID6
A2:02:BCC1:00:00:00
```

⚠️ **請務必一次性連續發送三條指令，不可逐條執行，否則卡片可能損壞無法恢復（變磚）**

## 🖥 使用方式

1. 開啟 [網頁工具](https://green-mochi.github.io/A2_gen/)
2. 輸入 7-byte UID，例如：`33 9e 0c 39 00 00 80`
3. 工具將即時產生如下指令：
   
```
A2:00:33:9E:0C:29,A2:01:39:00:00:80,A2:02:B9:00:00:00
```

4. 一鍵複製後可填入NFC Tools > OTHER > Advanced NFC commands 進行寫卡

## 💡 技術細節

- 支援深色與淺色主題切換
- 使用原生 JavaScript 前端渲染與 UID 驗證
- BCC 計算方式為 XOR 校驗
- 完整支援中文提示與使用說明

## ⚠️ 注意事項

- 本工具僅用於合法用途，如學術研究、教學、測試等
- 嚴禁用於未經授權的實體卡模擬、資料竄改或破解行為
- 若使用錯誤指令可能導致卡片永久損壞，請小心操作

## 📄 License

GPLv3.0
