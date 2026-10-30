# AI Lockdown（STM32G474RE 微算機期中專題）

AI Lockdown 是一個以 STM32G474RE 為核心的互動式問答闖關系統。故事設定為 AI 系統發生叛變，玩家必須在 30 秒倒數時間內回答問題，成功解除控制中心的鎖定，否則系統將進入緊急狀態。

## 1. 情境故事

某天，實驗室的 AI 系統突然失去控制，並鎖定整個控制中心。工程師必須在有限時間內回答系統提出的微算機相關問題，才能重新取得控制權。

玩家需要在 30 秒內完成三道隨機題目，若全部答對即可解除封鎖；若答錯或時間結束，系統便會進入緊急狀態。

## 2. 替身對照

本專題將 AI 控制中心的故事情境轉換成實際硬體與電腦畫面：

- AI 控制中心 → STM32G474RE 開發板
- 系統警告 → 黃色 LED
- 緊急狀態 → 紅色 LED
- 正常狀態 → 綠色 LED
- 玩家操作 → 麵包板按鈕與電腦遊戲介面
- 系統通訊 → UART 序列埠傳輸

## 3. 功能與驗收條件

- 三色 LED 顯示系統狀態。
- 綠燈代表正常，黃燈代表警告，紅燈代表緊急。
- 使用按鈕觸發 EXTI 外部中斷。
- 使用 Timer 進行 30 秒倒數。
- 紅色 LED 使用 PWM 控制閃爍效果。
- 電腦程式隨機抽選 3 道題目進行問答。
- 透過 UART 傳送題目答案及遊戲狀態。
- 全部答對可解除鎖定，答錯或逾時則進入緊急狀態。

## 4. 用到的技術

- GPIO：控制 LED 與讀取按鈕。
- EXTI：按鈕外部中斷。
- Timer：倒數計時。
- PWM：控制 LED 亮度與閃爍效果。
- UART：STM32 與電腦程式之間的序列通訊。
- Memory Mapping：透過暫存器與周邊記憶體映射了解硬體控制方式。

## 5. 腳位表

| STM32 腳位 | 功能 |
|---|---|
| PA5 | 黃色 LED（TIM2_CH1） |
| PA6 | 紅色 LED（TIM3_CH1） |
| PA7 | 綠色 LED（TIM3_CH2） |
| PA8 | 按鈕 A |
| PA9 | 按鈕 B |
| PA10 | 按鈕 C |
| PA2 | USART2_TX |
| PA3 | USART2_RX |

LED 使用限流電阻，按鈕使用 GPIO 上拉輸入。實際接線以開發板與 CubeMX 設定為準。

## 6. 建置與執行

### 硬體
- NUCLEO-G474RE 開發板
- 麵包板
- 紅、黃、綠 LED
- 按鈕與限流電阻
- USB 連接線

### 軟體
- STM32CubeMX
- STM32CubeIDE
- C 語言
- Windows 10/11
- MSYS2 UCRT64
- UART 序列埠通訊

STM32 程式使用 STM32CubeIDE 編譯與燒錄；電腦端使用 C 語言建立問答介面，透過 UART 與開發板通訊。

UART 通訊設定：115200 baud、8 data bits、No parity、1 stop bit（115200 8N1）。

## 7. 程式結構

- Core/Inc：程式標頭檔。
- Core/Src/main.c：主要控制程式。
- Drivers：STM32 HAL 與 CMSIS 驅動程式。
- AI_Lockdown_STM32.ioc：STM32CubeMX 專案設定檔。
- STM32G474RETX_FLASH.ld：Flash 記憶體配置。
- STM32G474RETX_RAM.ld：RAM 記憶體配置。

主要程式負責 LED 狀態控制、按鈕中斷、倒數計時、PWM 與 UART 通訊。

## 8. 實作照片

專題實際接線與執行畫面：

![開發板接線照片](docs/board.jpg)

![專題執行畫面](docs/game.jpg)

## 9. AI 使用說明

本專題使用 AI 協助整理 STM32 HAL 程式架構、GPIO、Timer、PWM、EXTI 與 UART 的使用方式，也協助排查程式編譯錯誤及整理專題文件。

程式仍由我們依照實際硬體接線與功能需求進行修改、編譯及測試，並確認開發板與電腦之間的通訊結果。

## 10. 已知限制

- 系統需要透過 USB 與電腦連接才能進行完整問答操作。
- UART 通訊需要使用正確的 COM Port。
- 按鈕可能受到機械彈跳影響，需要適當的防彈跳處理。
- 倒數時間與問答流程需要配合電腦端及 STM32 端的通訊狀態。
- 若未來繼續改進，可以增加題庫數量、遊戲關卡及更多系統狀態。

---

**專題名稱：AI Lockdown**

**硬體平台：NUCLEO-G474RE**

**GitHub：https://github.com/kennyz99/AI_Lockdown**
