# Christmas Slot

一款基於 Cocos Creator 3.8.6 開發的 5x3 聖誕主題老虎機遊戲。

## 🎮 線上試玩
[點擊這裡開始遊玩](https://zxl6852.github.io/Christmas_Slot_Play/)

## 🎮 遊戲特色

### 1. 基礎規則 (Base Game)
- **架構**：5 軸 x 3 行，**9 條賠付線**。
- **符號**：
  - **一般符號 (0-5)**：聖誕主題圖示。
  - **Wild (6)**：百搭符號，可替代 Bonus 與 Bullion 以外的符號。
  - **Bonus (7)**：觸發免費遊戲的關鍵。
- **中獎判定**：從最左側卷軸起，連續 3 個或以上相同符號即為中獎。
- **【符號賠率表 (此倍率 x 總押注)】**：
  - **( 5連線 / 4連線 / 3連線 )**
  - **★ Wild水晶球**：500 / 50 / 10
  - **★ 馴鹿金塊**：100 / 25 / 5 (基礎值)
  - **★ 聖誕樹**：100 / 25 / 5
  - **★ 星星**：50 / 10 / 2
  - **★ 襪子**：30 / 8 / 1.5
  - **★ 禮物**：20 / 5 / 1
  - **★ 鈴鐺,拐杖糖**：10 / 2 / 0.5
  - **★ Bonus聖誕老人**：20 / 5 / 1

### 2. 免費遊戲 (Free Game)
- **觸發方式**：在賠付線上連線 3 個或以上 `Bonus` 符號，獲得 15 次免費旋轉。
- **特色機制 - Bullion Rain (金條雨)**：
  - 進入 Free Game 前的轉場動畫。
  - 金條以拋物線軌跡飛入，隨機替換盤面 6~10 個符號為 `Bullion`。
  - 部分金條作為裝飾飛向隨機位置消失，增加視覺豐富度。
- **特色符號 - Bullion (8)**：
  - 僅在 Free Game 出現。
  - 擁有特殊的連線倍率公式：`基礎倍率 x 2 x 觸發倍數`。
- **無限加局**：Free Game 期間，盤面出現任意 `Bonus` 符號，每個增加 +1 次旋轉。

### 3. 系統功能
- **設定選單**：
  - **音效頁籤**：獨立控制背景音樂 (BGM) 與音效 (SFX) 音量。
  - **說明頁籤**：詳細的動態遊戲規則說明。
- **效能優化**：
  - 支援 **Sprite Atlas (合圖)**，減少 Draw Calls。
  - 使用 **Object Pool** 管理節點。

---

## 🛠️ 技術架構

本專案採用 **MVC (Model-View-Controller)** 架構，並使用 **EventBus** 進行解耦。

### 核心腳本 (`assets/scripts/`)

| 類別 | 腳本 | 職責 |
| :--- | :--- | :--- |
| **Model** | `SlotModel.ts` | 核心數據與邏輯。管理輸贏計算 (Win Calculation)、賠付線判定、Free Game 狀態、錢包餘額。 |
| **View** | `SlotView.ts` | 視覺呈現。負責轉輪動畫 (Spin/Stop)、符號生成、Atlas 資源管理、連線繪製。 |
| **Controller** | `GameController.ts` | 遊戲流程控制。處理 UI 交互、協調 Model 與 View、播放過場動畫 (Bullion Rain, Coin Fly)、音效管理。 |
| **View** | `SymbolView.ts` | 單個符號的顯示邏輯，支援一般的 SpriteFrame 切換。 |
| **Core** | `EventBus.ts` | 全局事件總線，用於跨組件通訊 (如 Spin Start, Win Changed)。 |
| **Controller** | `AudioController.ts`| 音效管理器，封裝 BGM/SFX 的播放與音量控制。 |

---

## ⚙️ 設定與使用 (Setup)

### 1. 資源綁定 (Cocos Inspector)

#### GameController
- **Settings UI**: 綁定 `Settings Btn`, `Settings Page`, `Settings Frame`, `Music/Effects Sliders`, `Directions Page` 等節點。
- **Game Objects**: 綁定 `Coin Sprite` (金幣袋), `Reel Mask Node` (金條雨父節點)。

#### SlotView
- **Symbol Atlas**: 為了效能，建議將符號圖片打包成 `.plist` 合圖並拖入此欄位。
- **Atlas Prefix**: 設定合圖中圖片的前綴 (預設 `symbol_`)。

#### AudioController
- 綁定對應的 `AudioClip` (BGM, Spin, Win, CoinIn, FG Trigger 等)。

### 2. 圖片命名規範
若使用 Atlas，建議符號命名如下：
- `symbol_0`, `symbol_1`, ... `symbol_5` (一般)
- `symbol_6` (Wild)
- `symbol_7` (Bonus)
- `symbol_8` (Bullion)

---

## 📝 開發筆記
- **賠付線修改**：如需修改連線規則，請編輯 `SlotModel.ts` 中的 `PAY_LINES` 陣列。
- **機率調整**：轉輪帶位於 `SlotModel.ts` 的 `NORMAL_STRIPS` 和 `FREE_GAME_STRIPS`。

---

**Merry Christmas & Happy Coding!** 🎄
